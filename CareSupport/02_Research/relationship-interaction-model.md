# Relationship & Interaction Model

**Version:** 1.0.0  
**Last Updated:** 2025-04-14  
**Status:** Draft  
**Document Location:** `/02_Research/Relationship_Model.md`

## 1. Overview

This document defines the foundational relationship structure and interaction models that underpin CareSupport's unified platform. It maps how different roles interact with each other and with shared objects, establishes permission frameworks, and defines information flows across the care ecosystem.

### 1.1 Related Documents

- `/04_Onboarding/README_Onboarding_Engineer_Handoff.md`
- `/01_Product_Strategy/CareSupport_Emotional_Intelligence_Constitution.md`
- `/05_Care_Plan_Setup/Care_Plan_Setup_Dashboard_Reminders.md`
- `/06_Core_Platform/Post_Setup_Transition/Welcome_Home_Specification.md`

### 1.2 Purpose & Goals

- Define the relationship structure between all care roles
- Map permissions and information flows between roles
- Establish shared object models and ownership
- Create the foundation for role-adaptive component design
- Ensure emotional intelligence principles apply to all interactions

## 2. Core Role Definitions

### 2.1 Role Characteristics & Responsibilities

| Role | Primary Characteristics | Core Responsibilities | Emotional Context |
|------|-------------------------|----------------------|-------------------|
| **Caregiver** | Hands-on, daily involvement, emotionally invested, often family member | Medication management, appointment coordination, daily care provision, health monitoring | May experience stress, guilt, fatigue; seeks relief and support; emotionally connected to care recipient |
| **Care Recipient** | Person receiving care, varying levels of independence, may have health challenges | Self-care to extent possible, communicating needs, maintaining dignity and agency | May feel vulnerability, loss of independence, desire for autonomy while needing support |
| **Organizer** | Logistics-focused, detail-oriented, often manages multiple people | Task coordination, schedule management, resource allocation, team communication | May feel pressure to keep everything running smoothly; focused on systems and efficiency |
| **Supporter** | Occasional help, specific skill areas, limited time commitment | Specific tasks, time-bounded support, responding to requests | Wants to help without overwhelming commitment; may have limited emotional bandwidth for care situation |

### 2.2 Role Prevalence

| Primary Role | Can Also Have These Additional Roles | Typical Pattern |
|--------------|-------------------------------------|----------------|
| Caregiver | Organizer | Common dual-role |
| Organizer | Supporter | Common dual-role |
| Supporter | (Rarely other roles) | Usually single-role |
| Care Recipient | (No additional roles) | Always single-role |

## 3. Relationship Map

### 3.1 Primary Relationships

![Relationship Diagram]()

| Relationship | Direction | Key Interactions | Information Shared | Boundaries |
|--------------|-----------|------------------|-------------------|------------|
| **Caregiver ↔ Care Recipient** | Bidirectional | Direct care provision, Needs expression, Status updates | Health info, Daily activities, Emotional state | Privacy preferences, Dignity preservation |
| **Caregiver ↔ Organizer** | Bidirectional | Task handoff, Schedule coordination, Resource requests | Care plan, Task assignments, Calendar | Caregiver autonomy, Decision authority |
| **Caregiver ↔ Supporter** | Primarily Caregiver → Supporter | Task requests, Status updates, Guidance | Specific task needs, Limited health context | Time boundaries, Commitment limits |
| **Organizer ↔ Care Recipient** | Limited bidirectional | Schedule confirmation, Preference checking | Limited to schedule, preferences, non-intimate care | Privacy controls stricter than with Caregiver |
| **Organizer ↔ Supporter** | Primarily Organizer → Supporter | Task assignment, Scheduling, Follow-up | Task details, Timing, Priority | Authority limits, Commitment boundaries |
| **Supporter ↔ Care Recipient** | Guided bidirectional | Direct help, Basic social interaction | Task-specific details, Social connection | Privacy heavily restricted, Minimal health info |

### 3.2 Care Circle Structure

The Care Circle represents the complete network of care relationships. Key characteristics:

- **Hierarchy**: Semi-flat with soft role-based permissions
- **Formation**: Initiated by Caregiver, can grow organically
- **Size**: Typically 1-3 Caregivers, 0-1 Organizers, 0-5 Supporters, 1 Care Recipient
- **Boundaries**: Permeable, with members able to join/leave based on invitation system
- **Leadership**: Distributed, with primary Caregiver as default coordinator

## 4. Shared Objects & Permissions

### 4.1 Task Object

A discrete unit of care work that can be assigned, tracked, and completed.

```json
{
  "task": {
    "id": "task123",
    "title": "Pick up medications",
    "description": "Get refills from Walgreens on Main St",
    "category": "medication",
    "priority": "high",
    "due_date": "2025-04-15T15:00:00Z",
    "recurrence": null,
    "status": "assigned",
    "created_by": {
      "id": "user456",
      "role": "caregiver"
    },
    "assigned_to": {
      "id": "user789",
      "role": "supporter"
    },
    "care_recipient": {
      "id": "recipient123"
    },
    "privacy_level": "circle"
  }
}
```

#### Task Permissions

| Role | Create | View | Edit | Assign | Complete | Delete |
|------|--------|------|------|--------|----------|--------|
| Caregiver | ✅ | ✅ | ✅ | ✅ | ✅ (if assigned to self) | ✅ (if creator) |
| Organizer | ✅ | ✅ | ✅ | ✅ | ✅ (if assigned to self) | ✅ (if creator) |
| Supporter | ❌ | ✅ (if assigned to them) | ❌ | ❌ | ✅ (if assigned to them) | ❌ |
| Care Recipient | ✅ (needs only) | ✅ | ❌ | ❌ | ✅ (self-care tasks) | ❌ |

### 4.2 Schedule/Event Object

Calendar events for appointments, medication times, or care activities.

```json
{
  "event": {
    "id": "event123",
    "title": "Doctor Appointment",
    "description": "Quarterly checkup with Dr. Smith",
    "location": "123 Medical Plaza, Suite 400",
    "category": "medical",
    "start_time": "2025-04-20T14:00:00Z",
    "end_time": "2025-04-20T15:00:00Z",
    "recurrence": null,
    "attendees": [
      {
        "id": "user456",
        "role": "caregiver",
        "status": "confirmed"
      },
      {
        "id": "recipient123",
        "role": "care_recipient",
        "status": "confirmed"
      }
    ],
    "created_by": {
      "id": "user456",
      "role": "caregiver"
    },
    "reminder": {
      "time": "1 hour before",
      "recipients": ["user456"]
    },
    "privacy_level": "caregiver_only"
  }
}
```

#### Event Permissions

| Role | Create | View | Edit | Add Attendees | Delete |
|------|--------|------|------|--------------|--------|
| Caregiver | ✅ | ✅ | ✅ | ✅ | ✅ (if creator) |
| Organizer | ✅ | ✅ | ✅ | ✅ | ✅ (if creator) |
| Supporter | ❌ | ✅ (if attendee or public) | ❌ | ❌ | ❌ |
| Care Recipient | ✅ (personal events) | ✅ (based on privacy settings) | ✅ (personal events only) | ❌ | ✅ (personal events only) |

### 4.3 Update/Journal Object

Communications, status updates, and observations about care.

```json
{
  "update": {
    "id": "update123",
    "type": "observation",
    "content": "Mom seemed more tired than usual today, but ate a full lunch",
    "media": [], 
    "timestamp": "2025-04-14T18:30:00Z",
    "author": {
      "id": "user456",
      "role": "caregiver"
    },
    "about": {
      "id": "recipient123",
      "role": "care_recipient"
    },
    "mood_indicator": "concerned",
    "tags": ["energy", "appetite"],
    "privacy_level": "caregivers_only",
    "comments": []
  }
}
```

#### Update Permissions

| Role | Create | View | Comment | Edit | Delete |
|------|--------|------|---------|------|--------|
| Caregiver | ✅ | ✅ | ✅ | ✅ (if author) | ✅ (if author) |
| Organizer | ✅ | ✅ (based on privacy) | ✅ | ✅ (if author) | ✅ (if author) |
| Supporter | ✅ (limited) | ✅ (based on privacy) | ✅ (limited) | ✅ (if author) | ✅ (if author) |
| Care Recipient | ✅ | ✅ (based on privacy) | ✅ | ✅ (if author) | ✅ (if author) |

### 4.4 Health Data Object

Medical information, measurements, and health observations.

```json
{
  "health_data": {
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
      "id": "user456",
      "role": "caregiver"
    },
    "care_recipient": {
      "id": "recipient123"
    },
    "notes": "Morning reading, after breakfast",
    "privacy_level": "caregivers_only"
  }
}
```

#### Health Data Permissions

| Role | Create | View | Edit | Delete |
|------|--------|------|------|--------|
| Caregiver | ✅ | ✅ | ✅ (if creator) | ✅ (if creator) |
| Organizer | ✅ | ✅ (if permitted by Care Recipient) | ✅ (if creator) | ✅ (if creator) |
| Supporter | ❌ | ❌ (rare exceptions) | ❌ | ❌ |
| Care Recipient | ✅ | ✅ | ✅ | ✅ |

## 5. Privacy Framework

### 5.1 Privacy Levels

| Level | Description | Applied To |
|-------|-------------|------------|
| **Private** | Visible only to creator and Care Recipient | Sensitive health data, personal journals |
| **Caregivers Only** | Limited to Caregivers and Care Recipient | Most health data, intimate care notes |
| **Care Team** | Caregivers, Organizers, and Care Recipient | Schedules, general health status |
| **Circle** | All members including Supporters | Tasks, general updates, non-medical events |
| **Public** | Anyone with link, including future members | Community resources, shared knowledge |

### 5.2 Care Recipient Privacy Control

The Care Recipient has ultimate control over their privacy through:

- Global privacy preferences set during onboarding
- Per-object privacy controls
- Ability to restrict specific users from accessing categories of information
- Option to review shared information

### 5.3 Information Visibility Matrix

| Information Type | Caregiver | Organizer | Supporter | Care Recipient |
|------------------|-----------|-----------|-----------|----------------|
| Medical Details | Full access | Limited by preference | None | Full access |
| Medications | Full access | Names + schedule | Task-specific only | Full access |
| Intimate Care Notes | Full access | None | None | Full access |
| Appointments | Full access | Full access | If involved | Full access |
| Tasks | Full access | Full access | Only assigned | Full access |
| Daily Activities | Full access | Summary | None | Full access |
| Emotional Status | Full access | Limited | None | Full access |
| Journal Entries | By permission | None | None | Full access |

## 6. Key Interaction Flows

### 6.1 Task Assignment Flow

1. Caregiver or Organizer creates task
2. System suggests assignee based on:
   - Role appropriateness
   - Past task history
   - Availability preferences
   - Relationship to Care Recipient
3. Creator assigns task (or accepts suggestion)
4. Assignee receives notification
5. Assignee accepts/declines
6. Upon completion:
   - Assignee marks as complete
   - Creator receives notification
   - Care Recipient can see completion

### 6.2 Health Status Communication Flow

1. Caregiver records health observation
2. System applies privacy rules based on:
   - Observation type
   - Care Recipient preferences
   - Caregiver sharing choices
3. Appropriate circle members notified based on:
   - Role
   - Urgency
   - Related responsibilities
4. View access granted according to privacy matrix
5. Comments/responses filtered by permission level

### 6.3 Schedule Coordination Flow

1. Caregiver or Organizer creates event
2. System identifies required attendees
3. Optional attendees suggested based on:
   - Event type
   - Role capabilities
   - Availability
4. Invitations sent with appropriate context per role
5. Conflicts identified across circle calendar
6. Attendance confirmed/declined
7. Reminders customized by role

### 6.4 Support Request Flow

1. Caregiver identifies need
2. System suggests appropriate Supporters based on:
   - Skill match
   - Availability
   - Relationship strength
   - Past reliability
3. Request sent with role-appropriate context
4. Supporter accepts/declines/suggests alternative
5. Confirmation loop with Caregiver
6. Task created and assigned upon acceptance
7. Completion notification and acknowledgment

## 7. Emotional Intelligence Integration

### 7.1 Role-Specific Emotional Awareness

| Role | Common Emotional States | System Response Patterns |
|------|-------------------------|--------------------------|
| Caregiver | Overwhelm, guilt, fatigue, fulfillment | Acknowledge effort, provide support resources, celebrate contributions, avoid judgment |
| Care Recipient | Vulnerability, frustration, gratitude, loss of agency | Reinforce autonomy, maintain dignity, acknowledge feelings, provide control mechanisms |
| Organizer | Pressure, responsibility, accomplishment, detachment | Recognize coordination effort, provide clarity, acknowledge organizational impact |
| Supporter | Desire to help, concern about commitment, connection | Reinforce value of limited contribution, provide clear boundaries, acknowledge impact |

### 7.2 Interaction Tone Mapping

| Interaction Direction | Communication Tone | Content Focus | Boundaries |
|-----------------------|-------------------|---------------|------------|
| Caregiver → Care Recipient | Warm, respectful, never infantilizing | Care actions, emotional check-ins, health monitoring | Privacy, dignity |
| Care Recipient → Caregiver | Direct need expression, personal choice | Preferences, needs, feelings, feedback | Autonomy, voice |
| Organizer → Caregiver | Supportive, coordinating, clarifying | Task logistics, scheduling, resource allocation | Authority limits |
| Supporter → Care Recipient | Friendly, focused, present-oriented | Current task, immediate needs, lightweight connection | Privacy, limited scope |
| Organizer → Supporter | Clear, appreciative, specific | Task details, timing, boundaries | Time commitment |

### 7.3 Emotional Trigger Points & System Responses

| Trigger Point | Potential Emotional Impact | System Response |
|---------------|----------------------------|-----------------|
| Missed medication | Caregiver anxiety, guilt | Non-judgmental alert, calm resolution guidance |
| Declined task | Organizer frustration | Suggest alternatives, acknowledge challenges |
| Health decline | Family concern, Care Recipient fear | Privacy-sensitive updates, support resources |
| Support limits | Supporter boundary stress | Validate contribution, reinforce acceptable limits |
| Privacy concerns | Care Recipient anxiety | Immediate control options, confirmation of choices |

## 8. Implementation Guidelines

### 8.1 Component Adaptation Strategy

Components should adapt to roles through:

- **Content variation**: Same structure, different content
- **Permission states**: UI reflects available actions
- **Contextual emphasis**: Highlighting role-relevant aspects
- **Emotional intelligence**: Adapting tone and framing

### 8.2 Key Component Adaptations

| Component | Caregiver Adaptation | Organizer Adaptation | Supporter Adaptation | Care Recipient Adaptation |
|-----------|----------------------|----------------------|----------------------|---------------------------|
| Task Card | Full details, assign controls | Team view, coordination tools | Simplified, accept/decline focus | Simple overview, privacy controls |
| Calendar | Medical details, care emphasis | Coordination view, full team schedule | Only events they're involved in | Daily rhythm view, privacy settings |
| Update Feed | Full health details, emotion support | Logistics updates prioritized | Limited to general updates | Privacy-controlled view of all updates |
| Health Tracker | Comprehensive input and history | Summary indicators only | Not visible | Full access with journal integration |

### 8.3 Information Architecture Implications

- Role-based navigation priorities
- Consistent core framework
- Permission-aware components
- Progressive disclosure based on role

## 9. Development Considerations

### 9.1 Technical Requirements

- Role-based permission system with granular control
- Relationship graph database structure
- Privacy rule evaluation engine
- Role state components

### 9.2 Experience Guidelines

- Clear role indication in UI
- Transparent permission visualization
- Consistent interaction patterns
- Role-switching support

## 10. Next Steps

1. Develop Information Architecture based on these relationships
2. Create Component Adaptation Specifications
3. Build Permission Framework
4. Design key role-based views

## Change Log

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0.0 | 2025-04-14 | Product Lead | Initial document |
