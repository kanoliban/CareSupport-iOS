# Phase 4: Cross-Role Integration Specification

**Version:** 1.0.0  
**Last Updated:** 2025-04-15  
**Status:** Draft  
**Document Location:** `/06_Core_Platform/Integration/Cross_Role_Integration.md`

## 1. Overview

This document defines the comprehensive Cross-Role Integration framework for CareSupport's unified platform, representing the first sprint of Phase 4 (Integration & Refinement). It establishes how multiple roles interact seamlessly, how users with multiple roles navigate between contexts, how permissions resolve across role boundaries, and how notifications unify across the platform.

### 1.1 Related Documents

- `/01_Product_Strategy/CareSupport_Emotional_Intelligence_Constitution.md`
- `/02_Research/Relationship_Model.md`
- `/03_Information_Architecture/Navigation_System.md`
- `/07_Technical/Implementation_Guides/Permission_Framework.md`
- `/06_Core_Platform/Role_Specific/Caregiver/Caregiver_Experience.md`
- `/06_Core_Platform/Role_Specific/Care_Recipient/Care_Recipient_Experience.md`
- `/06_Core_Platform/Role_Specific/Organizer/Organizer_Experience.md`
- `/06_Core_Platform/Role_Specific/Supporter/Supporter_Experience.md`

### 1.2 Purpose & Goals

- Define the comprehensive multi-role user experience
- Establish a consistent and intuitive role-switching interface
- Create a permission conflict resolution system
- Design a unified notification architecture that works across roles
- Maintain emotional intelligence principles during role transitions

### 1.3 Integration Context

The CareSupport platform has successfully developed focused experiences for each role:

- **Caregiver Experience:** Daily care management with comprehensive status monitoring
- **Care Recipient Experience:** Person-centered controls with agency and privacy emphasis
- **Organizer Experience:** Team coordination with scheduling and assignment capabilities
- **Supporter Experience:** Low-pressure, boundary-respecting contribution opportunities

Phase 4 integration brings these experiences together, addressing common scenarios where:
- A user holds multiple roles (e.g., both Caregiver and Organizer)
- Actions cross role boundaries (e.g., a task created by an Organizer for a Caregiver)
- Information flows between roles (e.g., updates from Care Recipient to Supporters)
- Notifications need to reach the right people at the right time

## 2. Multi-Role User Experience

### 2.1 Core Principles

The multi-role user experience adheres to these key principles:

1. **Clear Context:** Users should always understand which role context they're operating in
2. **Seamless Transitions:** Role switching should be fluid and maintain task continuity
3. **Information Separation:** Role-specific data should remain appropriately segregated
4. **Permission Clarity:** Users should understand how their permissions change by role
5. **Emotional Consistency:** Emotional intelligence principles apply across all roles

### 2.2 Multi-Role User Stories

| As a... | I want to... | So that... | Priority |
|---------|-------------|------------|----------|
| Multi-role user | Clearly see which role I'm currently using | I understand my current capabilities and context | 🔴 MUST |
| Multi-role user | Switch between my different roles easily | I can perform different functions without friction | 🔴 MUST |
| Multi-role user | Maintain continuity when switching roles | I don't lose my place in a workflow | 🔴 MUST |
| Multi-role user | See notifications across all my roles | I don't miss important information | 🔴 MUST |
| Multi-role user | Understand different permissions by role | I know what I can and cannot do in each context | 🟠 SHOULD |
| Multi-role user | Have personalized settings for each role | My experience is optimized for different contexts | 🟠 SHOULD |
| Multi-role user | See a unified task list across roles | I can prioritize all my responsibilities | 🟡 COULD |
| Multi-role user | Create cross-role workflows | I can streamline complex care processes | 🟡 COULD |

### 2.3 Role Distribution Patterns

Based on the Relationship Model, we've identified these common multi-role combinations:

| Primary Role | Secondary Role(s) | Frequency | Typical Context |
|--------------|-------------------|-----------|-----------------|
| Caregiver | Organizer | Common | Primary caregiver who also coordinates other helpers |
| Organizer | Supporter | Common | Family member who coordinates but also helps occasionally |
| Caregiver | Supporter | Uncommon | Primary caregiver who helps with other care recipients |
| Supporter | Supporter | Common | Person who helps multiple care recipients |
| Organizer | Organizer | Uncommon | Person who coordinates for multiple care recipients |

### 2.4 Role Context Management

To manage multiple-role users, the system implements:

- **Role Context State:** Current active role stored in session
- **Role Context Persistence:** Remembers last active role per care circle
- **Separate State Slices:** Maintains distinct state for each role
- **Shared Identity:** User profile remains consistent across roles
- **Cross-Role Awareness:** System understands all roles a user holds

## 3. Role-Switching Interface

### 3.1 Role-Switching User Stories

| As a... | I want to... | So that... | Priority |
|---------|-------------|------------|----------|
| Multi-role user | Switch roles from any screen | I can change context wherever I am | 🔴 MUST |
| Multi-role user | See visual confirmation of my current role | I always know my context | 🔴 MUST |
| Multi-role user | Return to the same location after switching | I don't lose my place | 🔴 MUST |
| Multi-role user | See a preview of what's awaiting in another role | I can decide if I need to switch | 🟠 SHOULD |
| Multi-role user | Customize which role is my default for a care circle | I start in my most common context | 🟠 SHOULD |
| Multi-role user | Switch to a specific role for a specific task | I can use the optimal context for each action | 🟡 COULD |

### 3.2 Interface Components & Interactions

#### 3.2.1 Role Indicator Component

A persistent visual indicator appears in the navigation header, showing:
- Current active role
- Visual icon representing the role
- Color theme associated with role

```
┌─────────────────────────────────────┐
│ 🧑‍⚕️ Caregiver ▼                       │
└─────────────────────────────────────┘
```

#### 3.2.2 Role Switcher Dropdown

Tapping the role indicator opens a dropdown menu:

```
┌─────────────────────────────────────┐
│ 🧑‍⚕️ Caregiver ▼                       │
└─────────────────────────────────────┘
   ┌─────────────────────────────────┐
   │ ✓ 🧑‍⚕️ Caregiver                  │
   │   📋 Organizer (3 tasks due)     │
   │   🤝 Supporter                   │
   └─────────────────────────────────┘
```

Components include:
- Current role (checkmarked)
- Available alternate roles
- Optional status indicators showing pending items

#### 3.2.3 Role Switching Dialog

When switching roles with unsaved changes or in-progress workflows:

```
┌────────────────────────────────────────┐
│ Switch to Organizer role?              │
│                                        │
│ You have unsaved changes to this task. │
│ Would you like to:                     │
│                                        │
│ ○ Save changes and switch              │
│ ○ Discard changes and switch           │
│ ○ Cancel and stay in Caregiver role    │
│                                        │
│ ┌──────────┐        ┌───────────────┐  │
│ │ Cancel   │        │ Switch Role   │  │
│ └──────────┘        └───────────────┘  │
└────────────────────────────────────────┘
```

#### 3.2.4 Role-Specific Navigation State

After switching roles, the navigation maintains state when possible:
- Same tab remains selected if available in new role
- Same entity (task, event) remains focused if accessible
- Falls back to role home screen if current context unavailable

#### 3.2.5 Role-Specific Theme

Visual design elements adapt to role:
- Color theme shifts to role-specific palette
- Navigation emphasis changes to role-primary actions
- Content organization adjusts to role priorities

### 3.3 Technical Implementation

#### 3.3.1 Role Context Management

**Data Model:**
```json
{
  "user_role_context": {
    "user_id": "user123",
    "current_role": "caregiver",
    "care_circle_id": "circle456",
    "last_locations": {
      "caregiver": {
        "route": "/care/tasks",
        "params": { "filter": "today" },
        "timestamp": "2025-04-14T14:30:00Z"
      },
      "organizer": {
        "route": "/connect/team",
        "params": {},
        "timestamp": "2025-04-14T10:15:00Z"
      }
    },
    "default_roles": {
      "circle456": "caregiver",
      "circle789": "supporter"
    },
    "role_permissions": {
      "caregiver": ["view_task", "create_task", "assign_task", "view_health"],
      "organizer": ["view_task", "create_task", "assign_task", "manage_team"]
    }
  }
}
```

**State Management:**
```javascript
// Role context state slice (pseudocode)
const roleContextState = {
  // Current active role
  activeRole: {
    role: 'caregiver',
    careCircleId: 'circle456',
    timestamp: '2025-04-14T14:30:00Z'
  },
  
  // Available roles for user
  availableRoles: [
    {
      role: 'caregiver',
      careCircleId: 'circle456',
      statusIndicators: {
        tasks: 5,
        notifications: 2
      }
    },
    {
      role: 'organizer',
      careCircleId: 'circle456',
      statusIndicators: {
        tasks: 3,
        notifications: 1
      }
    }
  ],
  
  // Location memory by role
  lastLocations: {
    'caregiver:circle456': {
      route: '/care/tasks',
      params: { filter: 'today' },
      scrollPosition: { x: 0, y: 124 }
    },
    'organizer:circle456': {
      route: '/connect/team',
      params: {},
      scrollPosition: { x: 0, y: 0 }
    }
  },
  
  // Role switching state
  switching: {
    inProgress: false,
    targetRole: null,
    hasUnsavedChanges: false,
    continueAction: null
  }
};

// Role context actions (pseudocode)
const roleContextActions = {
  // Switch to a different role
  switchRole: (role, careCircleId) => async (dispatch, getState) => {
    const state = getState();
    
    // Check for unsaved changes
    if (hasUnsavedChanges(state)) {
      dispatch({ 
        type: 'ROLE_SWITCH_CONFIRMATION_NEEDED',
        payload: { role, careCircleId }
      });
      return;
    }
    
    // Save current location
    const currentRoute = getCurrentRoute(state);
    const currentParams = getCurrentParams(state);
    const scrollPosition = getCurrentScrollPosition(state);
    
    dispatch({
      type: 'SAVE_LOCATION',
      payload: {
        roleKey: `${state.roleContext.activeRole.role}:${state.roleContext.activeRole.careCircleId}`,
        location: {
          route: currentRoute,
          params: currentParams,
          scrollPosition
        }
      }
    });
    
    // Switch role
    dispatch({
      type: 'SWITCH_ROLE',
      payload: { role, careCircleId }
    });
    
    // Navigate to saved location or default
    const roleKey = `${role}:${careCircleId}`;
    const savedLocation = state.roleContext.lastLocations[roleKey];
    
    if (savedLocation && isRouteValidForRole(savedLocation.route, role)) {
      navigateTo(savedLocation.route, savedLocation.params);
      setScrollPosition(savedLocation.scrollPosition);
    } else {
      navigateToRoleHome(role);
    }
    
    // Update permissions
    await dispatch(loadPermissionsForRole(role, careCircleId));
  },
  
  // Confirm role switch with unsaved changes
  confirmRoleSwitch: (action) => (dispatch, getState) => {
    const state = getState();
    const { targetRole, careCircleId } = state.roleContext.switching;
    
    if (action === 'save') {
      // Save pending changes
      dispatch(saveChanges()).then(() => {
        dispatch(roleContextActions.switchRole(targetRole, careCircleId));
      });
    } else if (action === 'discard') {
      // Discard changes and switch
      dispatch(discardChanges());
      dispatch(roleContextActions.switchRole(targetRole, careCircleId));
    } else {
      // Cancel switch
      dispatch({ type: 'CANCEL_ROLE_SWITCH' });
    }
  },
  
  // Set default role for care circle
  setDefaultRole: (role, careCircleId) => ({
    type: 'SET_DEFAULT_ROLE',
    payload: { role, careCircleId }
  }),
  
  // Load permissions for role
  loadPermissionsForRole: (role, careCircleId) => async (dispatch) => {
    const permissions = await api.getUserPermissions(role, careCircleId);
    dispatch({
      type: 'SET_ROLE_PERMISSIONS',
      payload: { role, permissions }
    });
  }
};
```

#### 3.3.2 API Endpoints

- `GET /user/roles` - Get user's available roles
- `GET /user/roles/:role/status` - Get status indicators for role
- `PUT /user/active-role` - Update active role
- `GET /user/permissions/:role` - Get permissions for role
- `PUT /user/roles/:role/default` - Set default role for care circle

#### 3.3.3 Role-Specific Navigation Guards

```javascript
// Navigation guard pseudocode
function roleNavigationGuard(to, from, next) {
  const store = useStore();
  const { activeRole } = store.state.roleContext;
  
  // Check if route requires specific role
  if (to.meta.requiredRoles && !to.meta.requiredRoles.includes(activeRole.role)) {
    // Option 1: Suggest role switch
    if (userHasRole(to.meta.requiredRoles[0])) {
      openRoleSwitchModal(to.meta.requiredRoles[0], () => {
        // After switch, continue navigation
        next();
      });
      return;
    }
    
    // Option 2: Redirect to not-authorized view
    next({ name: 'not-authorized' });
    return;
  }
  
  // Continue navigation
  next();
}
```

## 4. Permission Conflict Resolution

### 4.1 Permission Conflict User Stories

| As a... | I want to... | So that... | Priority |
|---------|-------------|------------|----------|
| Multi-role user | Understand my permissions in each role | I know what actions I can take | 🔴 MUST |
| Multi-role user | Have appropriate permissions applied automatically | I don't need to manually adjust permissions | 🔴 MUST |
| System | Resolve permission conflicts consistently | User experience is predictable | 🔴 MUST |
| Caregiver | Maintain appropriate boundaries with my Organizer role | I respect care recipient privacy | 🔴 MUST |
| Care Recipient | Have my privacy preferences respected across roles | My sensitive information remains protected | 🔴 MUST |
| Organizer | Access necessary coordination information | I can perform my role effectively | 🟠 SHOULD |
| Care Team | Have transparent permission visibility | We understand information boundaries | 🟠 SHOULD |

### 4.2 Permission Conflict Scenarios

| Scenario | Roles Involved | Conflict | Resolution Approach |
|----------|----------------|----------|---------------------|
| Health Data Access | Caregiver + Organizer | Caregiver can see health data, Organizer cannot | Default to most restrictive (Organizer cannot see) unless explicitly switched to Caregiver role |
| Task Assignment | Caregiver + Supporter | Caregiver can assign tasks, Supporter cannot | Use role-specific UI that shows/hides assignment capabilities |
| Team Management | Organizer + Supporter | Organizer can manage team, Supporter cannot | Provide clear role indicator when in management interfaces |
| Privacy Settings | Care Recipient + Any | Care Recipient controls privacy, other roles follow rules | Care Recipient settings always override regardless of user's other roles |
| Content Creation | Caregiver + Organizer | Different content types available by role | Show combined creation options with role requirements indicated |

### 4.3 Resolution Framework

The permission conflict resolution follows this hierarchy:

1. **Explicit Context:** Active role's permissions apply when in role context
2. **Most Restrictive Rule:** When conflict exists, apply most restrictive permission
3. **Care Recipient Overrides:** Privacy settings from Care Recipient always take precedence
4. **Role Switching Prompt:** Suggest role switching when attempting restricted actions
5. **Audit Tracking:** Log all permission decisions for accountability

### 4.4 Technical Implementation

#### 4.4.1 Multi-Role Permission Evaluation

```javascript
// Permission evaluation for multi-role users (pseudocode)
function evaluatePermission(userId, permission, objectId) {
  // Get user's current role context
  const roleContext = getUserRoleContext(userId);
  const activeRole = roleContext.activeRole.role;
  
  // Get all roles for user
  const userRoles = getAllUserRoles(userId);
  
  // Check if active role has permission
  const activeRolePermission = checkRolePermission(
    userId, 
    activeRole, 
    permission, 
    objectId
  );
  
  // If active role has permission, check for constraints
  if (activeRolePermission.granted) {
    // Check for Care Recipient privacy settings
    const privacyConstraint = checkPrivacyConstraints(
      userId,
      permission,
      objectId
    );
    
    if (privacyConstraint.restricted) {
      return {
        granted: false,
        reason: privacyConstraint.reason,
        suggestedAction: privacyConstraint.suggestedAction
      };
    }
    
    // Permission granted in current role context
    return {
      granted: true,
      role: activeRole
    };
  }
  
  // Active role doesn't have permission, check if any role does
  const otherRoleWithPermission = userRoles
    .filter(role => role !== activeRole)
    .find(role => {
      const check = checkRolePermission(userId, role, permission, objectId);
      return check.granted;
    });
  
  // If another role has permission, suggest role switch
  if (otherRoleWithPermission) {
    return {
      granted: false,
      reason: 'permission_in_other_role',
      suggestedRole: otherRoleWithPermission,
      suggestedAction: 'role_switch'
    };
  }
  
  // No role has permission
  return {
    granted: false,
    reason: 'no_permission'
  };
}
```

#### 4.4.2 Permission-Aware Components

Components detect and adapt to multi-role permission contexts:

```jsx
// React component with permission awareness (pseudocode)
const PermissionAwareAction = ({ 
  permission,
  objectId,
  activeRoleOnly,
  children,
  fallback,
  onPermissionDenied
}) => {
  const { hasPermission, activeRole, availableInRoles } = usePermission(
    permission, 
    objectId, 
    activeRoleOnly
  );
  
  // Permission granted in current role
  if (hasPermission) {
    return children;
  }
  
  // Permission available in another role
  if (!activeRoleOnly && availableInRoles.length > 0) {
    return (
      <RoleSwitchPrompt 
        roles={availableInRoles} 
        permission={permission}
        onSwitch={() => {
          // After role switch, the component will re-render with permission
        }}
        onDismiss={() => onPermissionDenied?.()}
      >
        {fallback}
      </RoleSwitchPrompt>
    );
  }
  
  // No permission in any role
  return fallback || null;
};
```

#### 4.4.3 Role Switch Permission Prompt

When a user attempts an action available in another role:

```
┌────────────────────────────────────────────┐
│ Switch to Caregiver Role?                  │
│                                            │
│ You need to be in your Caregiver role to   │
│ view Elizabeth's health information.       │
│                                            │
│ Would you like to switch roles now?        │
│                                            │
│ ┌──────────┐        ┌──────────────────┐   │
│ │ Not Now  │        │ Switch to        │   │
│ │          │        │ Caregiver Role   │   │
│ └──────────┘        └──────────────────┘   │
└────────────────────────────────────────────┘
```

#### 4.4.4 Persistent Permission Indicator

A subtle indicator shows permission differences in multi-role contexts:

```
┌─────────────────────────────────────────────┐
│ Health Information                   🔒      │
│                                      │      │
│ Blood Pressure: --/--                │      │
│ Medication Status: --                │      │
│                                      │      │
│ 🔒 Available in Caregiver role       │      │
└─────────────────────────────────────────────┘
```

## 5. Unified Notification System

### 5.1 Notification User Stories

| As a... | I want to... | So that... | Priority |
|---------|-------------|------------|----------|
| Multi-role user | See all relevant notifications regardless of role | I don't miss important information | 🔴 MUST |
| Multi-role user | Understand which role a notification is for | I know which context to address it in | 🔴 MUST |
| Multi-role user | Take immediate action on notifications | I can respond quickly to urgent matters | 🔴 MUST |
| Multi-role user | Have consistent notification experience across roles | The system feels unified and coherent | 🔴 MUST |
| Multi-role user | Filter or categorize notifications by role | I can focus on specific responsibilities | 🟠 SHOULD |
| Multi-role user | Customize notification preferences by role | I control my notification experience | 🟠 SHOULD |
| Multi-role user | See cross-role impact of my actions | I understand how roles interact | 🟡 COULD |
| System | Avoid duplicate notifications across roles | Users aren't overwhelmed with redundancy | 🟡 COULD |

### 5.2 Unified Notification Framework

#### 5.2.1 Core Principles

1. **Role Awareness:** Notifications maintain role context but appear in a unified feed
2. **De-duplication:** Multiple roles don't result in duplicate notifications
3. **Clear Context:** Each notification indicates which role it's relevant to
4. **Smart Routing:** System directs notifications to appropriate role context
5. **Action-Oriented:** Notifications include direct action options when possible

#### 5.2.2 Notification Types

| Type | Purpose | Priority | Role Relevance |
|------|---------|----------|----------------|
| Task Assignment | Inform about new assigned tasks | High | Caregiver, Supporter |
| Task Update | Updates to task status or details | Medium | All roles |
| Care Status | Changes to care recipient status | High | Caregiver, Organizer |
| Schedule Change | Updates to appointments or events | Medium | All roles |
| Team Update | Changes to care circle or roles | Medium | All roles |
| Care Request | Direct request from care recipient | High | Caregiver |
| System Alert | Critical system information | Critical | All roles |
| Appreciation | Recognition of contributions | Low | All roles |

### 5.3 UI Components & Interactions

#### 5.3.1 Unified Notification Center

```
┌─────────────────────────────────────────────┐
│ Notifications                               │
│                                             │
│ ┌──────┐ ┌────────┐ ┌──────┐ ┌─────────┐    │
│ │ All  │ │ 🧑‍⚕️ Care │ │ 📋 Org│ │ 🤝 Help │    │
│ └──────┘ └────────┘ └──────┘ └─────────┘    │
│                                             │
│ Today                                       │
│ ┌───────────────────────────────────────┐   │
│ │ 🧑‍⚕️ [Task] Medication reminder         │   │
│ │ Elizabeth needs morning medication    │   │
│ │ 20 minutes ago                        │   │
│ └───────────────────────────────────────┘   │
│                                             │
│ ┌───────────────────────────────────────┐   │
│ │ 📋 [Schedule] Team meeting changed     │   │
│ │ Now at 3:00 PM instead of 2:00 PM     │   │
│ │ 1 hour ago                            │   │
│ └───────────────────────────────────────┘   │
│                                             │
│ Yesterday                                   │
│ ┌───────────────────────────────────────┐   │
│ │ 🤝 [Task] New help opportunity         │   │
│ │ Grocery shopping needed next Tuesday  │   │
│ │ Yesterday, 4:30 PM                    │   │
│ └───────────────────────────────────────┘   │
└─────────────────────────────────────────────┘
```

Components include:
- Role-based filter tabs
- Role indicators on each notification
- Chronological grouping
- Action buttons on expandable notifications

#### 5.3.2 Notification Card

```
┌─────────────────────────────────────────────┐
│ 🧑‍⚕️ [Task] Medication reminder               │
│ Elizabeth needs morning medication          │
│ 20 minutes ago                              │
│                                             │
│ Details:                                    │
│ Blood pressure medication                   │
│ Due at 9:00 AM                              │
│                                             │
│ ┌─────────────┐   ┌─────────────────────┐   │
│ │ Snooze      │   │ Mark as Completed   │   │
│ └─────────────┘   └─────────────────────┘   │
└─────────────────────────────────────────────┘
```

#### 5.3.3 Notification Badge with Role Indicators

```
┌──────────────────┐
│ 🔔 5 (🧑‍⚕️ 3, 📋 2) │
└──────────────────┘
```

### 5.4 Technical Implementation

#### 5.4.1 Unified Notification Data Model

```json
{
  "notification": {
    "id": "notif123",
    "user_id": "user456",
    "relevant_roles": ["caregiver"],
    "primary_role": "caregiver",
    "type": "task_reminder",
    "priority": "high",
    "title": "Medication reminder",
    "content": "Elizabeth needs morning medication",
    "created_at": "2025-04-15T09:40:00Z",
    "read": false,
    "read_at": null,
    "actions": [
      {
        "type": "complete_task",
        "label": "Mark as Completed",
        "task_id": "task789",
        "requires_role": "caregiver"
      },
      {
        "type": "snooze",
        "label": "Snooze",
        "options": [
          {"label": "15 minutes", "value": 15},
          {"label": "1 hour", "value": 60},
          {"label": "4 hours", "value": 240}
        ]
      }
    ],
    "related_to": {
      "type": "task",
      "id": "task789"
    },
    "care_circle_id": "circle123"
  }
}
```

#### 5.4.2 Notification API Endpoints

- `GET /notifications` - Get unified notification feed
- `GET /notifications/:role` - Get role-specific notifications
- `PUT /notifications/:id/read` - Mark notification as read
- `POST /notifications/:id/action` - Perform notification action
- `PUT /notifications/batch/read` - Mark multiple notifications as read
- `PUT /notification-preferences` - Update notification preferences

#### 5.4.3 Notification Management

```javascript
// Notification state management (pseudocode)
const notificationState = {
  items: {
    byId: {},
    allIds: []
  },
  filters: {
    roles: ['all'],
    readStatus: 'all',
    dateRange: '30days'
  },
  unreadCounts: {
    total: 5,
    byRole: {
      caregiver: 3,
      organizer: 2,
      supporter: 0
    }
  },
  pagination: {
    currentPage: 1,
    perPage: 20,
    totalPages: 1
  },
  preferences: {
    byRole: {
      caregiver: {
        task_reminder: { push: true, email: false, inApp: true },
        care_status: { push: true, email: true, inApp: true }
        // Other preference mappings...
      },
      organizer: {
        // Role-specific preferences...
      }
    },
    quietHours: {
      enabled: true,
      start: "22:00",
      end: "07:00",
      exceptCritical: true
    }
  },
  loading: false,
  error: null
};

// Notification actions
const notificationActions = {
  // Fetch unified notifications
  fetchNotifications: (filters = {}) => async (dispatch) => {
    dispatch({ type: 'FETCH_NOTIFICATIONS_START' });
    try {
      const response = await api.getNotifications(filters);
      dispatch({ 
        type: 'FETCH_NOTIFICATIONS_SUCCESS', 
        payload: response
      });
    } catch (error) {
      dispatch({ 
        type: 'FETCH_NOTIFICATIONS_ERROR', 
        payload: error 
      });
    }
  },
  
  // Mark notification as read
  markAsRead: (notificationId) => async (dispatch) => {
    await api.markNotificationRead(notificationId);
    dispatch({
      type: 'MARK_NOTIFICATION_READ',
      payload: { notificationId }
    });
  },
  
  // Perform notification action
  performAction: (notificationId, actionType, actionData) => async (dispatch) => {
    dispatch({ 
      type: 'NOTIFICATION_ACTION_START', 
      payload: { notificationId, actionType }
    });
    
    try {
      const result = await api.performNotificationAction(
        notificationId, 
        actionType, 
        actionData
      );
      
      dispatch({ 
        type: 'NOTIFICATION_ACTION_SUCCESS', 
        payload: { notificationId, actionType, result }
      });
      
      // Handle role-specific updates
      if (result.roleSwitch) {
        dispatch(roleContextActions.switchRole(
          result.roleSwitch.role,
          result.roleSwitch.careCircleId
        ));
      }
      
      return result;
    } catch (error) {
      dispatch({ 
        type: 'NOTIFICATION_ACTION_ERROR', 
        payload: { notificationId, actionType, error } 
      });
      throw error;
    }
  },
  
  // Update notification filters
  updateFilters: (filters) => ({
    type: 'UPDATE_NOTIFICATION_FILTERS',
    payload: filters
  }),
  
  // Update notification preferences
  updatePreferences: (preferences) => async (dispatch) => {
    dispatch({ 
      type: 'UPDATE_NOTIFICATION_PREFERENCES_START', 
      payload: preferences 
    });
    
    try {
      await api.updateNotificationPreferences(preferences);
      dispatch({ 
        type: 'UPDATE_NOTIFICATION_PREFERENCES_SUCCESS', 
        payload: preferences
      });
    } catch (error) {
      dispatch({ 
        type: 'UPDATE_NOTIFICATION_PREFERENCES_ERROR', 
        payload: error 
      });
      throw error;
    }
  }
};
```

#### 5.4.4 Role-Aware Push Notifications

Push notifications include role context for clear routing:

```json
{
  "push_notification": {
    "title": "[Caregiver] Medication Reminder",
    "body": "Elizabeth needs morning medication",
    "role_context": "caregiver",
    "deep_link": "/care/tasks/task789",
    "action_buttons": [
      {
        "title": "Complete",
        "action": "complete_task",
        "data": { "task_id": "task789" }
      },
      {
        "title": "Snooze",
        "action": "snooze",
        "data": { "minutes": 15 }
      }
    ],
    "require_role_switch": true
  }
}
```

## 6. Emotional Intelligence Integration

### 6.1 Emotional Intelligence Principles

The Cross-Role Integration maintains consistency with the Emotional Intelligence Constitution through:

| Principle | Integration Approach | Implementation Examples |
|-----------|----------------------|------------------------|
| Care Before Management | Focus on human impact during role transitions | "Switching to Caregiver to respond to Elizabeth's need" vs "Switching roles" |
| Invisible Understanding | Contextual awareness during role shifts | System understanding of when role switching is appropriate |
| Dignity Through Agency | Clear control over role context and actions | Explicit role switching with consent, never automatic |
| Whole-Person Recognition | Acknowledging multi-faceted nature of care relationships | Support for users who naturally hold multiple care roles |

### 6.2 Role Transition Emotional Intelligence

When users switch between roles, the emotional tone adjusts appropriately:

| Transition | Emotional Consideration | Approach |
|------------|-------------------------|----------|
| To Caregiver | Increased emotional investment | Supportive, acknowledging care burden |
| To Organizer | Shift to coordination mindset | Clear, structured, logistics-focused |
| To Supporter | Bounded commitment emphasis | Appreciative, focused on specific contributions |
| To Care Recipient | Full agency and control | Dignity-preserving, choice-centered |

### 6.3 Role-Specific Language Patterns

Communication adapts based on active role context:

| Role | Notification Style | Dialog Tone | Guidance Approach |
|------|-------------------|-------------|-------------------|
| Caregiver | Supportive, acknowledging effort | Warm, empathetic | Suggestion-based, resource-oriented |
| Organizer | Clear, action-oriented | Structured, efficient | Process-focused, coordination-oriented |
| Supporter | Appreciative, boundary-respecting | Warm, straightforward | Task-specific, time-aware |
| Care Recipient | Agency-centered, choice-focused | Respectful, never infantilizing | Option-presenting, control-emphasizing |

### 6.4 Multi-Role Stress Awareness

The system recognizes potential stress in managing multiple roles:

- **Workload Visibility:** Clear visualization of tasks across roles
- **Role Separation:** Support for mental separation between roles
- **Cognitive Load Management:** Minimizing unnecessary context switching
- **Self-Care Prompts:** Appropriate reminders based on multi-role burden

## 7. Implementation Guidance

### 7.1 Development Approach

The Cross-Role Integration should be implemented using this phased approach:

1. **Foundation Layer:**
   - Role context management system
   - Permission resolution framework
   - Unified notification data model

2. **UI Components:**
   - Role indicator and switcher
   - Role-aware action components
   - Unified notification center

3. **Role-Specific Adaptations:**
   - Theme transitions
   - Content adaptation
   - Permission boundary visualizations

4. **Edge Cases & Refinements:**
   - Multi-role conflict resolution
   - State preservation optimization
   - Performance enhancements

### 7.2 Testing Strategy

Testing should focus on these key areas:

| Test Area | Approach | Priority |
|-----------|----------|----------|
| Role Switching | Verify state preservation and correct context | High |
| Permission Boundaries | Confirm appropriate enforcement across roles | High |
| Notification Delivery | Test unified delivery with role context | High |
| Multi-Role Scenarios | Test common workflows across roles | High |
| Edge Cases | Test conflict resolution and boundary conditions | Medium |
| Performance | Measure impact of role context on performance | Medium |
| Accessibility | Verify role context is accessible | Medium |

### 7.3 Success Metrics

The Cross-Role Integration should be measured against these metrics:

- **Role Switch Time:** Under 500ms for complete context change
- **Permission Accuracy:** 100% correct permission enforcement
- **Notification Unification:** No duplicate notifications across roles
- **State Preservation:** 95%+ success in maintaining context during switch
- **User Satisfaction:** 90%+ satisfaction with multi-role management
- **Error Rate:** <1% errors during role transitions

## 8. Next Steps

1. Implement Role Context Management foundation
2. Develop Role Switcher UI component
3. Create Permission Resolution System
4. Build Unified Notification Center
5. Test with multi-role user scenarios

## Change Log

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0.0 | 2025-04-15 | Product Lead | Initial document |
