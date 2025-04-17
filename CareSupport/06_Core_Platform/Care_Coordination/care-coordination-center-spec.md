# Care Coordination Center Specification

**Version:** 1.0.0  
**Last Updated:** 2025-04-14  
**Status:** Draft  
**Document Location:** `/06_Core_Platform/Care_Coordination/Care_Coordination_Specification.md`

## 1. Overview

### 1.1 Purpose

The Care Coordination Center serves as the central hub for the CareSupport platform, providing a unified experience for monitoring, managing, and coordinating daily care activities. This module integrates schedule management, medication tracking, health monitoring, and status updates into a cohesive dashboard that adapts to the unique needs and responsibilities of each role. The Care Coordination Center transforms isolated care tasks into a comprehensive, synchronized care experience that reduces cognitive load while improving care quality and communication.

### 1.2 Related Documents

- `/06_Core_Platform/Post_Setup_Transition/Welcome_Home_Specification.md` - Post-setup experience
- `/06_Core_Platform/Task_Management/Task_Management_Specification.md` - Task system integration
- `/02_Research/Relationship_Model.md` - Role relationships and interaction patterns
- `/03_Information_Architecture/Navigation_System.md` - Navigation framework
- `/07_Technical/Data_Models/data-model-extensions.md` - Core data structures
- `/07_Technical/API_Requirements/Core_API_Specs.md` - API endpoints
- `/07_Technical/Implementation_Guides/Permission_Framework.md` - Permission implementation
- `/07_Technical/Implementation_Guides/State_Management_System.md` - State management approach

### 1.3 Module Goals

- Provide a comprehensive, role-adapted dashboard experience for care coordination
- Integrate schedule, medication, health data, and status updates in one cohesive interface
- Reduce cognitive load for caregivers through intelligent organization and prioritization
- Support care recipient agency through appropriate visibility and control mechanisms
- Enable effective team coordination while respecting privacy boundaries
- Create clear awareness of care recipient status and needs across the care circle

## 2. Core Components

### 2.1 Dashboard Components

#### 2.1.1 Purpose
Provides a unified, role-appropriate view of the most relevant care information, serving as the primary landing experience and coordination hub.

#### 2.1.2 Technical Specifications

| Attribute | Specification |
|-----------|---------------|
| **View Types** | Role-specific layouts, Customizable layouts (Phase 2) |
| **Update Frequency** | Real-time for status, 5-minute sync for other data |
| **Data Sources** | Tasks, Schedule, Medications, Health Records, Status Updates |
| **Personalization** | Role-based defaults, Individual preferences (Phase 2) |
| **State Persistence** | Expanded/collapsed sections, Selected filters |

#### 2.1.3 Component Structure

```json
{
  "dashboard": {
    "sections": {
      "care_recipient_status": {
        "priority": "high",
        "default_collapsed": false,
        "refresh_rate": "real-time",
        "data_sources": ["health_records", "status_updates", "medications"]
      },
      "todays_schedule": {
        "priority": "high",
        "default_collapsed": false,
        "refresh_rate": "5min",
        "data_sources": ["events", "tasks"]
      },
      "medication_tracker": {
        "priority": "medium",
        "default_collapsed": false,
        "refresh_rate": "5min",
        "data_sources": ["medications", "medication_logs"]
      },
      "quick_tasks": {
        "priority": "medium",
        "default_collapsed": false,
        "refresh_rate": "5min",
        "data_sources": ["tasks"]
      },
      "recent_updates": {
        "priority": "low",
        "default_collapsed": true,
        "refresh_rate": "real-time",
        "data_sources": ["updates", "status_changes"]
      },
      "care_team": {
        "priority": "low",
        "default_collapsed": true,
        "refresh_rate": "15min",
        "data_sources": ["circle_members", "member_status"]
      }
    }
  }
}
```

#### 2.1.4 Role Adaptations

| Role | Dashboard Focus | Primary Sections | Secondary Sections | Special Features |
|------|----------------|------------------|-------------------|------------------|
| **Caregiver** | Care recipient wellbeing, Daily coordination | Care Recipient Status, Today's Schedule, Medication Tracker | Quick Tasks, Recent Updates, Care Team | Comprehensive care view, Critical alerts |
| **Care Recipient** | Daily plan, Personal autonomy | Today's Plan, My Needs, Privacy Center | Upcoming Visits, My Health, Messages | Simplified layout, Privacy controls, Need expression |
| **Organizer** | Team coordination, Schedule management | Team Overview, Today's Schedule, Task Board | Care Updates, Conflict Alerts, Resource Center | Team status indicators, Conflict resolution tools |
| **Supporter** | Current commitments, Opportunity awareness | My Tasks, Availability Status, Upcoming Commitments | Care Updates, Quick Actions | Simple focused view, Availability toggle |

#### 2.1.5 Implementation Example

```jsx
// Dashboard component with role adaptations
function CareCoordinationDashboard({ userRole }) {
  // Get role-specific configuration
  const config = useDashboardConfig(userRole);
  const { sections } = config;
  
  // Load dashboard data with relevant permissions
  const { dashboardData, loading, error } = useDashboardData();
  
  // Track expanded/collapsed state
  const [expandedSections, setExpandedSections] = useState(
    Object.keys(sections).reduce((acc, key) => {
      acc[key] = !sections[key].default_collapsed;
      return acc;
    }, {})
  );
  
  // Handle toggling sections
  const toggleSection = (sectionId) => {
    setExpandedSections({
      ...expandedSections,
      [sectionId]: !expandedSections[sectionId]
    });
  };
  
  if (loading) return <LoadingDashboard />;
  if (error) return <ErrorDisplay error={error} />;
  
  return (
    <div className="care-coordination-dashboard">
      <header className="dashboard-header">
        <h1>{config.title}</h1>
        <div className="dashboard-actions">
          {config.showRefreshButton && (
            <RefreshButton onRefresh={() => refetchDashboardData()} />
          )}
          {config.showCustomizeButton && (
            <CustomizeButton onClick={() => openCustomizeModal()} />
          )}
        </div>
      </header>
      
      <div className="dashboard-content">
        {/* Render sections in priority order */}
        {Object.entries(sections)
          .sort((a, b) => getPriorityValue(a[1].priority) - getPriorityValue(b[1].priority))
          .map(([sectionId, sectionConfig]) => {
            // Skip sections that aren't enabled for this role
            if (!sectionConfig.enabled) return null;
            
            // Get component for this section
            const SectionComponent = getSectionComponent(sectionId, userRole);
            
            return (
              <DashboardSection
                key={sectionId}
                title={sectionConfig.title}
                expanded={expandedSections[sectionId]}
                onToggle={() => toggleSection(sectionId)}
                priority={sectionConfig.priority}
              >
                <SectionComponent 
                  data={dashboardData[sectionId]}
                  userRole={userRole}
                />
              </DashboardSection>
            );
          })
        }
      </div>
      
      {/* Role-specific footer content */}
      <footer className="dashboard-footer">
        {userRole === 'caregiver' && <CaregiverResources />}
        {userRole === 'care_recipient' && <PrivacyStatus />}
        {userRole === 'organizer' && <TeamStatus />}
        {userRole === 'supporter' && <AvailabilityToggle />}
        
        <LastUpdated timestamp={dashboardData.lastUpdated} />
      </footer>
    </div>
  );
}

// Example section component for Care Recipient Status
function CareRecipientStatus({ data, userRole }) {
  const { careRecipient, healthStatus, recentUpdates } = data;
  
  // Adapt display based on role
  const statusDetail = getStatusDetailLevel(userRole);
  
  return (
    <div className="care-recipient-status">
      <div className="status-header">
        <UserAvatar user={careRecipient} size="large" />
        <div className="status-overview">
          <h3>{careRecipient.firstName}'s Status</h3>
          <StatusIndicator status={healthStatus.overall} />
          <LastUpdatedTime timestamp={healthStatus.lastUpdated} />
        </div>
        
        {/* Quick action buttons - role specific */}
        {(userRole === 'caregiver' || userRole === 'organizer') && (
          <div className="status-actions">
            <Button onClick={() => recordUpdate()}>Record Update</Button>
            <Button onClick={() => checkIn()}>Check In</Button>
          </div>
        )}
      </div>
      
      {/* Status metrics - detail level varies by role */}
      <div className="status-metrics">
        {statusDetail === 'detailed' && (
          <>
            <MetricCard 
              title="Medications" 
              value={`${healthStatus.medications.taken}/${healthStatus.medications.total}`}
              status={healthStatus.medications.status}
            />
            <MetricCard 
              title="Mood" 
              value={healthStatus.mood.value}
              status={healthStatus.mood.status}
            />
            <MetricCard 
              title="Activity" 
              value={healthStatus.activity.value}
              status={healthStatus.activity.status}
            />
            <MetricCard 
              title="Nutrition" 
              value={healthStatus.nutrition.value}
              status={healthStatus.nutrition.status}
            />
          </>
        )}
        
        {statusDetail === 'moderate' && (
          <>
            <MetricCard 
              title="Medications" 
              value={`${healthStatus.medications.taken}/${healthStatus.medications.total}`}
              status={healthStatus.medications.status}
            />
            <MetricCard 
              title="Overall" 
              value={healthStatus.overall.value}
              status={healthStatus.overall.status}
            />
          </>
        )}
        
        {statusDetail === 'limited' && (
          <MetricCard 
            title="Overall" 
            value={healthStatus.overall.value}
            status={healthStatus.overall.status}
          />
        )}
      </div>
      
      {/* Recent updates - only shown for some roles */}
      {(statusDetail === 'detailed' || statusDetail === 'moderate') && (
        <div className="recent-status-updates">
          <h4>Recent Updates</h4>
          <UpdatesList updates={recentUpdates} limit={3} />
          {recentUpdates.length > 3 && (
            <Button link onClick={() => viewAllUpdates()}>
              View All Updates
            </Button>
          )}
        </div>
      )}
    </div>
  );
}
```

### 2.2 Schedule Management Components

#### 2.2.1 Purpose
Provides a comprehensive view of care-related events, appointments, and time-based tasks, enabling effective coordination across the care circle while respecting role boundaries.

#### 2.2.2 Technical Specifications

| Attribute | Specification |
|-----------|---------------|
| **View Types** | Day, Week, Month, Agenda |
| **Event Categories** | Medical, Medication, Personal Care, Social, Transportation |
| **Recurrence Options** | One-time, Daily, Weekly, Monthly, Custom |
| **Conflict Detection** | Schedule overlaps, Availability conflicts, Travel buffer checking |
| **Calendar Integration** | Google Calendar, Apple Calendar, Microsoft Outlook (Phase 2) |

#### 2.2.3 Component Structure

```json
{
  "schedule": {
    "view_types": ["day", "week", "month", "agenda"],
    "event_categories": {
      "medical": {
        "color": "#E57373",
        "icon": "medical",
        "default_duration": 60
      },
      "medication": {
        "color": "#64B5F6",
        "icon": "pill",
        "default_duration": 15
      },
      "personal_care": {
        "color": "#81C784",
        "icon": "care",
        "default_duration": 30
      },
      "social": {
        "color": "#FFD54F",
        "icon": "social",
        "default_duration": 60
      },
      "transportation": {
        "color": "#9575CD",
        "icon": "transportation",
        "default_duration": 45
      }
    },
    "conflict_rules": {
      "buffer_before": 15,
      "buffer_after": 15,
      "travel_time_default": 30,
      "availability_check": true
    }
  }
}
```

#### 2.2.4 Role Adaptations

| Role | Default View | Creation Capabilities | Event Display | Special Features |
|------|--------------|----------------------|----------------|-----------------|
| **Caregiver** | Week or Day | All event types | Full details | Schedule optimization, Conflict resolution |
| **Care Recipient** | Agenda or Day | Personal events only | Simplified view | Privacy controls, Input on preferences |
| **Organizer** | Week (team view) | All event types | Coordination focus | Team availability view, Conflict detection |
| **Supporter** | Agenda | None (view only) | Assigned events only | Availability integration, Commitment tracking |

#### 2.2.5 Implementation Example

```jsx
// Schedule component with role adaptations
function ScheduleManager({ userRole }) {
  // Get role-specific configuration
  const config = useScheduleConfig(userRole);
  
  // State for current view and date
  const [currentView, setCurrentView] = useState(config.defaultView);
  const [currentDate, setCurrentDate] = useState(new Date());
  const [selectedEvent, setSelectedEvent] = useState(null);
  
  // Load events with permission filtering
  const { events, loading, error } = useScheduleEvents(currentView, currentDate);
  
  // Handle event selection
  const handleEventClick = (event) => {
    setSelectedEvent(event);
  };
  
  // Handle event creation - only for roles with permission
  const handleCreateEvent = () => {
    if (config.canCreateEvents) {
      openEventCreationModal();
    }
  };
  
  // Handle view change
  const handleViewChange = (newView) => {
    setCurrentView(newView);
  };
  
  // Handle date navigation
  const handleDateChange = (newDate) => {
    setCurrentDate(newDate);
  };
  
  if (loading) return <LoadingSchedule />;
  if (error) return <ErrorDisplay error={error} />;
  
  return (
    <div className="schedule-manager">
      <header className="schedule-header">
        <h2>{config.title}</h2>
        
        <div className="schedule-controls">
          {/* Date navigation */}
          <DateNavigation 
            currentDate={currentDate}
            onDateChange={handleDateChange}
            view={currentView}
          />
          
          {/* View selector - only show views available for this role */}
          <ViewSelector
            currentView={currentView}
            availableViews={config.availableViews}
            onViewChange={handleViewChange}
          />
          
          {/* Create button - only shown for roles with permission */}
          {config.canCreateEvents && (
            <Button primary onClick={handleCreateEvent}>
              {config.createButtonText}
            </Button>
          )}
          
          {/* Additional controls for some roles */}
          {userRole === 'organizer' && (
            <TeamFilter 
              onFilterChange={handleTeamFilterChange} 
            />
          )}
          
          {userRole === 'care_recipient' && (
            <PrivacyControl 
              onPrivacyChange={handlePrivacyChange} 
            />
          )}
        </div>
      </header>
      
      {/* Calendar view - adapts to role and current view */}
      <div className="schedule-calendar">
        {currentView === 'day' && (
          <DayView 
            date={currentDate}
            events={events}
            onEventClick={handleEventClick}
            userRole={userRole}
          />
        )}
        
        {currentView === 'week' && (
          <WeekView 
            date={currentDate}
            events={events}
            onEventClick={handleEventClick}
            userRole={userRole}
          />
        )}
        
        {currentView === 'month' && (
          <MonthView 
            date={currentDate}
            events={events}
            onEventClick={handleEventClick}
            userRole={userRole}
          />
        )}
        
        {currentView === 'agenda' && (
          <AgendaView 
            date={currentDate}
            events={events}
            onEventClick={handleEventClick}
            userRole={userRole}
          />
        )}
        
        {/* Team view for organizers */}
        {currentView === 'team' && userRole === 'organizer' && (
          <TeamView 
            date={currentDate}
            events={events}
            onEventClick={handleEventClick}
          />
        )}
      </div>
      
      {/* Event details shown when event is selected */}
      {selectedEvent && (
        <EventDetails 
          event={selectedEvent}
          userRole={userRole}
          onClose={() => setSelectedEvent(null)}
          onEdit={config.canEditEvents ? handleEditEvent : undefined}
          onDelete={config.canDeleteEvents ? handleDeleteEvent : undefined}
        />
      )}
      
      {/* Conflict detection alerts - primarily for caregiver/organizer */}
      {(userRole === 'caregiver' || userRole === 'organizer') && 
       events.conflicts && events.conflicts.length > 0 && (
        <ConflictAlerts 
          conflicts={events.conflicts}
          onResolve={handleResolveConflict}
        />
      )}
      
      {/* Role-specific footer content */}
      <footer className="schedule-footer">
        {userRole === 'caregiver' && (
          <EventSummary events={events} />
        )}
        
        {userRole === 'organizer' && (
          <TeamAvailabilitySummary />
        )}
        
        {userRole === 'supporter' && (
          <AvailabilityPrompt />
        )}
      </footer>
    </div>
  );
}
```

### 2.3 Medication Management Components

#### 2.3.1 Purpose
Enables tracking, administration, and monitoring of medications with appropriate role adaptations that respect both medical needs and privacy considerations.

#### 2.3.2 Technical Specifications

| Attribute | Specification |
|-----------|---------------|
| **Tracking Types** | Schedule-based, As-needed (PRN), Condition-based |
| **Administration Records** | Taken, Skipped, Delayed, Modified dosage |
| **Reminder Types** | Time-based, Location-based, Caregiver assistance |
| **Supply Tracking** | Current count, Days remaining, Refill alerts |
| **Privacy Levels** | Medication names, Purpose, History, Adherence data |

#### 2.3.3 Component Structure

```json
{
  "medication_management": {
    "medication_attributes": {
      "standard": ["name", "dosage", "form", "frequency", "timing"],
      "detailed": ["prescriber", "pharmacy", "purpose", "instructions", "side_effects"],
      "monitoring": ["effectiveness", "side_effects_experienced", "adherence_rate"]
    },
    "administration_types": ["taken", "skipped", "delayed", "partial", "modified"],
    "reminder_types": {
      "time_based": {
        "options": ["exact_time", "morning", "afternoon", "evening", "night", "with_meals"]
      },
      "assistance_based": {
        "options": ["self_administered", "needs_reminder", "needs_assistance", "needs_full_administration"]
      }
    }
  }
}
```

#### 2.3.4 Role Adaptations

| Role | View Capabilities | Administration | Monitoring | Special Features |
|------|------------------|----------------|------------|-----------------|
| **Caregiver** | Complete medication details | Record for others, Verify | Full adherence data | Refill management, Side effect tracking |
| **Care Recipient** | Own medications, Privacy control | Self-recording, Request help | Personal adherence | Privacy settings, Medication education |
| **Organizer** | Basic list, Schedule impact | None (unless assigned) | Schedule adherence only | Integration with calendar, Task creation |
| **Supporter** | Task-specific only | Record if assigned | Task completion only | Simple instructions, Limited scope |

#### 2.3.5 Implementation Example

```jsx
// Medication management component with role adaptations
function MedicationManager({ userRole }) {
  // Get role-specific configuration
  const config = useMedicationConfig(userRole);
  
  // State for current view
  const [currentView, setCurrentView] = useState(config.defaultView);
  const [selectedMedication, setSelectedMedication] = useState(null);
  
  // Load medications with permission filtering
  const { medications, loading, error } = useMedications();
  
  // Handle medication selection
  const handleMedicationSelect = (medication) => {
    setSelectedMedication(medication);
  };
  
  // Handle medication administration recording
  const handleRecordAdministration = (medicationId, status, details) => {
    if (config.canRecordAdministration) {
      recordMedicationAdministration(medicationId, status, details);
    }
  };
  
  // Handle new medication creation
  const handleCreateMedication = () => {
    if (config.canCreateMedications) {
      openMedicationCreationModal();
    }
  };
  
  if (loading) return <LoadingMedications />;
  if (error) return <ErrorDisplay error={error} />;
  
  return (
    <div className="medication-manager">
      <header className="medication-header">
        <h2>{config.title}</h2>
        
        <div className="medication-controls">
          {/* View selector - only if multiple views available */}
          {config.availableViews.length > 1 && (
            <ViewSelector
              currentView={currentView}
              availableViews={config.availableViews}
              onViewChange={setCurrentView}
            />
          )}
          
          {/* Create button - only shown for roles with permission */}
          {config.canCreateMedications && (
            <Button primary onClick={handleCreateMedication}>
              {config.createButtonText}
            </Button>
          )}
          
          {/* Role-specific controls */}
          {userRole === 'caregiver' && (
            <RefillTracker medications={medications} />
          )}
          
          {userRole === 'care_recipient' && (
            <PrivacyControl 
              type="medications"
              onPrivacyChange={handlePrivacyChange} 
            />
          )}
        </div>
      </header>
      
      {/* Medication list or grid view */}
      <div className="medication-list">
        {currentView === 'list' && (
          <MedicationList
            medications={medications}
            onSelect={handleMedicationSelect}
            onRecord={config.canRecordAdministration ? handleRecordAdministration : undefined}
            detailLevel={config.detailLevel}
            userRole={userRole}
          />
        )}
        
        {currentView === 'schedule' && (
          <MedicationSchedule
            medications={medications}
            onSelect={handleMedicationSelect}
            onRecord={config.canRecordAdministration ? handleRecordAdministration : undefined}
            userRole={userRole}
          />
        )}
        
        {currentView === 'grid' && (
          <MedicationGrid
            medications={medications}
            onSelect={handleMedicationSelect}
            onRecord={config.canRecordAdministration ? handleRecordAdministration : undefined}
            userRole={userRole}
          />
        )}
      </div>
      
      {/* Medication details shown when medication is selected */}
      {selectedMedication && (
        <MedicationDetails
          medication={selectedMedication}
          userRole={userRole}
          detailLevel={config.detailLevel}
          onClose={() => setSelectedMedication(null)}
          onEdit={config.canEditMedications ? handleEditMedication : undefined}
          onDelete={config.canDeleteMedications ? handleDeleteMedication : undefined}
          onRecord={config.canRecordAdministration ? handleRecordAdministration : undefined}
        />
      )}
      
      {/* Administration recording panel - shown for roles that can record */}
      {config.canRecordAdministration && selectedMedication && (
        <AdministrationPanel
          medication={selectedMedication}
          onRecord={handleRecordAdministration}
          userRole={userRole}
        />
      )}
      
      {/* Adherence summary - varies by role */}
      {config.showAdherence && (
        <AdherenceSummary
          medications={medications}
          detailLevel={config.adherenceDetailLevel}
          userRole={userRole}
        />
      )}
      
      {/* Role-specific footer content */}
      <footer className="medication-footer">
        {userRole === 'caregiver' && (
          <RefillReminders medications={medications} />
        )}
        
        {userRole === 'care_recipient' && (
          <MedicationAssistanceRequest />
        )}
        
        {userRole === 'organizer' && (
          <MedicationScheduleExport />
        )}
      </footer>
    </div>
  );
}
```

### 2.4 Health Tracking Components

#### 2.4.1 Purpose
Enables monitoring and recording of health-related information with appropriate privacy controls and role-based access levels, facilitating better care while respecting boundaries.

#### 2.4.2 Technical Specifications

| Attribute | Specification |
|-----------|---------------|
| **Data Types** | Vitals, Symptoms, Measurements, Observations, Logs |
| **Visualization Types** | Line charts, Trend indicators, Heatmaps, Summary cards |
| **Tracking Frequency** | Continuous, Daily, Weekly, Event-based |
| **Privacy Levels** | Raw data, Derived insights, Trends, General status |
| **External Integration** | Health devices, Medical APIs (Phase 2) |

#### 2.4.3 Component Structure

```json
{
  "health_tracking": {
    "metric_types": {
      "vitals": ["blood_pressure", "heart_rate", "temperature", "oxygen_saturation", "respiratory_rate"],
      "measurements": ["weight", "blood_glucose", "fluid_intake", "nutrition"],
      "observations": ["pain", "mood", "energy", "sleep", "appetite", "cognition"],
      "logs": ["symptoms", "side_effects", "behaviors", "events"]
    },
    "visualization_types": {
      "time_series": ["line", "bar", "area"],
      "summary": ["card", "gauge", "trend_indicator"],
      "distribution": ["heatmap", "histogram", "calendar"]
    },
    "data_access_levels": {
      "full": "All data with raw values",
      "detailed": "Most data with some sensitive data excluded",
      "limited": "Summary trends and status indicators only",
      "minimal": "General status only"
    }
  }
}
```

#### 2.4.4 Role Adaptations

| Role | Data Access Level | Recording Capabilities | Visualization Focus | Special Features |
|------|------------------|------------------------|---------------------|-----------------|
| **Caregiver** | Full or Detailed (per privacy) | All metrics, On behalf of recipient | Trends, Correlations, Anomalies | Health insights, Pattern detection |
| **Care Recipient** | Full (own data) | Self-reporting, Personal observations | Personal trends, Simple visualizations | Privacy controls, Education |
| **Organizer** | Limited or Minimal | None (unless assigned) | Status indicators only | Care coordination impact |
| **Supporter** | Minimal | Task-specific only | None or very limited | Simple instructions for care context |

#### 2.4.5 Implementation Example

```jsx
// Health tracking component with role adaptations
function HealthTracker({ userRole }) {
  // Get role-specific configuration
  const config = useHealthTrackerConfig(userRole);
  
  // State for current metric and timeframe
  const [currentMetric, setCurrentMetric] = useState(config.defaultMetric);
  const [timeRange, setTimeRange] = useState(config.defaultTimeRange);
  const [recordingMode, setRecordingMode] = useState(false);
  
  // Load health data with permission filtering
  const { healthData, loading, error } = useHealthData(currentMetric, timeRange);
  
  // Handle metric selection
  const handleMetricChange = (metric) => {
    setCurrentMetric(metric);
  };
  
  // Handle recording new measurement
  const handleRecordMeasurement = (metricType, value, notes) => {
    if (config.canRecordMeasurements) {
      recordHealthMeasurement(metricType, value, notes);
      setRecordingMode(false);
    }
  };
  
  // Handle entering recording mode
  const handleStartRecording = () => {
    if (config.canRecordMeasurements) {
      setRecordingMode(true);
    }
  };
  
  if (loading) return <LoadingHealthData />;
  if (error) return <ErrorDisplay error={error} />;
  
  return (
    <div className="health-tracker">
      <header className="health-tracker-header">
        <h2>{config.title}</h2>
        
        <div className="health-tracker-controls">
          {/* Metric selector - only shows metrics available to this role */}
          <MetricSelector
            metrics={config.availableMetrics}
            currentMetric={currentMetric}
            onChange={handleMetricChange}
          />
          
          {/* Time range selector */}
          <TimeRangeSelector
            current={timeRange}
            options={config.timeRangeOptions}
            onChange={setTimeRange}
          />
          
          {/* Record button - only shown for roles with permission */}
          {config.canRecordMeasurements && !recordingMode && (
            <Button primary onClick={handleStartRecording}>
              {config.recordButtonText}
            </Button>
          )}
          
          {/* Role-specific controls */}
          {userRole === 'care_recipient' && (
            <PrivacyControl 
              type="health_data"
              onPrivacyChange={handlePrivacyChange} 
            />
          )}
        </div>
      </header>
      
      {/* Recording form - shown when in recording mode */}
      {recordingMode && (
        <RecordingForm
          metric={currentMetric}
          onSubmit={handleRecordMeasurement}
          onCancel={() => setRecordingMode(false)}
          userRole={userRole}
        />
      )}
      
      {/* Health data visualization - adapts to role and metric */}
      <div className="health-data-visualization">
        {!recordingMode && (
          <>
            {/* Summary cards - shown in detail level according to role */}
            <MetricSummaryCards 
              data={healthData.summary}
              detailLevel={config.summaryDetailLevel}
            />
            
            {/* Main visualization - only certain roles see detailed charts */}
            {(config.visualizationLevel === 'detailed' || 
              config.visualizationLevel === 'limited') && (
              <MetricChartView
                data={healthData.timeSeriesData}
                metric={currentMetric}
                timeRange={timeRange}
                detailLevel={config.visualizationLevel}
              />
            )}
            
            {/* Data table - only certain roles see raw data */}
            {config.visualizationLevel === 'detailed' && (
              <MetricDataTable
                data={healthData.rawData}
                metric={currentMetric}
              />
            )}
            
            {/* Insights and correlations - only for roles with this permission */}
            {config.showInsights && (
              <HealthInsights
                insights={healthData.insights}
                detailLevel={config.insightDetailLevel}
              />
            )}
          </>
        )}
      </div>
      
      {/* History log - only shown for some roles */}
      {config.showHistory && !recordingMode && (
        <HealthHistoryLog
          history={healthData.history}
          metric={currentMetric}
          detailLevel={config.historyDetailLevel}
        />
      )}
      
      {/* Role-specific footer content */}
      <footer className="health-tracker-footer">
        {userRole === 'caregiver' && (
          <HealthAlertSettings />
        )}
        
        {userRole === 'care_recipient' && (
          <HealthEducationResources metric={currentMetric} />
        )}
      </footer>
    </div>
  );
}
```

### 2.5 Care Recipient Status Components

#### 2.5.1 Purpose
Provides a holistic, real-time view of the care recipient's wellbeing with appropriate privacy protections and role-specific detail levels, serving as the central awareness mechanism for the care circle.

#### 2.5.2 Technical Specifications

| Attribute | Specification |
|-----------|---------------|
| **Status Types** | Overall, Physical, Emotional, Social, Cognitive |
| **Update Frequency** | Real-time, Daily summary, Event-based |
| **Input Methods** | Caregiver observation, Self-reporting, Automated tracking |
| **Privacy Controls** | Granular sharing options, Detail level control |
| **Alert Thresholds** | Normal, Attention, Concern, Urgent |

#### 2.5.3 Component Structure

```json
{
  "care_recipient_status": {
    "status_categories": {
      "overall": {
        "metrics": ["general_wellbeing"],
        "inputs": ["caregiver_observation", "self_report", "derived"]
      },
      "physical": {
        "metrics": ["mobility", "pain", "sleep", "nutrition", "medication_adherence"],
        "inputs": ["caregiver_observation", "self_report", "health_data"]
      },
      "emotional": {
        "metrics": ["mood", "anxiety", "engagement"],
        "inputs": ["caregiver_observation", "self_report"]
      },
      "social": {
        "metrics": ["interaction", "connection", "participation"],
        "inputs": ["caregiver_observation", "self_report", "activity_log"]
      },
      "cognitive": {
        "metrics": ["orientation", "memory", "decision_making"],
        "inputs": ["caregiver_observation", "self_report", "assessment"]
      }
    },
    "status_levels": ["excellent", "good", "fair", "needs_attention", "concerning"],
    "update_types": ["observation", "check_in", "event", "routine", "self_report"]
  }
}
```

#### 2.5.4 Role Adaptations

| Role | Status View | Update Capabilities | Alert Level | Special Features |
|------|------------|---------------------|-------------|-----------------|
| **Caregiver** | Comprehensive with trends | Record observations, Check-ins | All alerts | Pattern recognition, History access |
| **Care Recipient** | Self view with privacy | Self-reporting, Set sharing preferences | Personal alerts | Privacy controls, Self-monitoring |
| **Organizer** | Status overview | None (coordination focus) | Important alerts only | Care coordination impact |
| **Supporter** | Task-relevant only | Task-specific observations | Urgent alerts only | Context for assigned tasks |

#### 2.5.5 Implementation Example

```jsx
// Care recipient status component with role adaptations
function CareRecipientStatusTracker({ userRole }) {
  // Get role-specific configuration
  const config = useStatusConfig(userRole);
  
  // State for current category and recording
  const [currentCategory, setCurrentCategory] = useState('overall');
  const [isRecording, setIsRecording] = useState(false);
  
  // Load status data with permission filtering
  const { statusData, loading, error } = useStatusData();
  
  // Handle category change
  const handleCategoryChange = (category) => {
    setCurrentCategory(category);
  };
  
  // Handle recording new status update
  const handleRecordUpdate = (category, status, notes) => {
    if (config.canRecordUpdates) {
      recordStatusUpdate(category, status, notes);
      setIsRecording(false);
    }
  };
  
  // Handle check-in
  const handleCheckIn = () => {
    if (config.canPerformCheckIn) {
      initiateCheckIn();
    }
  };
  
  if (loading) return <LoadingStatus />;
  if (error) return <ErrorDisplay error={error} />;
  
  return (
    <div className="care-recipient-status-tracker">
      <header className="status-tracker-header">
        <h2>{config.title}</h2>
        
        <div className="status-tracker-controls">
          {/* Category selector - only for roles with multiple categories */}
          {config.availableCategories.length > 1 && (
            <CategorySelector
              categories={config.availableCategories}
              currentCategory={currentCategory}
              onChange={handleCategoryChange}
            />
          )}
          
          {/* Update buttons - only shown for roles with permission */}
          {config.canRecordUpdates && !isRecording && (
            <Button primary onClick={() => setIsRecording(true)}>
              {config.updateButtonText}
            </Button>
          )}
          
          {config.canPerformCheckIn && (
            <Button onClick={handleCheckIn}>
              Check In
            </Button>
          )}
          
          {/* Privacy controls - only for care recipient */}
          {userRole === 'care_recipient' && (
            <PrivacyControl 
              type="status"
              onPrivacyChange={handlePrivacyChange} 
            />
          )}
        </div>
      </header>
      
      {/* Recording form - shown when in recording mode */}
      {isRecording && (
        <StatusUpdateForm
          category={currentCategory}
          onSubmit={handleRecordUpdate}
          onCancel={() => setIsRecording(false)}
          userRole={userRole}
        />
      )}
      
      {/* Overall status display - adapts to role */}
      {!isRecording && (
        <div className="status-overview">
          <CareRecipientAvatar 
            status={statusData.overall.status}
            showIndicator={true}
          />
          
          <div className="primary-status">
            <h3>{getStatusLabel(statusData.overall.status)}</h3>
            <StatusIndicator status={statusData.overall.status} size="large" />
            <LastUpdatedTime timestamp={statusData.overall.lastUpdated} />
          </div>
          
          {/* Quick actions based on role */}
          <div className="status-actions">
            {userRole === 'caregiver' && (
              <>
                <Button onClick={() => setIsRecording(true)}>Update Status</Button>
                <Button onClick={handleCheckIn}>Check In</Button>
              </>
            )}
            
            {userRole === 'care_recipient' && (
              <Button onClick={() => setIsRecording(true)}>Update My Status</Button>
            )}
            
            {userRole === 'organizer' && (
              <Button onClick={() => navigateToCoordination()}>
                View Team Status
              </Button>
            )}
          </div>
        </div>
      )}
      
      {/* Category status cards - detail level varies by role */}
      {!isRecording && config.showCategoryCards && (
        <div className="status-categories">
          {config.visibleCategories.map(category => (
            <CategoryStatusCard
              key={category}
              category={category}
              data={statusData[category]}
              detailLevel={config.detailLevel}
              isActive={currentCategory === category}
              onClick={() => handleCategoryChange(category)}
            />
          ))}
        </div>
      )}
      
      {/* Detailed category view - shown when category is selected */}
      {!isRecording && currentCategory !== 'overall' && (
        <CategoryDetailView
          category={currentCategory}
          data={statusData[currentCategory]}
          detailLevel={config.detailLevel}
          userRole={userRole}
        />
      )}
      
      {/* Recent updates - only shown for some roles */}
      {!isRecording && config.showUpdates && (
        <RecentStatusUpdates
          updates={statusData.recentUpdates}
          limit={config.updatesLimit}
          detailLevel={config.updateDetailLevel}
        />
      )}
      
      {/* Alerts section - visibility varies by role */}
      {statusData.alerts && statusData.alerts.length > 0 && 
       config.alertThreshold <= getHighestAlertLevel(statusData.alerts) && (
        <StatusAlerts
          alerts={filterAlertsByThreshold(statusData.alerts, config.alertThreshold)}
          userRole={userRole}
          onRespond={handleAlertResponse}
        />
      )}
      
      {/* Role-specific footer content */}
      <footer className="status-tracker-footer">
        {userRole === 'caregiver' && (
          <StatusHistoryLink />
        )}
        
        {userRole === 'care_recipient' && (
          <StatusSharingPreferences />
        )}
      </footer>
    </div>
  );
}
```

## 3. Technical Implementation

### 3.1 Data Model

The Care Coordination Center builds on the data model extensions defined in `/07_Technical/Data_Models/data-model-extensions.md`, with these key structures:

```json
{
  "event": {
    "id": "event123",
    "title": "Doctor Appointment",
    "description": "Quarterly checkup with Dr. Smith",
    "location": {
      "name": "St. Mary's Medical Center",
      "address": "123 Medical Plaza, Suite 400",
      "coordinates": {
        "latitude": 37.7749,
        "longitude": -122.4194
      }
    },
    "category": "medical",
    "start_time": "2025-04-20T14:00:00Z",
    "end_time": "2025-04-20T15:00:00Z",
    "all_day": false,
    "recurrence": null,
    "attendees": [
      {
        "user_id": "user123",
        "role": "caregiver",
        "status": "confirmed",
        "notification_opt_in": true
      },
      {
        "user_id": "recipient456",
        "role": "care_recipient",
        "status": "confirmed",
        "notification_opt_in": true
      }
    ],
    "created_at": "2025-04-10T09:30:00Z",
    "created_by": {
      "user_id": "user123",
      "role": "caregiver"
    },
    "care_circle_id": "circle123",
    "notes": [
      {
        "content": "Bring medication list and recent lab results",
        "author_id": "user123",
        "created_at": "2025-04-10T09:32:00Z"
      }
    ],
    "reminders": [
      {
        "time": "2025-04-20T13:00:00Z",
        "recipients": ["user123", "recipient456"],
        "sent": false
      },
      {
        "time": "2025-04-19T18:00:00Z",
        "recipients": ["user123"],
        "sent": false,
        "message": "Don't forget to prepare medication list for tomorrow's appointment"
      }
    ],
    "transportation": {
      "needed": true,
      "arranged": true,
      "provider": "user123",
      "notes": "Will drive Mom to appointment"
    },
    "privacy_level": "caregivers_only",
    "visibility_overrides": [],
    "history": [
      {
        "action": "created",
        "actor_id": "user123",
        "timestamp": "2025-04-10T09:30:00Z"
      }
    ]
  },
  
  "medication": {
    "id": "med123",
    "name": "Lisinopril",
    "generic_name": "Lisinopril",
    "strength": "10",
    "unit": "mg",
    "form": "tablet",
    "instructions": "Take one tablet daily with food",
    "prescriber": "Dr. Jane Williams",
    "pharmacy": {
      "name": "Walgreens",
      "phone": "+15551234567",
      "address": "500 Main St"
    },
    "rx_number": "RX12345678",
    "purpose": "Blood pressure management",
    "care_recipient": {
      "user_id": "recipient456"
    },
    "care_circle_id": "circle123",
    "schedule": [
      {
        "time": "08:00",
        "days": ["monday", "tuesday", "wednesday", "thursday", "friday", "saturday", "sunday"],
        "with_food": true,
        "with_water": true,
        "special_instructions": "Take with breakfast"
      }
    ],
    "supply": {
      "last_filled": "2025-04-01T00:00:00Z",
      "quantity": 30,
      "remaining": 17,
      "refills": 2,
      "next_refill_date": "2025-04-25T00:00:00Z"
    },
    "alerts": {
      "low_supply": true,
      "refill_reminder_days": 5,
      "interaction_check": true
    },
    "side_effects": [
      "Dizziness",
      "Cough"
    ],
    "images": [
      {
        "type": "pill",
        "url": "https://storage.example.com/medications/lisinopril_10mg.jpg"
      }
    ],
    "status": "active",
    "start_date": "2025-01-15T00:00:00Z",
    "end_date": null,
    "created_at": "2025-01-15T09:30:00Z",
    "created_by": {
      "user_id": "user123",
      "role": "caregiver"
    },
    "privacy_level": "caregivers_only",
    "visibility_overrides": [],
    "administration_log": [
      {
        "status": "taken",
        "scheduled_time": "2025-04-14T08:00:00Z",
        "actual_time": "2025-04-14T08:10:00Z",
        "recorded_by": {
          "user_id": "user123",
          "role": "caregiver"
        },
        "notes": "Taken with breakfast"
      }
    ],
    "history": [
      {
        "action": "created",
        "actor_id": "user123",
        "timestamp": "2025-01-15T09:30:00Z"
      }
    ]
  },
  
  "health_record": {
    "id": "health123",
    "type": "measurement",
    "metric": "blood_pressure",
    "value": {
      "systolic": 120,
      "diastolic": 80
    },
    "unit": "mmHg",
    "timestamp": "2025-04-14T09:15:00Z",
    "recorded_by": {
      "user_id": "user123",
      "role": "caregiver"
    },
    "care_recipient": {
      "user_id": "recipient456"
    },
    "care_circle_id": "circle123",
    "notes": "Morning reading, after breakfast",
    "context": {
      "location": "home",
      "position": "seated",
      "time_of_day": "morning",
      "medications_taken": true
    },
    "related_to": {
      "medication_id": "med123",
      "condition_id": "condition456"
    },
    "privacy_level": "caregivers_only",
    "visibility_overrides": [],
    "history": [
      {
        "action": "created",
        "actor_id": "user123",
        "timestamp": "2025-04-14T09:15:00Z"
      }
    ]
  },
  
  "status_update": {
    "id": "status123",
    "care_recipient": {
      "user_id": "recipient456"
    },
    "care_circle_id": "circle123",
    "timestamp": "2025-04-14T10:30:00Z",
    "recorded_by": {
      "user_id": "user123",
      "role": "caregiver"
    },
    "update_type": "observation",
    "categories": {
      "overall": {
        "status": "good",
        "note": "Generally doing well today"
      },
      "physical": {
        "status": "good",
        "metrics": {
          "mobility": "good",
          "pain": "minimal",
          "sleep": "adequate",
          "appetite": "normal"
        },
        "note": "Moving well, minimal pain"
      },
      "emotional": {
        "status": "fair",
        "metrics": {
          "mood": "slightly_low",
          "anxiety": "mild",
          "engagement": "moderate"
        },
        "note": "Seemed a bit quiet today"
      },
      "social": {
        "status": "good",
        "metrics": {
          "interaction": "engaged",
          "connection": "present"
        },
        "note": "Enjoyed visit from neighbor"
      },
      "cognitive": {
        "status": "good",
        "metrics": {
          "orientation": "oriented",
          "memory": "typical",
          "decision_making": "independent"
        },
        "note": "Normal cognition today"
      }
    },
    "privacy_level": "caregivers_only",
    "visibility_overrides": [],
    "media": [
      {
        "type": "image",
        "url": "https://storage.example.com/media/img456.jpg",
        "thumbnail_url": "https://storage.example.com/media/thumb_img456.jpg",
        "created_at": "2025-04-14T10:30:00Z"
      }
    ],
    "follow_up_needed": false,
    "history": [
      {
        "action": "created",
        "actor_id": "user123",
        "timestamp": "2025-04-14T10:30:00Z"
      }
    ]
  }
}
```

### 3.2 State Management

```javascript
// Care coordination state management (pseudocode)
const careCoordinationState = {
  // Dashboard state
  dashboard: {
    sections: {
      careRecipientStatus: {
        expanded: true,
        loading: false,
        error: null,
        data: null
      },
      todaysSchedule: {
        expanded: true,
        loading: false,
        error: null,
        data: null
      },
      medicationTracker: {
        expanded: true,
        loading: false,
        error: null,
        data: null
      },
      quickTasks: {
        expanded: true,
        loading: false,
        error: null,
        data: null
      },
      recentUpdates: {
        expanded: false,
        loading: false,
        error: null,
        data: null
      },
      careTeam: {
        expanded: false,
        loading: false,
        error: null,
        data: null
      }
    },
    lastUpdated: null
  },
  
  // Schedule state
  schedule: {
    view: 'week',
    currentDate: new Date(),
    events: {
      byId: {},
      allIds: [],
      conflicts: []
    },
    selectedEventId: null,
    filters: {
      categories: ['all'],
      attendees: ['all']
    },
    loading: false,
    error: null
  },
  
  // Medication state
  medications: {
    list: {
      byId: {},
      allIds: []
    },
    administrationLog: {
      byMedicationId: {}
    },
    view: 'list',
    selectedMedicationId: null,
    filters: {
      status: 'active',
      scheduled: 'all'
    },
    recording: {
      medicationId: null,
      status: null,
      notes: ''
    },
    loading: false,
    error: null
  },
  
  // Health tracking state
  healthTracking: {
    metrics: {
      byId: {},
      allIds: []
    },
    records: {
      byMetricId: {}
    },
    currentMetric: 'blood_pressure',
    timeRange: 'week',
    recording: {
      metricId: null,
      value: null,
      notes: ''
    },
    loading: false,
    error: null
  },
  
  // Status tracking state
  statusTracking: {
    current: {
      overall: null,
      physical: null,
      emotional: null,
      social: null,
      cognitive: null,
      lastUpdated: null
    },
    updates: {
      byId: {},
      allIds: []
    },
    alerts: [],
    currentCategory: 'overall',
    recording: {
      categoryId: null,
      status: null,
      notes: ''
    },
    loading: false,
    error: null
  }
};

// Care coordination actions (pseudocode)
const careCoordinationActions = {
  // Dashboard actions
  loadDashboard: () => async (dispatch) => {
    dispatch({ type: 'LOAD_DASHBOARD_START' });
    try {
      const data = await api.getDashboard();
      dispatch({ type: 'LOAD_DASHBOARD_SUCCESS', payload: data });
    } catch (error) {
      dispatch({ type: 'LOAD_DASHBOARD_ERROR', payload: error });
    }
  },
  
  toggleDashboardSection: (sectionId) => ({
    type: 'TOGGLE_DASHBOARD_SECTION',
    payload: sectionId
  }),
  
  // Schedule actions
  loadEvents: (view, date, filters) => async (dispatch) => {
    dispatch({ type: 'LOAD_EVENTS_START', payload: { view, date, filters } });
    try {
      const events = await api.getEvents(view, date, filters);
      dispatch({ type: 'LOAD_EVENTS_SUCCESS', payload: events });
    } catch (error) {
      dispatch({ type: 'LOAD_EVENTS_ERROR', payload: error });
    }
  },
  
  createEvent: (eventData) => async (dispatch) => {
    dispatch({ type: 'CREATE_EVENT_START', payload: eventData });
    try {
      const event = await api.createEvent(eventData);
      dispatch({ type: 'CREATE_EVENT_SUCCESS', payload: event });
      return event;
    } catch (error) {
      dispatch({ type: 'CREATE_EVENT_ERROR', payload: error });
      throw error;
    }
  },
  
  setScheduleView: (view) => ({
    type: 'SET_SCHEDULE_VIEW',
    payload: view
  }),
  
  setScheduleDate: (date) => ({
    type: 'SET_SCHEDULE_DATE',
    payload: date
  }),
  
  // Medication actions
  loadMedications: (filters) => async (dispatch) => {
    dispatch({ type: 'LOAD_MEDICATIONS_START', payload: filters });
    try {
      const medications = await api.getMedications(filters);
      dispatch({ type: 'LOAD_MEDICATIONS_SUCCESS', payload: medications });
    } catch (error) {
      dispatch({ type: 'LOAD_MEDICATIONS_ERROR', payload: error });
    }
  },
  
  recordMedicationAdministration: (medicationId, status, details) => async (dispatch) => {
    dispatch({ 
      type: 'RECORD_MEDICATION_ADMINISTRATION_START', 
      payload: { medicationId, status, details } 
    });
    try {
      const result = await api.recordMedicationAdministration(medicationId, status, details);
      dispatch({ 
        type: 'RECORD_MEDICATION_ADMINISTRATION_SUCCESS', 
        payload: result 
      });
      return result;
    } catch (error) {
      dispatch({ 
        type: 'RECORD_MEDICATION_ADMINISTRATION_ERROR', 
        payload: error 
      });
      throw error;
    }
  },
  
  // Health tracking actions
  loadHealthData: (metric, timeRange) => async (dispatch) => {
    dispatch({ 
      type: 'LOAD_HEALTH_DATA_START', 
      payload: { metric, timeRange } 
    });
    try {
      const data = await api.getHealthData(metric, timeRange);
      dispatch({ type: 'LOAD_HEALTH_DATA_SUCCESS', payload: data });
    } catch (error) {
      dispatch({ type: 'LOAD_HEALTH_DATA_ERROR', payload: error });
    }
  },
  
  recordHealthMeasurement: (metric, value, notes) => async (dispatch) => {
    dispatch({ 
      type: 'RECORD_HEALTH_MEASUREMENT_START', 
      payload: { metric, value, notes } 
    });
    try {
      const result = await api.recordHealthMeasurement(metric, value, notes);
      dispatch({ 
        type: 'RECORD_HEALTH_MEASUREMENT_SUCCESS', 
        payload: result 
      });
      return result;
    } catch (error) {
      dispatch({ 
        type: 'RECORD_HEALTH_MEASUREMENT_ERROR', 
        payload: error 
      });
      throw error;
    }
  },
  
  // Status tracking actions
  loadStatusData: () => async (dispatch) => {
    dispatch({ type: 'LOAD_STATUS_DATA_START' });
    try {
      const data = await api.getStatusData();
      dispatch({ type: 'LOAD_STATUS_DATA_SUCCESS', payload: data });
    } catch (error) {
      dispatch({ type: 'LOAD_STATUS_DATA_ERROR', payload: error });
    }
  },
  
  recordStatusUpdate: (category, status, notes) => async (dispatch) => {
    dispatch({ 
      type: 'RECORD_STATUS_UPDATE_START', 
      payload: { category, status, notes } 
    });
    try {
      const result = await api.recordStatusUpdate(category, status, notes);
      dispatch({ type: 'RECORD_STATUS_UPDATE_SUCCESS', payload: result });
      return result;
    } catch (error) {
      dispatch({ type: 'RECORD_STATUS_UPDATE_ERROR', payload: error });
      throw error;
    }
  }
};
```

### 3.3 API Integration

```javascript
// Care coordination API integration (pseudocode)
const careCoordinationApi = {
  // Dashboard API
  getDashboard: async () => {
    const response = await api.get('/dashboard');
    return response.data;
  },
  
  // Schedule API
  getEvents: async (view, date, filters = {}) => {
    const queryParams = new URLSearchParams();
    
    // Add view and date to query params
    queryParams.append('view', view);
    queryParams.append('date', formatDate(date));
    
    // Add filters to query params
    if (filters.categories) queryParams.append('filter[categories]', filters.categories.join(','));
    if (filters.attendees) queryParams.append('filter[attendees]', filters.attendees.join(','));
    
    const response = await api.get(`/events?${queryParams.toString()}`);
    return response.data;
  },
  
  createEvent: async (eventData) => {
    const response = await api.post('/events', {
      data: eventData
    });
    return response.data;
  },
  
  updateEvent: async (eventId, changes) => {
    const response = await api.put(`/events/${eventId}`, {
      data: changes
    });
    return response.data;
  },
  
  deleteEvent: async (eventId) => {
    await api.delete(`/events/${eventId}`);
    return true;
  },
  
  // Medication API
  getMedications: async (filters = {}) => {
    const queryParams = new URLSearchParams();
    
    // Add filters to query params
    if (filters.status) queryParams.append('filter[status]', filters.status);
    if (filters.scheduled) queryParams.append('filter[scheduled]', filters.scheduled);
    
    const response = await api.get(`/medications?${queryParams.toString()}`);
    return response.data;
  },
  
  getMedication: async (medicationId) => {
    const response = await api.get(`/medications/${medicationId}`);
    return response.data;
  },
  
  createMedication: async (medicationData) => {
    const response = await api.post('/medications', {
      data: medicationData
    });
    return response.data;
  },
  
  updateMedication: async (medicationId, changes) => {
    const response = await api.put(`/medications/${medicationId}`, {
      data: changes
    });
    return response.data;
  },
  
  recordMedicationAdministration: async (medicationId, status, details = {}) => {
    const response = await api.post(`/medications/${medicationId}/administration`, {
      data: {
        status,
        ...details
      }
    });
    return response.data;
  },
  
  // Health tracking API
  getHealthData: async (metric, timeRange) => {
    const queryParams = new URLSearchParams();
    
    queryParams.append('metric', metric);
    queryParams.append('time_range', timeRange);
    
    const response = await api.get(`/health-data?${queryParams.toString()}`);
    return response.data;
  },
  
  recordHealthMeasurement: async (metric, value, notes = '') => {
    const response = await api.post('/health-data', {
      data: {
        metric,
        value,
        notes
      }
    });
    return response.data;
  },
  
  // Status tracking API
  getStatusData: async () => {
    const response = await api.get('/status');
    return response.data;
  },
  
  recordStatusUpdate: async (category, status, notes = '') => {
    const response = await api.post('/status/update', {
      data: {
        category,
        status,
        notes
      }
    });
    return response.data;
  },
  
  performCheckIn: async () => {
    const response = await api.post('/status/check-in');
    return response.data;
  }
};
```

### 3.4 Permission Integration

```javascript
// Care coordination permission hooks (pseudocode)
function useDashboardPermissions() {
  const { checkPermission } = usePermissions();
  const [permissions, setPermissions] = useState({
    canViewStatus: false,
    canRecordStatus: false,
    canViewMedications: false,
    canRecordMedications: false,
    canViewSchedule: false,
    canCreateEvents: false,
    canViewHealthData: false,
    canRecordHealthData: false
  });
  
  useEffect(() => {
    const checkPermissions = async () => {
      const [
        canViewStatus,
        canRecordStatus,
        canViewMedications,
        canRecordMedications,
        canViewSchedule,
        canCreateEvents,
        canViewHealthData,
        canRecordHealthData
      ] = await Promise.all([
        checkPermission('view_status'),
        checkPermission('record_status'),
        checkPermission('view_medications'),
        checkPermission('record_medication_administration'),
        checkPermission('view_schedule'),
        checkPermission('create_event'),
        checkPermission('view_health_data'),
        checkPermission('record_health_data')
      ]);
      
      setPermissions({
        canViewStatus,
        canRecordStatus,
        canViewMedications,
        canRecordMedications,
        canViewSchedule,
        canCreateEvents,
        canViewHealthData,
        canRecordHealthData
      });
    };
    
    checkPermissions();
  }, [checkPermission]);
  
  return permissions;
}

// Role-based permission configurations
const careCoordinationPermissionConfigs = {
  caregiver: {
    dashboard: {
      sections: {
        careRecipientStatus: true,
        todaysSchedule: true,
        medicationTracker: true,
        quickTasks: true,
        recentUpdates: true,
        careTeam: true
      }
    },
    status: {
      canView: true,
      canRecord: true,
      detailLevel: 'detailed',
      categories: ['overall', 'physical', 'emotional', 'social', 'cognitive']
    },
    medications: {
      canView: true,
      canCreate: true,
      canRecord: true,
      detailLevel: 'detailed'
    },
    schedule: {
      canView: true,
      canCreate: true,
      canEdit: true,
      canDelete: true,
      views: ['day', 'week', 'month', 'agenda']
    },
    health: {
      canView: true,
      canRecord: true,
      metrics: ['all'],
      detailLevel: 'detailed'
    }
  },
  
  care_recipient: {
    dashboard: {
      sections: {
        todaysPlan: true,
        myNeeds: true,
        privacyCenter: true,
        upcomingVisits: true,
        myHealth: true,
        messages: true
      }
    },
    status: {
      canView: true,
      canRecord: true,
      detailLevel: 'detailed',
      categories: ['overall', 'physical', 'emotional', 'social', 'cognitive']
    },
    medications: {
      canView: true,
      canCreate: false,
      canRecord: true,
      detailLevel: 'detailed'
    },
    schedule: {
      canView: true,
      canCreate: true,
      canEdit: true,
      canDelete: true,
      views: ['day', 'agenda']
    },
    health: {
      canView: true,
      canRecord: true,
      metrics: ['all'],
      detailLevel: 'detailed'
    }
  },
  
  organizer: {
    dashboard: {
      sections: {
        teamOverview: true,
        todaysSchedule: true,
        taskBoard: true,
        careUpdates: true,
        conflictAlerts: true,
        resourceCenter: true
      }
    },
    status: {
      canView: true,
      canRecord: false,
      detailLevel: 'limited',
      categories: ['overall']
    },
    medications: {
      canView: true,
      canCreate: false,
      canRecord: false,
      detailLevel: 'limited'
    },
    schedule: {
      canView: true,
      canCreate: true,
      canEdit: true,
      canDelete: true,
      views: ['day', 'week', 'month', 'team']
    },
    health: {
      canView: true,
      canRecord: false,
      metrics: ['limited'],
      detailLevel: 'limited'
    }
  },
  
  supporter: {
    dashboard: {
      sections: {
        myTasks: true,
        availabilityStatus: true,
        upcomingCommitments: true,
        careUpdates: true,
        quickActions: true
      }
    },
    status: {
      canView: true,
      canRecord: false,
      detailLevel: 'minimal',
      categories: ['overall']
    },
    medications: {
      canView: true,
      canCreate: false,
      canRecord: true,
      detailLevel: 'minimal'
    },
    schedule: {
      canView: true,
      canCreate: false,
      canEdit: false,
      canDelete: false,
      views: ['agenda']
    },
    health: {
      canView: false,
      canRecord: false,
      metrics: [],
      detailLevel: 'none'
    }
  }
};
```

## 4. Role-Specific Workflows

### 4.1 Caregiver Workflow

1. **Dashboard Experience**
   - Focus on care recipient status and today's priorities
   - Quick access to recording medications, tasks, and status updates
   - Comprehensive view of care activities
   - Critical alerts prominently displayed

2. **Status Monitoring and Recording**
   - Detailed status view across all categories
   - Ability to record observations and check-ins
   - Access to status history and trends
   - Alert system for concerning changes

3. **Medication Management**
   - Complete medication schedule view
   - Administration recording and verification
   - Refill management and pharmacy coordination
   - Side effect monitoring and history

4. **Schedule Coordination**
   - Comprehensive calendar with all care events
   - Ability to create, edit, and manage events
   - Conflict detection and resolution
   - Appointment preparation and follow-up

5. **Health Tracking**
   - Detailed health metrics with trends
   - Recording vital signs and symptoms
   - Pattern recognition and correlation
   - Medical appointment preparation

### 4.2 Care Recipient Workflow

1. **Dashboard Experience**
   - Focus on today's plan and personal autonomy
   - Privacy controls prominently displayed
   - Need expression simplified and dignified
   - Personal wellbeing tracking

2. **Self-Monitoring and Reporting**
   - Self-reporting of status and needs
   - Control over information sharing
   - Dignity-preserving interfaces
   - Personal reflection tools

3. **Medication Management**
   - Own medication schedule view
   - Self-administration recording
   - Assistance requesting when needed
   - Educational resources

4. **Schedule Awareness**
   - Personal calendar view
   - Preferences and input collection
   - Privacy controls for events
   - Creation of personal events

5. **Health Self-Tracking**
   - Personal health metrics
   - Self-recording capabilities
   - Privacy-controlled sharing
   - Educational insights

### 4.3 Organizer Workflow

1. **Dashboard Experience**
   - Focus on team coordination and schedule
   - Task assignment and status tracking
   - Conflict and problem identification
   - Resource allocation

2. **Team Management**
   - Team member availability view
   - Role-based task distribution
   - Conflict resolution tools
   - Team communication facilitation

3. **Schedule Coordination**
   - Master schedule view
   - Team availability integration
   - Conflict detection and resolution
   - Schedule optimization

4. **Status Awareness**
   - High-level status monitoring
   - Care coordination implications
   - Team communication facilitation
   - Resource allocation based on needs

5. **Task Coordination**
   - Care task integration with schedule
   - Task assignment and tracking
   - Priority management
   - Team workload balancing

### 4.4 Supporter Workflow

1. **Dashboard Experience**
   - Focus on assigned tasks and availability
   - Simple, focused interface
   - Clear expectations and boundaries
   - Easy availability management

2. **Task Completion**
   - Clear instructions for assigned tasks
   - Simple recording interfaces
   - Contextual information as needed
   - Effort tracking

3. **Availability Management**
   - Simple availability toggling
   - Schedule integration
   - Commitment visibility
   - Preference settings

4. **Basic Status Awareness**
   - Minimal status information
   - Task-relevant context only
   - Privacy-respectful interfaces
   - Need-to-know basis

5. **Communication Flow**
   - Task-focused updates
   - Direct communication with coordinators
   - Feedback mechanisms
   - Support request capability

## 5. Integration with Task Management System

### 5.1 Task-Schedule Integration

The Care Coordination Center integrates with the Task Management System in several key ways:

1. **Schedule-Based Tasks**
   - Tasks with due dates appear in the schedule
   - Schedule conflicts trigger task warnings
   - Task completion status reflected in calendar
   - Calendar events can generate tasks

2. **Task Dashboard Integration**
   - Today's tasks appear in dashboard
   - Task priority influences dashboard ordering
   - Task status updates trigger dashboard updates
   - Quick task actions available from dashboard

3. **Medication-Task Connection**
   - Medication schedules generate reminder tasks
   - Task completion records medication administration
   - Refill needs create tasks automatically
   - Medication non-adherence creates follow-up tasks

4. **Status-Task Relationship**
   - Status changes can trigger follow-up tasks
   - Task completion influences status updates
   - Status context provided for relevant tasks
   - Care needs identified in status generate tasks

### 5.2 Technical Integration Points

```javascript
// Integration between task system and care coordination (pseudocode)
function useTaskCoordinationIntegration() {
  const { tasks, taskActions } = useTaskSystem();
  const { events, eventActions } = useScheduleSystem();
  const { medications, medicationActions } = useMedicationSystem();
  const { status, statusActions } = useStatusSystem();
  
  // Convert scheduled tasks to calendar events
  useEffect(() => {
    const scheduledTasks = tasks.filter(task => task.due_date);
    
    // Create events from tasks that don't already have events
    scheduledTasks.forEach(task => {
      const existingEvent = events.find(event => event.task_id === task.id);
      
      if (!existingEvent) {
        eventActions.createEvent({
          title: task.title,
          description: task.description,
          start_time: task.due_date,
          end_time: addMinutes(task.due_date, 30),
          category: 'task',
          task_id: task.id
        });
      }
    });
  }, [tasks, events, eventActions]);
  
  // Create medication tasks from schedule
  useEffect(() => {
    const dueMedications = medications.filter(med => isMedicationDueToday(med));
    
    // Create tasks for medications that don't already have tasks
    dueMedications.forEach(med => {
      const existingTask = tasks.find(task => 
        task.medication_id === med.id && 
        isSameDay(task.due_date, new Date())
      );
      
      if (!existingTask) {
        taskActions.createTask({
          title: `Take ${med.name}`,
          description: med.instructions,
          category: 'medication',
          priority: 'high',
          due_date: getMedicationTime(med),
          medication_id: med.id
        });
      }
    });
  }, [medications, tasks, taskActions]);
  
  // Create follow-up tasks from status updates
  useEffect(() => {
    const statusUpdatesNeedingFollowUp = status.updates.filter(
      update => update.follow_up_needed
    );
    
    // Create follow-up tasks if they don't already exist
    statusUpdatesNeedingFollowUp.forEach(update => {
      const existingTask = tasks.find(task => task.status_update_id === update.id);
      
      if (!existingTask) {
        taskActions.createTask({
          title: `Follow up: ${getConcernFromUpdate(update)}`,
          description: `Follow up needed based on status update at ${formatTime(update.timestamp)}`,
          category: 'follow_up',
          priority: 'high',
          due_date: addHours(new Date(), 24),
          status_update_id: update.id
        });
      }
    });
  }, [status.updates, tasks, taskActions]);
  
  // Update task status when medication administration is recorded
  useEffect(() => {
    const administrationLogs = getAllMedicationLogs();
    
    // Find tasks that should be completed based on med logs
    administrationLogs.forEach(log => {
      const matchingTask = tasks.find(task => 
        task.medication_id === log.medication_id && 
        isSameDay(task.due_date, log.actual_time)
      );
      
      if (matchingTask && matchingTask.status !== 'completed') {
        taskActions.updateTask(matchingTask.id, {
          status: 'completed',
          completed_at: log.actual_time,
          completed_by: log.recorded_by.user_id,
          completion_notes: `Automatically marked complete: medication ${log.status}`
        });
      }
    });
  }, [medications, tasks, taskActions]);
  
  return {
    // Return integration utilities
    createTaskFromEvent,
    createTaskFromMedication,
    createTaskFromStatusUpdate,
    completeTaskFromMedication
  };
}
```

## 6. Testing & Validation

### 6.1 Functional Testing Requirements

| Test Area | Test Cases | Acceptance Criteria |
|-----------|------------|---------------------|
| **Dashboard Integration** | Role-specific dashboard loading, Section interaction, Data refresh | Correct content per role, Sections expand/collapse, Data updates properly |
| **Schedule Management** | Event creation, View changes, Conflict detection | Events save correctly, Views render properly, Conflicts detected and displayed |
| **Medication Management** | Medication display, Administration recording, Supply tracking | Medications displayed correctly, Administration records saved, Supply accurately tracked |
| **Health Tracking** | Metric recording, Chart display, Privacy filtering | Metrics recorded properly, Charts render correctly, Privacy settings respected |
| **Status Tracking** | Status updates, Category navigation, Alert generation | Status updates saved, Categories navigate correctly, Alerts generated appropriately |
| **Cross-Component Integration** | Task-schedule integration, Medication-task integration, Status-task integration | Tasks appear in schedule, Medication tasks generated, Status tasks created |

### 6.2 Role Simulation Testing

```javascript
// Example role simulation test
test('Dashboard adapts to role', async () => {
  // Test for Caregiver role
  const { getByText, queryByText } = render(
    <RoleContextProvider initialRole="caregiver">
      <CareCoordinationDashboard />
    </RoleContextProvider>
  );
  
  // Verify caregiver sees appropriate sections
  expect(getByText(/Care Recipient Status/i)).toBeInTheDocument();
  expect(getByText(/Today's Schedule/i)).toBeInTheDocument();
  expect(getByText(/Medication Tracker/i)).toBeInTheDocument();
  expect(getByText(/Quick Tasks/i)).toBeInTheDocument();
  
  // Test for Care Recipient role
  cleanup();
  const { getByText: getRecipientText, queryByText: queryRecipientText } = render(
    <RoleContextProvider initialRole="care_recipient">
      <CareCoordinationDashboard />
    </RoleContextProvider>
  );
  
  // Verify care recipient sees appropriate sections
  expect(getRecipientText(/Today's Plan/i)).toBeInTheDocument();
  expect(getRecipientText(/My Needs/i)).toBeInTheDocument();
  expect(getRecipientText(/Privacy Center/i)).toBeInTheDocument();
  expect(queryRecipientText(/Medication Tracker/i)).not.toBeInTheDocument();
  
  // Additional role tests...
});
```

### 6.3 Permission Testing

```javascript
// Example permission test
test('Medication recording respects permissions', async () => {
  // Mock medication data
  const mockMedication = { id: 'med123', name: 'Test Medication' };
  
  // Mock permission service
  const mockPermissionService = {
    checkPermission: jest.fn()
  };
  
  // Test as Caregiver with permission
  mockPermissionService.checkPermission.mockImplementation(permission => {
    if (permission === 'record_medication_administration') {
      return Promise.resolve(true);
    }
    return Promise.resolve(false);
  });
  
  const { getByText } = render(
    <PermissionContext.Provider value={mockPermissionService}>
      <RoleContextProvider initialRole="caregiver">
        <MedicationDetails medication={mockMedication} />
      </RoleContextProvider>
    </PermissionContext.Provider>
  );
  
  // Verify record button exists for caregiver with permission
  expect(getByText(/Record Administration/i)).toBeInTheDocument();
  
  // Test as Supporter without permission
  cleanup();
  mockPermissionService.checkPermission.mockImplementation(permission => {
    return Promise.resolve(false);
  });
  
  const { queryByText } = render(
    <PermissionContext.Provider value={mockPermissionService}>
      <RoleContextProvider initialRole="supporter">
        <MedicationDetails medication={mockMedication} />
      </RoleContextProvider>
    </PermissionContext.Provider>
  );
  
  // Verify record button does not exist for supporter without permission
  expect(queryByText(/Record Administration/i)).not.toBeInTheDocument();
});
```

### 6.4 Integration Testing

```javascript
// Example integration test
test('Medication administration creates task completion', async () => {
  // Mock medication and task data
  const mockMedication = { 
    id: 'med123', 
    name: 'Test Medication',
    schedule: [{ time: '09:00', days: ['monday', 'tuesday', 'wednesday', 'thursday', 'friday'] }]
  };
  
  const mockTask = {
    id: 'task123',
    title: 'Take Test Medication',
    medication_id: 'med123',
    due_date: new Date('2025-04-14T09:00:00Z'),
    status: 'incomplete'
  };
  
  // Mock APIs
  const mockApi = {
    recordMedicationAdministration: jest.fn(),
    updateTask: jest.fn()
  };
  
  mockApi.recordMedicationAdministration.mockResolvedValue({
    medication_id: 'med123',
    status: 'taken',
    actual_time: new Date('2025-04-14T09:10:00Z'),
    recorded_by: { user_id: 'user123', role: 'caregiver' }
  });
  
  mockApi.updateTask.mockResolvedValue({
    ...mockTask,
    status: 'completed',
    completed_at: new Date('2025-04-14T09:10:00Z'),
    completed_by: 'user123'
  });
  
  // Set up test component with mocked state and APIs
  const { getByText } = render(
    <ApiContext.Provider value={mockApi}>
      <TaskContext.Provider value={{ tasks: [mockTask], taskActions }}>
        <MedicationAdministrationForm medication={mockMedication} />
      </TaskContext.Provider>
    </ApiContext.Provider>
  );
  
  // Simulate recording administration
  fireEvent.click(getByText('Taken'));
  fireEvent.click(getByText('Record'));
  
  // Wait for all promises to resolve
  await waitFor(() => {
    // Verify medication administration was recorded
    expect(mockApi.recordMedicationAdministration).toHaveBeenCalledWith(
      'med123',
      'taken',
      expect.any(Object)
    );
    
    // Verify task was marked complete
    expect(mockApi.updateTask).toHaveBeenCalledWith(
      'task123',
      expect.objectContaining({
        status: 'completed',
        completed_at: expect.any(Date),
        completed_by: expect.any(String)
      })
    );
  });
});
```

### 6.5 Accessibility Testing

| Test Method | Focus Areas | Tools |
|-------------|-------------|-------|
| **Automated Testing** | ARIA roles, Labels, Contrast | Axe, Lighthouse |
| **Keyboard Navigation** | Tab order, Focus management, Section navigation | Manual testing |
| **Screen Reader** | Dashboard announcements, Status changes, Medication instructions | NVDA, VoiceOver |
| **Touch Targets** | Dashboard cards, Calendar events, Administration buttons | Manual testing |
| **Reduced Motion** | Chart animations, Status updates, Transitions | Manual testing |

## 7. Emotional Intelligence Considerations

### 7.1 Role-Specific Emotional Design

| Role | Emotional Context | Design Approach | Language Principles |
|------|-------------------|-----------------|---------------------|
| **Caregiver** | May feel overwhelm from coordination, anxiety about wellbeing | Organize information to reduce cognitive load, Prioritize critical items, Provide reassurance through visibility | Supportive, appreciative, clarity-focused |
| **Care Recipient** | May feel loss of privacy, concern about being burdensome | Emphasize control and agency, Privacy-first design, Dignified interaction patterns | Empowering, respectful, privacy-centered |
| **Organizer** | May feel responsibility for team effectiveness, need for clarity | Focus on coordination tools, Clear status indicators, Efficient problem-solving | Clarity-focused, team-oriented, solution-driven |
| **Supporter** | May worry about commitment level, helping effectively | Simple focused interface, Clear boundaries, Appreciation for contribution | Appreciative, clear, focused on specific contribution |

### 7.2 Status Communication Guidelines

| Status Type | Language Guidelines | Visual Treatment | Alert Handling |
|-------------|---------------------|------------------|----------------|
| **Good Status** | Positive but not overly cheerful, Recognition without patronizing | Green/blue tones, Gentle positive indicators | Quiet confirmation, No unnecessary emphasis |
| **Fair Status** | Matter-of-fact, without catastrophizing | Neutral tones, Informational presentation | Low-key awareness, Context-appropriate |
| **Needs Attention** | Clear without alarming, Action-oriented | Amber tones, Moderate emphasis | Visible but not intrusive, Clear next steps |
| **Concerning Status** | Direct without fostering panic, Solution-focused | Red tones, Appropriate emphasis | Clear notification, Action guidance |

### 7.3 Privacy-Sensitive Design

| Content Type | Privacy Approach | Visual Indicators | User Controls |
|--------------|------------------|-------------------|--------------|
| **Health Data** | Need-to-know basis, Granular permissions | Clear privacy indicators, Locked content visualization | Simple sharing toggles, Default to private |
| **Medication Information** | Basic info for coordination, Details restricted | Medication presence without details, Access level indicators | Category-level controls, Purpose-based access |
| **Personal Status** | Emotional state sharing optional, Physical status need-to-know | Privacy-state indicators, Abstracted visualizations | Granular sharing controls, Revocable access |
| **Schedule** | Personal vs. care events distinction, Private appointment contents | Event type without details, Privacy-level color coding | Event-level privacy, Role-based defaults |

## 8. Implementation Plan

### 8.1 Development Phases

| Phase | Focus | Timeline | Dependencies |
|-------|-------|----------|--------------|
| **Phase 1: Core Framework** | Dashboard components, Integration architecture | Week 8, Days 1-2 | Task Management System |
| **Phase 2: Schedule System** | Calendar views, Event management | Week 8, Days 3-4 | Core Framework |
| **Phase 3: Medication System** | Medication tracking, Administration recording | Week 8, Day 5 - Week 9, Day 1 | Core Framework |
| **Phase 4: Health & Status** | Health metrics, Status tracking | Week 9, Days 2-3 | Core Framework |
| **Phase 5: Integration** | Cross-component integration, Role adaptations | Week 9, Days 4-5 | All previous phases |

### 8.2 Key Milestones

1. Dashboard framework with role adaptations functional
2. Schedule management system implemented
3. Medication tracking and administration system complete
4. Health tracking and status system integrated
5. Cross-component integration and task system connections complete
6. Full role adaptation testing and refinement complete

### 8.3 Dependencies

| Component | Dependencies | Risk Factors |
|-----------|--------------|--------------|
| **Dashboard** | Role context, Permission framework | Medium - relies on role adaptation |
| **Schedule System** | Task integration | Medium - event-task coordination complexity |
| **Medication System** | Permission framework, Task integration | High - privacy and medical accuracy concerns |
| **Health & Status** | Permission framework, Privacy controls | High - sensitive data handling |
| **Integration** | All previous components | High - complex interdependencies |

## 9. Acceptance Criteria

### 9.1 Functional Requirements

- ✅ Role-adaptive dashboard that presents relevant information for each role
- ✅ Schedule management with appropriate creation and viewing capabilities per role
- ✅ Medication tracking with role-appropriate administration recording
- ✅ Health data visualization and recording with privacy controls
- ✅ Status tracking and updates with role-based access levels
- ✅ Cross-component integration with the Task Management System
- ✅ Proper permission enforcement across all components

### 9.2 Performance Requirements

- ✅ Dashboard loads within 1 second with all components
- ✅ Calendar renders with 100+ events in under 500ms
- ✅ Medication recording completes within 300ms
- ✅ Health charts render with 1000+ data points in under 1 second
- ✅ Status updates apply within 300ms
- ✅ Role switching adapts UI within 500ms

### 9.3 Accessibility Requirements

- ✅ All components pass WCAG 2.1 AA compliance checks
- ✅ Dashboard sections navigable via keyboard
- ✅ Calendar accessible via keyboard with screen reader support
- ✅ Medication interfaces usable without vision
- ✅ Health charts include non-visual alternatives
- ✅ Status changes announced appropriately to screen readers

### 9.4 Cross-Role Requirements

- ✅ All roles can access appropriate dashboard with role-specific content
- ✅ Permission boundaries respected across all components
- ✅ Privacy controls honored in all data display
- ✅ Role-appropriate language and UI elements throughout
- ✅ Consistent information architecture across roles

## 10. Change Log

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0.0 | 2025-04-14 | Product Lead | Initial specification |
