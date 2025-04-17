# Core Component System Specifications

**Version:** 1.0.0  
**Last Updated:** 2025-04-14  
**Status:** Draft  
**Document Location:** `/08_Design_Standards/Component_Library/Component_Overview.md`

## 1. Overview

This document defines the comprehensive component system that powers CareSupport's unified platform. It details how our UI components adapt to different roles while maintaining consistency in structure, interaction patterns, and accessibility features.

### 1.1 Related Documents

- `/02_Research/Relationship_Model.md`
- `/03_Information_Architecture/Navigation_System.md`
- `/08_Design_Standards/accessibility.md`
- `/08_Design_Standards/terminology.md`

### 1.2 Purpose & Goals

- Define the core UI components used across the platform
- Detail how components adapt to different roles and contexts
- Establish shared interaction patterns and behaviors
- Ensure consistent accessibility implementation
- Provide implementation guidance for engineering

## 2. Component Adaptation Framework

### 2.1 Core Adaptation Principles

All components in the CareSupport platform adapt to user roles and contexts through these consistent mechanisms:

| Adaptation Method | Description | Implementation |
|-------------------|-------------|----------------|
| **Content Filtering** | Same structure, different content | Content props with role-based rendering |
| **Permission Controls** | Actions enabled or disabled based on role | Permission checks within component logic |
| **Progressive Disclosure** | Different levels of detail shown by role | Conditional rendering with complexity levels |
| **Visual Emphasis** | Different elements highlighted by role | Role-specific style variations |
| **Tone Adaptation** | Language adapts to role and context | Text variation through string templates |

### 2.2 Component State System

Every component implements a consistent state system:

| State | Visual Indication | Purpose | Accessibility |
|-------|-------------------|---------|--------------|
| Default | Standard styling | Normal interactive state | - |
| Focused | Outline or highlight | Keyboard navigation | `aria-current="true"` |
| Loading | Skeleton or spinner | Data fetching in progress | `aria-busy="true"` |
| Error | Error icon, text | Data or action failure | `aria-invalid="true"` |
| Disabled | Reduced opacity | Action unavailable | `aria-disabled="true"` |
| Selected | Highlight, checkmark | Item is selected | `aria-selected="true"` |
| Expanded | Content visible | Showing additional details | `aria-expanded="true"` |

### 2.3 Responsive Behaviors

All components respond to:

- Screen size (mobile, tablet, desktop)
- Orientation changes
- Font size adjustments
- Input method (touch, keyboard, voice)
- Reduced motion preferences
- High contrast preferences

## 3. Core Component Library

### 3.1 Navigation Components

#### 3.1.1 Tab Bar

Primary navigation component used across all roles.

**Base Structure:**
- Container with 4 tab items
- Each tab: Icon + Label
- Active state indication
- Optional notification badges

**Role Adaptations:**

| Role | Visible Tabs | Default Tab | Notifications |
|------|--------------|-------------|---------------|
| Caregiver | All 4 tabs | Home | All types |
| Organizer | All 4 tabs | Home | Task, schedule related |
| Supporter | All 4 tabs | Home | Only assigned tasks |
| Care Recipient | All 4 tabs | Home | Privacy-filtered |

**States:**
- Default: All tabs visible
- Active: Current tab highlighted
- Badge: Shows counts for new items
- Disabled: Grayed out if section unavailable

**Accessibility:**
- Role: `role="tablist"`
- Tab items: `role="tab"`
- Active tab: `aria-selected="true"`
- Keyboard navigation with arrow keys

#### 3.1.2 Header Bar

Context header that appears at the top of each screen.

**Base Structure:**
- Title (current section)
- Back button (when applicable)
- Action buttons (contextual)
- Role indicator (for multi-role users)

**Role Adaptations:**

| Role | Default Title | Action Buttons | Special Features |
|------|---------------|----------------|-----------------|
| Caregiver | "Home" or section name | Add, Search, Notifications | Care recipient selector (if multiple) |
| Organizer | "Home" or section name | Add, Search, Notifications | Team filter dropdown |
| Supporter | "Home" or section name | Availability toggle, Notifications | Simplified actions |
| Care Recipient | "Home" or section name | Help, Notifications | Privacy indicator |

**Accessibility:**
- Title: `<h1>` for screen section
- Back button: Clear label "Back to [previous screen]"
- Actions: Descriptive labels for each button

### 3.2 Content Components

#### 3.2.1 Task Card

Displays individual tasks across the platform.

**Base Structure:**
- Title
- Due date/time
- Status indicator
- Category icon
- Assigned to (avatar)
- Priority indicator
- Expand/collapse control

**Role Adaptations:**

| Role | Content Emphasis | Available Actions | Special Features |
|------|------------------|-------------------|-----------------|
| Caregiver | Priority, due date | Create, Edit, Assign, Complete | Full details, notes |
| Organizer | Team assignment, status | Create, Edit, Assign, Reassign | Conflict detection |
| Supporter | Due date, instructions | Accept/Decline, Complete, Comment | Simplified view |
| Care Recipient | What, who, when | View, Request changes | Privacy controls |

**States:**
- Default: Collapsed view
- Expanded: Full details visible
- Completed: Checked off, reduced opacity
- Overdue: Warning indication
- In progress: Status indicator

**Accessibility:**
- Structure: Heading for task name
- Status: Both visual and text indicators
- Actions: Clearly labeled buttons
- Expanded state: `aria-expanded="true/false"`

#### 3.2.2 Status Card

Displays care recipient status information.

**Base Structure:**
- Status summary line
- Last updated timestamp
- Key metrics (customizable)
- Update/action buttons

**Role Adaptations:**

| Role | Information Shown | Available Actions | Special Features |
|------|-------------------|-------------------|-----------------|
| Caregiver | Comprehensive health data | Update, Take action, Add note | Detailed metrics, history |
| Organizer | General wellbeing, key metrics | View, Notify team | Coordination emphasis |
| Supporter | Basic status only | View | Minimal information |
| Care Recipient | Self-reflection, own metrics | Update, Add journal entry | Privacy controls |

**States:**
- Default: Current status view
- Alert: Indicates concerning metrics
- Updating: Shows when new data is being entered
- Historical: When viewing past status

**Accessibility:**
- Status changes: Announced to screen readers
- Metrics: Include units and context
- Color-coding: Always paired with text
- Updates: Timestamp with relative time

#### 3.2.3 Calendar Component

Displays schedule and events.

**Base Structure:**
- Day/week/month views
- Event cards within time slots
- Quick navigation controls
- Add/edit controls

**Role Adaptations:**

| Role | Calendar View | Available Actions | Special Features |
|------|---------------|-------------------|-----------------|
| Caregiver | Full calendar with all events | Create, Edit, Share | Medication schedule integration |
| Organizer | Team calendar with assignments | Create, Edit, Assign | Conflict highlighting |
| Supporter | Personal availability + commitments | Update availability, Accept/decline | Limited to relevant events |
| Care Recipient | Daily view emphasis, simplified | Add personal events, View | Privacy controls for sharing |

**Accessibility:**
- Event cards: Complete date/time in accessible format
- Navigation: Clear previous/next controls
- Selection: Visual and programmatic focus states
- View toggle: Properly labeled options

#### 3.2.4 Update Feed Card

Displays status updates and communication.

**Base Structure:**
- Author info
- Timestamp
- Message content
- Media (if applicable)
- Reaction/comment controls
- Privacy indicator

**Role Adaptations:**

| Role | Content Shown | Available Actions | Special Features |
|------|---------------|-------------------|-----------------|
| Caregiver | All updates, full details | Create, Comment, React | Health observation tools |
| Organizer | Team updates, coordination focus | Create, Comment, Share | Task-related filtering |
| Supporter | Limited to general updates | View, Comment on limited basis | Task-related updates only |
| Care Recipient | All updates about them | Create, Comment, Privacy controls | Control over visibility |

**Accessibility:**
- Clear authorship and timestamps
- Media alternatives (descriptions)
- Non-text content indicators
- Proper heading structure

### 3.3 Form Components

#### 3.3.1 Multi-mode Input

Input component that supports text, voice, and selection.

**Base Structure:**
- Input field with label
- Voice input option
- Validation feedback
- Helper text

**Role Adaptations:**

| Role | Input Emphasis | Special Features | Defaults |
|------|----------------|------------------|----------|
| Caregiver | Efficiency, voice-first | Medical term suggestions | Autocomplete from history |
| Organizer | Precision, format | Team member suggestions | Task templates |
| Supporter | Simplicity | Minimal input requirements | Quick responses |
| Care Recipient | Accessibility-first | Large tap targets, voice emphasis | Simplified options |

**Accessibility:**
- Voice input: Alternative to typing
- Error states: Clear feedback
- Labels: Always visible
- Validation: Immediate feedback

#### 3.3.2 Action Button

Primary interaction component for key actions.

**Base Structure:**
- Label text
- Optional icon
- Visual emphasis variations
- Loading state

**Role Adaptations:**

| Role | Primary Button Style | Language Pattern | Behavior |
|------|----------------------|------------------|----------|
| Caregiver | Standard emphasis | Action-oriented, efficient | Immediate feedback |
| Organizer | Coordination emphasis | Team-focused language | Confirmation for some actions |
| Supporter | Reduced prominence | Opt-in language, flexibility | Clear accept/decline options |
| Care Recipient | High visibility | Agency-reinforcing language | Confirmation steps for actions |

**States:**
- Default: Ready for interaction
- Hover/Focus: Visual emphasis
- Active: Press state
- Loading: Progress indicator
- Success: Completion feedback
- Error: Failure indication

**Accessibility:**
- Touch target: Minimum 44x44px
- Text contrast: Minimum 4.5:1
- Loading state: Properly announced
- Success/error: Visual and announced feedback

### 3.4 Feedback Components

#### 3.4.1 Notification Component

Displays system and user notifications.

**Base Structure:**
- Icon indicating type
- Title text
- Description text
- Timestamp
- Actions (if applicable)
- Dismiss control

**Role Adaptations:**

| Role | Notification Types | Tone | Priority System |
|------|-------------------|------|-----------------|
| Caregiver | All types | Supportive, action-oriented | Critical health first |
| Organizer | Team, schedule, task-related | Coordination focus | Schedule conflicts first |
| Supporter | Only assigned tasks, availability | Low-pressure, optional | Task timing first |
| Care Recipient | Privacy-controlled, daily plan | Empowering, respectful | Daily rhythm first |

**States:**
- Unread: Visual emphasis
- Read: Reduced emphasis
- Urgent: High visibility treatment
- Dismissed: Removed or in history
- Actionable: With action buttons

**Accessibility:**
- Announced to screen readers
- Urgent notifications have proper `aria-live="assertive"`
- Clear heading structure
- Keyboard accessible actions

#### 3.4.2 Empty State Component

Displays when content is unavailable or not yet created.

**Base Structure:**
- Illustration
- Message title
- Description text
- Action button (when applicable)

**Role Adaptations:**

| Role | Empty State Tone | Call to Action | Examples |
|------|------------------|----------------|----------|
| Caregiver | Supportive suggestion | Direct action prompt | "No tasks yet. Create your first task." |
| Organizer | Opportunity framing | Team coordination prompt | "Team calendar open. Add your first event." |
| Supporter | Low-pressure | Optional engagement | "No tasks assigned. Update your availability." |
| Care Recipient | Empowering | Control emphasis | "No plans today. Add activities or express needs." |

**Accessibility:**
- Illustration: Proper alt text
- Clear heading structure
- Action button: Proper labeling
- Context: Screen reader announcement of empty state

## 4. Component Composition Patterns

### 4.1 Dashboard Composition

The home dashboard uses a consistent composition pattern with role-specific modules:

**Base Structure:**
- Status summary (top)
- Priority items (middle)
- Recent activity (bottom)
- Quick action button (floating)

**Role-Specific Composition:**

| Role | Top Module | Middle Module | Bottom Module | Quick Action |
|------|------------|--------------|---------------|--------------|
| Caregiver | Care Recipient Status | Today's Priority Tasks | Recent Updates | Add Task |
| Organizer | Team Status | Schedule Conflicts | Recent Team Activity | Add Event |
| Supporter | Availability Toggle | My Assigned Tasks | Upcoming Commitments | Update Availability |
| Care Recipient | Today's Plan | Upcoming Visits/Events | Messages & Updates | Express Need |

### 4.2 Detail View Composition

Detail screens follow a consistent pattern:

**Base Structure:**
- Header with breadcrumbs
- Primary content area
- Related items section
- Action bar (bottom)

**Role-Specific Adaptations:**

| Role | Primary Focus | Related Items | Available Actions |
|------|--------------|---------------|-------------------|
| Caregiver | Complete details | Related tasks, history | Edit, Share, Complete |
| Organizer | Coordination details | Team assignments, conflicts | Assign, Reschedule, Message |
| Supporter | Task instructions | Time commitment, contact | Accept, Complete, Communicate |
| Care Recipient | Simplified view | Privacy settings, preferences | Privacy controls, Feedback |

## 5. Accessibility Implementation

### 5.1 Universal Requirements

All components must implement:

- Proper ARIA roles, states, and properties
- Keyboard navigation support
- Touch target sizing (minimum 44x44px)
- Color contrast (4.5:1 for text, 3:1 for UI elements)
- Focus indicators
- Screen reader support
- Reduced motion alternatives
- Text scaling support

### 5.2 Role-Specific Considerations

| Role | Specific Considerations |
|------|-------------------------|
| Caregiver | Quick task entry shortcuts, efficient flows for repeated tasks |
| Organizer | Bulk operations, keyboard shortcuts for frequent actions |
| Supporter | Simplified interfaces, clear accept/decline actions |
| Care Recipient | Enhanced text scaling, voice input priority, simplified interactions |

## 6. Implementation Guidelines

### 6.1 Development Approach

- Build components as independent, reusable elements
- Use composition over inheritance
- Implement role-based props for adaptation
- Use context providers for shared state (user role, permissions)
- Maintain separation between presentation and logic

### 6.2 Performance Considerations

- Lazy load non-critical components
- Use virtualization for long lists
- Optimize render performance (memoization, etc.)
- Implement skeleton screens for loading states
- Reduce JavaScript payload through code splitting

### 6.3 Naming Conventions

| Entity | Format | Example |
|--------|--------|---------|
| Component | PascalCase | `TaskCard` |
| Props | camelCase | `onTaskComplete` |
| State | camelCase | `isExpanded` |
| CSS Classes | kebab-case | `task-card--overdue` |
| CSS Variables | --kebab-case | `--color-primary` |

## 7. Component Examples

### 7.1 Task Card Implementation

```jsx
// Simplified pseudocode example
const TaskCard = ({ 
  task, 
  userRole, 
  onComplete, 
  onAssign, 
  onEdit 
}) => {
  const [expanded, setExpanded] = useState(false);
  
  // Determine available actions based on role
  const actions = getActionsForRole(userRole, task);
  
  // Determine content detail level based on role
  const contentDetail = getContentDetailForRole(userRole);
  
  return (
    <div 
      className={`task-card ${task.status}`}
      aria-expanded={expanded}
    >
      <div className="task-card__header">
        <h3>{task.title}</h3>
        <span className="task-card__category">{task.category}</span>
        {/* Show priority indicator based on role visibility */}
        {shouldShowPriority(userRole) && (
          <PriorityIndicator level={task.priority} />
        )}
      </div>
      
      {/* Basic information visible to all roles */}
      <div className="task-card__basic-info">
        <span className="due-date">{formatDate(task.dueDate)}</span>
        <StatusIndicator status={task.status} />
      </div>
      
      {/* Progressive disclosure of details */}
      {expanded && (
        <div className="task-card__details">
          {/* Content adapted to role */}
          {contentDetail === 'full' && (
            <>
              <p>{task.description}</p>
              <AssigneeInfo user={task.assignee} />
              <TaskNotes notes={task.notes} />
            </>
          )}
          
          {contentDetail === 'limited' && (
            <>
              <p>{task.description}</p>
              <AssigneeInfo user={task.assignee} />
            </>
          )}
          
          {contentDetail === 'minimal' && (
            <p>{task.description}</p>
          )}
        </div>
      )}
      
      {/* Actions vary by role */}
      <div className="task-card__actions">
        {actions.canComplete && (
          <Button onClick={onComplete}>Complete</Button>
        )}
        
        {actions.canAssign && (
          <Button onClick={onAssign}>Assign</Button>
        )}
        
        {actions.canEdit && (
          <Button onClick={onEdit}>Edit</Button>
        )}
        
        <Button 
          onClick={() => setExpanded(!expanded)}
          aria-label={expanded ? "Show less" : "Show more"}
        >
          {expanded ? "Less" : "More"}
        </Button>
      </div>
    </div>
  );
};
```

### 7.2 Status Card Implementation

```jsx
// Simplified pseudocode example
const StatusCard = ({
  status,
  userRole,
  lastUpdated,
  metrics,
  onUpdate
}) => {
  // Determine visible metrics based on role
  const visibleMetrics = getVisibleMetricsForRole(userRole, metrics);
  
  // Determine available actions based on role
  const canUpdate = canUpdateStatus(userRole);
  
  // Determine tone and emphasis based on role
  const {statusMessage, emphasis} = getStatusMessageForRole(userRole, status);
  
  return (
    <div className={`status-card status-card--${emphasis}`}>
      <div className="status-card__header">
        <h2>Status Update</h2>
        <span className="status-card__timestamp">
          Updated {formatRelativeTime(lastUpdated)}
        </span>
      </div>
      
      <div className="status-card__message">
        <p>{statusMessage}</p>
      </div>
      
      {visibleMetrics.length > 0 && (
        <div className="status-card__metrics">
          {visibleMetrics.map(metric => (
            <MetricDisplay
              key={metric.id}
              name={metric.name}
              value={metric.value}
              unit={metric.unit}
              trend={metric.trend}
            />
          ))}
        </div>
      )}
      
      <div className="status-card__actions">
        {canUpdate && (
          <Button onClick={onUpdate}>Update Status</Button>
        )}
        
        {userRole === 'care_recipient' && (
          <PrivacyControl entityType="status" />
        )}
      </div>
    </div>
  );
};
```

## 8. Next Steps

1. Create detailed specifications for each component
2. Develop prototype components for testing
3. Validate component adaptations across all roles
4. Create comprehensive examples for engineering implementation
5. Develop component documentation for the design system

## Change Log

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0.0 | 2025-04-14 | Product Lead | Initial document |
