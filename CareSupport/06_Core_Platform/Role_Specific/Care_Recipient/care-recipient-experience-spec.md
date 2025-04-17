# Care Recipient Experience

**Version:** 1.0.0  
**Last Updated:** 2025-04-14  
**Status:** Draft  
**Document Location:** `/06_Core_Platform/Role_Specific/Care_Recipient/Care_Recipient_Experience.md`

## 1. Overview

This document defines the comprehensive Care Recipient Experience for CareSupport's unified platform. The Care Recipient role represents the individual receiving care, who maintains agency, dignity, and an active voice in their care journey despite potential health challenges or limitations.

The Care Recipient Experience builds upon the foundation and core modules established in Phases 1 and 2, creating a tailored interface that acknowledges the care recipient as a full participant in their care - not merely a passive recipient of services. This experience embodies our commitment to dignity-preserving care that enhances independence rather than diminishing it.

### 1.1 Related Documents

- `/01_Product_Strategy/CareSupport_Emotional_Intelligence_Constitution.md`
- `/02_Research/Relationship_Model.md`
- `/03_Information_Architecture/Navigation_System.md`
- `/07_Technical/Implementation_Guides/Permission_Framework.md`
- `/08_Design_Standards/Component_Library/Component_Overview.md`
- `/05_Care_Plan_Setup/Care_Recipient/[06]_CarePlan-Complete.md`

### 1.2 Purpose & Scope

This specification:
- Defines the complete user experience for the Care Recipient role
- Details the four core modules that comprise the Care Recipient experience
- Provides implementation guidance for developers, designers, and QA
- Establishes integration points with other system components
- Outlines testing requirements for all Care Recipient-related functionality

### 1.3 Care Recipient Role Context

Care Recipients are individuals who are receiving care support, with key characteristics including:

- **Agency:** Maintaining control and decision-making authority when possible
- **Dignity:** Preserving personal dignity through respectful interaction design
- **Voice:** Having meaningful input into care decisions and processes
- **Varying Capabilities:** Potentially dealing with cognitive, physical, or emotional challenges
- **Privacy Concerns:** Maintaining appropriate control over personal information

### 1.4 Design Philosophy

The Care Recipient Experience is built on five core principles:

1. **Dignity Through Agency:** Preserving autonomy through design that emphasizes choice and control
2. **Simplicity Without Infantilization:** Creating accessible interfaces that never condescend
3. **Privacy by Design:** Giving granular control over personal information sharing
4. **Voice Preservation:** Ensuring the care recipient's preferences and needs are consistently expressed
5. **Adaptive Accessibility:** Accommodating varying abilities without assuming limitation

## 2. Experience Overview

The Care Recipient Experience centers around four interconnected modules that work together to create a cohesive, empowering experience:

1. **Today's Plan View:** A clear, manageable view of daily care activities and events
2. **Express Needs System:** Tools to communicate care needs while preserving dignity
3. **Privacy Control Center:** Granular control over personal information sharing
4. **Journal & Reflection Tools:** Personal space for thoughts, observations, and wellbeing tracking

These modules are designed to work together, creating a balanced experience that provides structure without constraint, information without overwhelm, and support without dependency.

### 2.1 User Journey Overview

The typical Care Recipient journey follows this pattern:

1. **Onboarding:** Introduction to the platform with privacy and preference setting
2. **Daily Routine:** Viewing plan, expressing needs, and monitoring care
3. **Boundary Management:** Controlling information sharing and privacy
4. **Personal Reflection:** Recording thoughts, tracking wellbeing, and reviewing care
5. **Ongoing Engagement:** Maintaining voice in care decisions and activities

### 2.2 Interaction with Other Roles

| Role | Interaction Pattern | Primary Touchpoints |
|------|---------------------|---------------------|
| Caregiver | Structured communication with privacy controls | Need expression, Plan adjustments, Journal sharing (selective) |
| Organizer | Limited direct interaction, primarily for coordination | Schedule visibility, Privacy preferences |
| Supporter | Minimal, task-specific interaction | Need requests, Appreciation messaging |

## 3. Core Modules

### 3.1 Today's Plan View

#### 3.1.1 Purpose & Overview

The Today's Plan View provides Care Recipients with a clear, accessible overview of their daily care schedule. Unlike complex calendar interfaces, this module presents activities, medications, appointments, and visits in a simple, chronological format that focuses on what matters to the Care Recipient, creating predictability without overwhelming with logistics.

This module creates a sense of structure while maintaining flexibility, and importantly, distinguishes between care activities and personal activities—reinforcing that the person's life extends beyond their care needs.

#### 3.1.2 User Stories

| As a... | I want to... | So that... | Priority |
|---------|-------------|------------|----------|
| Care Recipient | See what's happening today | I know what to expect | 🔴 MUST |
| Care Recipient | Know who's visiting me | I can prepare mentally and emotionally | 🔴 MUST |
| Care Recipient | See when I need to take medications | I can maintain my health independently when possible | 🔴 MUST |
| Care Recipient | View my appointments and activities | I can participate in planning my day | 🔴 MUST |
| Care Recipient | Add my own activities to my schedule | I maintain control of my personal time | 🟠 SHOULD |
| Care Recipient | Indicate my preferences for timing | My care accommodates my natural rhythms | 🟠 SHOULD |
| Care Recipient | Get gentle reminders for important events | I don't miss things that matter | 🟡 COULD |
| Care Recipient | Share my plan with specific people | I can coordinate with family or friends | 🟡 COULD |

#### 3.1.3 UI Components & Interactions

**Primary Components:**
- **Day Overview Panel:** Summary of the day's activities and structure
- **Timeline View:** Chronological representation of the day's events
- **Activity Cards:** Individual activities with essential details
- **Day Navigation:** Controls for moving between days

**Day Overview Panel:**
```
┌─────────────────────────────────────────────┐
│ Today • Tuesday, April 15                   │
│                                             │
│ 3 Care Activities • 1 Appointment           │
│ 2 Medications • 1 Personal Activity         │
│                                             │
│ First Activity: Breakfast at 8:00 AM        │
└─────────────────────────────────────────────┘
```

**Timeline View:**
```
┌─────────────────────────────────────────────┐
│ Morning                                     │
│ ┌─────────────────────────────────────────┐ │
│ │ 8:00 AM • Breakfast                     │ │
│ │ With Sarah (Caregiver)                  │ │
│ └─────────────────────────────────────────┘ │
│                                             │
│ ┌─────────────────────────────────────────┐ │
│ │ 9:30 AM • Morning Medication            │ │
│ │ Blood pressure medication, vitamin      │ │
│ └─────────────────────────────────────────┘ │
│                                             │
│ Afternoon                                   │
│ ┌─────────────────────────────────────────┐ │
│ │ 2:00 PM • Doctor Appointment            │ │
│ │ Dr. Johnson • Michael will drive        │ │
│ └─────────────────────────────────────────┘ │
│                                             │
│ ┌─────────────────────────────────────────┐ │
│ │ 4:30 PM • Reading Club (Personal)       │ │
│ │ Added by you                            │ │
│ └─────────────────────────────────────────┘ │
└─────────────────────────────────────────────┘
```

**Activity Card:**
```
┌─────────────────────────────────────────────┐
│ [Icon] 2:00 PM • Doctor Appointment         │
│                                             │
│ Dr. Johnson - Annual Check-up               │
│ Memorial Medical Center                     │
│                                             │
│ Michael will drive you                      │
│ Needs to leave by 1:30 PM                   │
│                                             │
│ ┌──────────────┐  ┌────────────────────┐    │
│ │ Preferences  │  │ Add to Journal     │    │
│ └──────────────┘  └────────────────────┘    │
└─────────────────────────────────────────────┘
```

**Day Navigation:**
```
┌─────────────────────────────────────────────┐
│ ┌──────────┐   Today    ┌──────────┐        │
│ │ Monday   │  Tuesday   │ Wednesday│        │
│ │ Apr 14   │  Apr 15    │ Apr 16   │        │
│ └──────────┘            └──────────┘        │
└─────────────────────────────────────────────┘
```

**Interactions:**
- **Swipe between days:** View past or upcoming days
- **Tap activity:** Expand to show detailed information
- **Long press activity:** Show options menu (preferences, notes, etc.)
- **Tap "Add Activity":** Create a new personal activity
- **Pull down:** Refresh the current day's plan

**Empty States:**
- **No activities:** "No activities scheduled for today. Tap 'Add Activity' to create one."
- **No care activities:** "No care activities scheduled. Your day is open for personal activities."

#### 3.1.4 Technical Implementation

**Data Model:**
```json
{
  "daily_plan": {
    "date": "2025-04-15",
    "care_recipient_id": "recipient123",
    "day_summary": {
      "care_activities_count": 3,
      "medication_count": 2,
      "appointment_count": 1,
      "personal_activities_count": 1,
      "first_activity_time": "08:00",
      "last_activity_time": "16:30"
    },
    "activities": [
      {
        "id": "act123",
        "type": "care_activity",
        "title": "Breakfast",
        "description": "Morning meal with assistance",
        "start_time": "08:00",
        "end_time": "08:30",
        "location": "Home - Kitchen",
        "participants": [
          {
            "user_id": "user456",
            "name": "Sarah",
            "role": "caregiver"
          }
        ],
        "preferences": {
          "food_preferences": "Prefers oatmeal with fruit",
          "assistance_level": "Minimal assistance"
        },
        "created_by": {
          "user_id": "user456",
          "role": "caregiver"
        },
        "updated_at": "2025-04-14T10:30:00Z"
      },
      {
        "id": "act124",
        "type": "medication",
        "title": "Morning Medication",
        "description": "Blood pressure medication, vitamin",
        "start_time": "09:30",
        "end_time": "09:35",
        "medication_details": {
          "names": ["Lisinopril", "Multivitamin"],
          "dosages": ["10mg", "1 tablet"],
          "instructions": "Take with water after breakfast"
        },
        "created_by": {
          "user_id": "user456",
          "role": "caregiver"
        },
        "updated_at": "2025-04-14T10:30:00Z"
      },
      {
        "id": "act125",
        "type": "appointment",
        "title": "Doctor Appointment",
        "description": "Annual check-up with Dr. Johnson",
        "start_time": "14:00",
        "end_time": "15:00",
        "location": "Memorial Medical Center",
        "participants": [
          {
            "user_id": "user789",
            "name": "Michael",
            "role": "supporter",
            "notes": "Will drive you"
          }
        ],
        "transportation": {
          "mode": "car",
          "driver": "Michael",
          "departure_time": "13:30"
        },
        "created_by": {
          "user_id": "user456",
          "role": "caregiver"
        },
        "updated_at": "2025-04-14T10:30:00Z"
      },
      {
        "id": "act126",
        "type": "personal_activity",
        "title": "Reading Club",
        "description": "Weekly book discussion",
        "start_time": "16:30",
        "end_time": "17:30",
        "created_by": {
          "user_id": "recipient123",
          "role": "care_recipient"
        },
        "updated_at": "2025-04-14T11:15:00Z"
      }
    ],
    "preferences": {
      "display_format": "timeline",
      "time_grouping": "morning_afternoon_evening",
      "show_weather": true
    }
  }
}
```

**API Endpoints:**
- `GET /daily-plan/:date` - Get plan for a specific date
- `GET /daily-plan/today` - Get today's plan
- `POST /daily-plan/activities` - Add personal activity
- `PUT /daily-plan/activities/:id/preferences` - Update preferences for activity
- `GET /daily-plan/week-preview` - Get summary for upcoming week

**State Management:**
```javascript
// Plan state management (pseudocode)
const planState = {
  // Current plan data
  plan: {
    date: "2025-04-15",
    daySummary: {},
    activities: [],
    loading: false,
    error: null
  },
  
  // UI state
  ui: {
    selectedDate: "2025-04-15",
    selectedActivityId: null,
    expandedActivities: [],
    displayFormat: "timeline"
  },
  
  // Activity creation state
  activityCreation: {
    isCreating: false,
    newActivityData: {},
    errors: {}
  },
  
  // Preference editing state
  preferenceEditing: {
    isEditing: false,
    activityId: null,
    preferenceData: {},
    errors: {}
  }
};

// Plan actions (pseudocode)
const planActions = {
  // Load plan for a date
  loadPlan: (date) => async (dispatch) => {
    dispatch({ type: 'LOAD_PLAN_START', payload: date });
    try {
      const plan = await api.getDailyPlan(date);
      dispatch({ type: 'LOAD_PLAN_SUCCESS', payload: plan });
    } catch (error) {
      dispatch({ type: 'LOAD_PLAN_ERROR', payload: error });
    }
  },
  
  // Select a different date
  selectDate: (date) => async (dispatch) => {
    dispatch({ type: 'SELECT_DATE', payload: date });
    dispatch(planActions.loadPlan(date));
  },
  
  // Select an activity
  selectActivity: (activityId) => ({
    type: 'SELECT_ACTIVITY',
    payload: activityId
  }),
  
  // Toggle activity expanded state
  toggleExpandActivity: (activityId) => ({
    type: 'TOGGLE_EXPAND_ACTIVITY',
    payload: activityId
  }),
  
  // Start activity creation
  startActivityCreation: () => ({
    type: 'START_ACTIVITY_CREATION'
  }),
  
  // Cancel activity creation
  cancelActivityCreation: () => ({
    type: 'CANCEL_ACTIVITY_CREATION'
  }),
  
  // Create personal activity
  createActivity: (activityData) => async (dispatch) => {
    dispatch({ type: 'CREATE_ACTIVITY_START', payload: activityData });
    try {
      const activity = await api.createPersonalActivity(activityData);
      dispatch({ type: 'CREATE_ACTIVITY_SUCCESS', payload: activity });
      return activity;
    } catch (error) {
      dispatch({ type: 'CREATE_ACTIVITY_ERROR', payload: error });
      throw error;
    }
  },
  
  // Update activity preferences
  updateActivityPreferences: (activityId, preferences) => async (dispatch) => {
    dispatch({ 
      type: 'UPDATE_PREFERENCES_START', 
      payload: { activityId, preferences } 
    });
    try {
      const result = await api.updateActivityPreferences(activityId, preferences);
      dispatch({ 
        type: 'UPDATE_PREFERENCES_SUCCESS', 
        payload: { activityId, preferences: result } 
      });
      return result;
    } catch (error) {
      dispatch({ 
        type: 'UPDATE_PREFERENCES_ERROR', 
        payload: { activityId, error } 
      });
      throw error;
    }
  }
};
```

#### 3.1.5 Integration Points

- **Calendar System:** Sources appointments and scheduled activities
- **Task Management System:** Sources care tasks and related information
- **Medication Management:** Sources medication schedules and details
- **Journal System:** Links activities to journal entries
- **Preferences System:** Incorporates care recipient preferences into activities

#### 3.1.6 Testing Requirements

**Functional Testing:**
- Verify plan displays correctly with all activities in proper order
- Test navigation between days works properly
- Validate activity details display correctly
- Confirm personal activity creation works

**Usability Testing:**
- Test with care recipients of varying cognitive abilities
- Verify readability at different text sizes
- Ensure touch targets are appropriately sized (minimum 44x44px)
- Confirm color contrast meets accessibility guidelines

**Integration Testing:**
- Verify calendar events appear correctly in the plan
- Test that preference changes affect future occurrences
- Confirm medication management system integrates properly
- Validate that journal entries can be created from activities

**Performance Testing:**
- Test loading time for daily plan (under 2 seconds)
- Verify smooth day-to-day navigation
- Test performance with 10+ activities per day
- Validate offline functionality for viewing today's plan

### 3.2 Express Needs System

#### 3.2.1 Purpose & Overview

The Express Needs System enables Care Recipients to clearly communicate their needs, preferences, and requests to their care circle while preserving dignity and supporting varying levels of communication ability. Unlike traditional help buttons or generic messaging, this system provides structured yet flexible ways to express specific needs.

This module acknowledges that expressing needs can be challenging due to physical limitations, cognitive challenges, emotional discomfort with asking for help, or simply not knowing how to articulate specific requirements. It offers multiple expression pathways, from simple predefined needs to detailed custom requests.

#### 3.2.2 User Stories

| As a... | I want to... | So that... | Priority |
|---------|-------------|------------|----------|
| Care Recipient | Easily request help with basic needs | I get timely assistance without struggle | 🔴 MUST |
| Care Recipient | Express preferences about how care is provided | Care feels respectful and comfortable | 🔴 MUST |
| Care Recipient | Communicate non-urgent requests | I can plan ahead for my needs | 🔴 MUST |
| Care Recipient | Indicate who I prefer to help with specific tasks | I maintain comfort and dignity | 🔴 MUST |
| Care Recipient | Express emotional needs, not just physical ones | My complete wellbeing is supported | 🟠 SHOULD |
| Care Recipient | Provide feedback about recent care experiences | Future care can be adjusted | 🟠 SHOULD |
| Care Recipient | Request changes to my care plan | I can adapt as my needs change | 🟡 COULD |
| Care Recipient | Save common or recurring requests as templates | I don't have to repeat myself | 🟡 COULD |

#### 3.2.3 UI Components & Interactions

**Primary Components:**
- **Quick Needs Panel:** Simple buttons for common needs
- **Need Request Builder:** Detailed form for specific requests
- **Helper Preference Selector:** Tools to indicate preferred helpers
- **Request Status Tracker:** Visual tracking of request status
- **Voice Input Support:** Prominent voice recording options

**Quick Needs Panel:**
```
┌─────────────────────────────────────────────┐
│ I Need...                                   │
│                                             │
│ ┌─────────┐ ┌─────────┐ ┌─────────┐        │
│ │   🥤    │ │   🚽    │ │   💊    │        │
│ │ Drink   │ │Bathroom │ │Medicine │        │
│ └─────────┘ └─────────┘ └─────────┘        │
│                                             │
│ ┌─────────┐ ┌─────────┐ ┌─────────┐        │
│ │   🍽️    │ │   🛌    │ │   🗣️    │        │
│ │  Food   │ │  Rest   │ │  Talk   │        │
│ └─────────┘ └─────────┘ └─────────┘        │
│                                             │
│ ┌───────────────────────────────────────┐   │
│ │       Something Else...               │   │
│ └───────────────────────────────────────┘   │
└─────────────────────────────────────────────┘
```

**Need Request Builder:**
```
┌─────────────────────────────────────────────┐
│ Express a Need                              │
│                                             │
│ What type of help do you need?              │
│ ○ Physical Assistance                       │
│ ○ Social Need                               │
│ ○ Emotional Support                         │
│ ● Practical Help                            │
│ ○ Medical Assistance                        │
│                                             │
│ Describe what you need:                     │
│ ┌───────────────────────────────────────┐   │
│ │ I need help organizing my bookshelf   │   │
│ │ since I can't reach the top shelf.    │   │
│ │                                       │ 🎤 │
│ └───────────────────────────────────────┘   │
│                                             │
│ When do you need this?                      │
│ ○ Right Now  ○ Today  ● When Convenient     │
│                                             │
│ Who would you prefer to help? (Optional)    │
│ ┌───────────────────────────────────────┐   │
│ │ [Sarah ✓]  [Michael]  [Jennifer]      │   │
│ └───────────────────────────────────────┘   │
│                                             │
│ Privacy:                                    │
│ ● Share with care circle  ○ Keep private    │
│                                             │
│ ┌───────────────────────────────────────┐   │
│ │             Send Request              │   │
│ └───────────────────────────────────────┘   │
└─────────────────────────────────────────────┘
```

**Request Status Tracker:**
```
┌─────────────────────────────────────────────┐
│ Your Requests                               │
│                                             │
│ ┌─────────────────────────────────────────┐ │
│ │ Bookshelf Organization                  │ │
│ │ Requested 20 minutes ago                │ │
│ │ Status: Sarah will help this afternoon  │ │
│ └─────────────────────────────────────────┘ │
│                                             │
│ ┌─────────────────────────────────────────┐ │
│ │ Water                                   │ │
│ │ Requested 5 minutes ago                 │ │
│ │ Status: Michael is bringing it now      │ │
│ └─────────────────────────────────────────┘ │
│                                             │
│ ┌───────────────────────────────────────┐   │
│ │          New Request                  │   │
│ └───────────────────────────────────────┘   │
└─────────────────────────────────────────────┘
```

**Interactions:**
- **Tap need button:** Quick request for common needs
- **Voice recording:** Speak request for natural language processing
- **Tap helper avatars:** Select preferred helpers
- **Slide urgency:** Indicate time sensitivity
- **Tap status card:** View detailed request status

**Empty States:**
- **No active requests:** "No active requests. Tap 'New Request' when you need something."
- **No response yet:** "Your request has been sent. Someone will respond soon."

#### 3.2.4 Technical Implementation

**Data Model:**
```json
{
  "need_request": {
    "id": "req123",
    "care_recipient_id": "recipient123",
    "need_type": "practical",
    "title": "Bookshelf Organization",
    "description": "I need help organizing my bookshelf since I can't reach the top shelf.",
    "urgency_level": "when_convenient",
    "preferred_helpers": [
      {
        "user_id": "user456",
        "name": "Sarah",
        "role": "caregiver"
      }
    ],
    "privacy_level": "circle",
    "created_at": "2025-04-15T10:30:00Z",
    "status": {
      "state": "acknowledged",
      "responder": {
        "user_id": "user456",
        "name": "Sarah",
        "role": "caregiver"
      },
      "response_message": "I'll help this afternoon around 3pm",
      "response_time": "2025-04-15T10:35:00Z",
      "estimated_fulfillment_time": "2025-04-15T15:00:00Z"
    },
    "fulfillment": null,
    "communication_thread": [
      {
        "author_id": "user456",
        "content": "Would you like me to organize them alphabetically?",
        "timestamp": "2025-04-15T10:40:00Z"
      },
      {
        "author_id": "recipient123",
        "content": "Yes, alphabetical would be perfect",
        "timestamp": "2025-04-15T10:42:00Z"
      }
    ],
    "care_circle_id": "circle789"
  }
}
```

**API Endpoints:**
- `POST /need-requests` - Create new need request
- `GET /need-requests` - Get list of active requests
- `GET /need-requests/:id` - Get specific request details
- `POST /need-requests/:id/messages` - Add message to request thread
- `PUT /need-requests/:id/cancel` - Cancel a request
- `GET /need-requests/templates` - Get saved request templates

**State Management:**
```javascript
// Need request state management (pseudocode)
const needRequestState = {
  // Active requests
  requests: {
    byId: {},
    allIds: [],
    loading: false,
    error: null
  },
  
  // Request creation form state
  creation: {
    step: 1,
    needType: null,
    description: '',
    urgencyLevel: 'when_convenient',
    preferredHelpers: [],
    privacyLevel: 'circle',
    isSubmitting: false,
    error: null
  },
  
  // Quick needs configuration
  quickNeeds: {
    items: [
      { id: 'drink', icon: '🥤', label: 'Drink' },
      { id: 'bathroom', icon: '🚽', label: 'Bathroom' },
      { id: 'medicine', icon: '💊', label: 'Medicine' },
      { id: 'food', icon: '🍽️', label: 'Food' },
      { id: 'rest', icon: '🛌', label: 'Rest' },
      { id: 'talk', icon: '🗣️', label: 'Talk' }
    ]
  },
  
  // Templates for common requests
  templates: {
    items: [],
    loading: false,
    error: null
  }
};

// Need request actions (pseudocode)
const needRequestActions = {
  // Load active requests
  loadRequests: () => async (dispatch) => {
    dispatch({ type: 'LOAD_REQUESTS_START' });
    try {
      const requests = await api.getNeedRequests();
      dispatch({ type: 'LOAD_REQUESTS_SUCCESS', payload: requests });
    } catch (error) {
      dispatch({ type: 'LOAD_REQUESTS_ERROR', payload: error });
    }
  },
  
  // Create quick need
  createQuickNeed: (needId) => async (dispatch) => {
    dispatch({ type: 'CREATE_QUICK_NEED_START', payload: needId });
    try {
      const quickNeedData = getQuickNeedData(needId);
      const request = await api.createNeedRequest(quickNeedData);
      dispatch({ type: 'CREATE_QUICK_NEED_SUCCESS', payload: request });
      
      // Show confirmation
      dispatch(uiActions.showNotification({
        type: 'success',
        message: `Your ${quickNeedData.title} request has been sent`
      }));
      
      return request;
    } catch (error) {
      dispatch({ type: 'CREATE_QUICK_NEED_ERROR', payload: error });
      throw error;
    }
  },
  
  // Update creation form field
  updateCreationField: (field, value) => ({
    type: 'UPDATE_CREATION_FIELD',
    payload: { field, value }
  }),
  
  // Move to next creation step
  nextCreationStep: () => ({
    type: 'NEXT_CREATION_STEP'
  }),
  
  // Move to previous creation step
  prevCreationStep: () => ({
    type: 'PREV_CREATION_STEP'
  }),
  
  // Create detailed need request
  createNeedRequest: (requestData) => async (dispatch) => {
    dispatch({ type: 'CREATE_NEED_REQUEST_START', payload: requestData });
    try {
      const request = await api.createNeedRequest(requestData);
      dispatch({ type: 'CREATE_NEED_REQUEST_SUCCESS', payload: request });
      
      // Reset form and show confirmation
      dispatch({ type: 'RESET_CREATION_FORM' });
      dispatch(uiActions.showNotification({
        type: 'success',
        message: 'Your request has been sent'
      }));
      
      return request;
    } catch (error) {
      dispatch({ type: 'CREATE_NEED_REQUEST_ERROR', payload: error });
      throw error;
    }
  },
  
  // Cancel a request
  cancelRequest: (requestId, reason) => async (dispatch) => {
    dispatch({ type: 'CANCEL_REQUEST_START', payload: requestId });
    try {
      await api.cancelNeedRequest(requestId, reason);
      dispatch({ type: 'CANCEL_REQUEST_SUCCESS', payload: requestId });
    } catch (error) {
      dispatch({ type: 'CANCEL_REQUEST_ERROR', payload: { requestId, error } });
      throw error;
    }
  },
  
  // Send message in request thread
  sendRequestMessage: (requestId, message) => async (dispatch) => {
    dispatch({ 
      type: 'SEND_REQUEST_MESSAGE_START', 
      payload: { requestId, message } 
    });
    try {
      const result = await api.sendNeedRequestMessage(requestId, message);
      dispatch({ 
        type: 'SEND_REQUEST_MESSAGE_SUCCESS', 
        payload: { requestId, message: result } 
      });
    } catch (error) {
      dispatch({ 
        type: 'SEND_REQUEST_MESSAGE_ERROR', 
        payload: { requestId, error } 
      });
      throw error;
    }
  },
  
  // Load request templates
  loadTemplates: () => async (dispatch) => {
    dispatch({ type: 'LOAD_TEMPLATES_START' });
    try {
      const templates = await api.getNeedRequestTemplates();
      dispatch({ type: 'LOAD_TEMPLATES_SUCCESS', payload: templates });
    } catch (error) {
      dispatch({ type: 'LOAD_TEMPLATES_ERROR', payload: error });
    }
  },
  
  // Save request as template
  saveAsTemplate: (requestId, templateName) => async (dispatch) => {
    dispatch({ 
      type: 'SAVE_AS_TEMPLATE_START', 
      payload: { requestId, templateName } 
    });
    try {
      const template = await api.saveNeedRequestTemplate(requestId, templateName);
      dispatch({ type: 'SAVE_AS_TEMPLATE_SUCCESS', payload: template });
    } catch (error) {
      dispatch({ 
        type: 'SAVE_AS_TEMPLATE_ERROR', 
        payload: { requestId, error } 
      });
      throw error;
    }
  }
};
```

#### 3.2.5 Integration Points

- **Task Management System:** Converts need requests into tasks for caregivers
- **Communication Hub:** Enables messaging around requests
- **Notification System:** Alerts caregivers to new requests
- **Calendar System:** Optionally schedules fulfillment time

#### 3.2.6 Testing Requirements

**Functional Testing:**
- Verify quick needs successfully generate requests
- Test detailed request creation with all fields
- Validate request status updates properly
- Confirm communication thread functions correctly

**Usability Testing:**
- Test with care recipients having different cognitive abilities
- Verify voice input functions reliably
- Ensure quick needs are easily distinguishable
- Test with various physical limitations (touch precision, etc.)

**Integration Testing:**
- Verify requests appear for appropriate caregivers
- Test that responses from caregivers display correctly
- Confirm task creation from requests works properly
- Validate notification delivery for new requests

**Privacy Testing:**
- Verify privacy settings properly control visibility
- Test that private requests remain confidential
- Confirm privacy level changes take effect immediately

### 3.3 Privacy Control Center

#### 3.3.1 Purpose & Overview

The Privacy Control Center gives Care Recipients granular control over what personal information is shared, with whom, and under what circumstances. Unlike binary privacy settings, this system allows nuanced control that respects the complexity of care relationships while maintaining personal dignity and appropriate boundaries.

This module recognizes that privacy needs vary widely based on the type of information, specific care circle members, and changing circumstances. It provides intuitive tools for managing these complex privacy decisions without technical jargon or overwhelming complexity.

#### 3.3.2 User Stories

| As a... | I want to... | So that... | Priority |
|---------|-------------|------------|----------|
| Care Recipient | Control who sees my health information | I maintain appropriate privacy | 🔴 MUST |
| Care Recipient | Set different privacy levels for different data types | I can share what's necessary without oversharing | 🔴 MUST |
| Care Recipient | Override default settings for specific situations | I have control in changing circumstances | 🔴 MUST |
| Care Recipient | See who has access to what information | I understand my current privacy state | 🔴 MUST |
| Care Recipient | Set time-limited access to information | I can enable temporary access when needed | 🟠 SHOULD |
| Care Recipient | Quickly adjust privacy during sensitive situations | I maintain dignity during vulnerable moments | 🟠 SHOULD |
| Care Recipient | Create privacy presets for different scenarios | I can easily apply appropriate settings | 🟡 COULD |
| Care Recipient | Automatically revoke access when no longer needed | My privacy is protected by default | 🟡 COULD |

#### 3.3.3 UI Components & Interactions

**Primary Components:**
- **Privacy Dashboard:** Overview of current privacy settings
- **Person-Based Controls:** Settings for individual care circle members
- **Category-Based Controls:** Settings for different types of information
- **Access Log:** Record of who has viewed sensitive information
- **Quick Privacy Actions:** Tools for temporary privacy adjustments

**Privacy Dashboard:**
```
┌─────────────────────────────────────────────┐
│ Your Privacy Settings                       │
│                                             │
│ Privacy Level: Standard                     │
│ [○───●───○]                                 │
│ More Private    Standard    More Open       │
│                                             │
│ What Others Can See:                        │
│ ✓ Basic Schedule                            │
│ ✓ General Health Status                     │
│ ✓ Medication Schedule                       │
│ ✗ Medication Details                        │
│ ✗ Personal Care Details                     │
│ ✓ Journal Entries (shared only)             │
│                                             │
│ ┌───────────────────────────────────────┐   │
│ │          Adjust Settings              │   │
│ └───────────────────────────────────────┘   │
└─────────────────────────────────────────────┘
```

**Person-Based Controls:**
```
┌─────────────────────────────────────────────┐
│ Privacy By Person                           │
│                                             │
│ ┌─────────────────────────────────────────┐ │
│ │ [Avatar] Sarah (Caregiver)              │ │
│ │ Full access to health information       │ │
│ │ Can see medication details              │ │
│ │ Can see personal care information       │ │
│ └─────────────────────────────────────────┘ │
│                                             │
│ ┌─────────────────────────────────────────┐ │
│ │ [Avatar] Michael (Supporter)            │ │
│ │ Limited access to health information    │ │
│ │ Cannot see medication details           │ │
│ │ Cannot see personal care information    │ │
│ └─────────────────────────────────────────┘ │
│                                             │
│ ┌─────────────────────────────────────────┐ │
│ │ [Avatar] Jennifer (Supporter)           │ │
│ │ Standard privacy settings               │ │
│ │ Custom exceptions: Can see journal      │ │
│ └─────────────────────────────────────────┘ │
└─────────────────────────────────────────────┘
```

**Category-Based Controls:**
```
┌─────────────────────────────────────────────┐
│ Health Information Privacy                  │
│                                             │
│ Who can see your health information?        │
│                                             │
│ ○ Everyone in care circle                   │
│ ● Only caregivers and medical supporters    │
│ ○ Only primary caregiver                    │
│ ○ Custom selection:                         │
│   □ Sarah    □ Michael    □ Jennifer        │
│                                             │
│ What level of detail can they see?          │
│                                             │
│ ○ General status only (feeling better/worse)│
│ ● Basic metrics (temperature, pulse, etc.)  │
│ ○ Detailed medical information              │
│ ○ Custom by person:                         │
│   Sarah: [Detailed▼]  Michael: [Basic▼]     │
│                                             │
│ ┌───────────────────────────────────────┐   │
│ │          Save Settings                │   │
│ └───────────────────────────────────────┘   │
└─────────────────────────────────────────────┘
```

**Access Log:**
```
┌─────────────────────────────────────────────┐
│ Information Access Log                      │
│                                             │
│ Today                                       │
│ ┌─────────────────────────────────────────┐ │
│ │ Sarah viewed your medication schedule   │ │
│ │ 10:15 AM                                │ │
│ └─────────────────────────────────────────┘ │
│                                             │
│ ┌─────────────────────────────────────────┐ │
│ │ Dr. Johnson viewed your health records  │ │
│ │ 2:30 PM • Medical appointment           │ │
│ └─────────────────────────────────────────┘ │
│                                             │
│ Yesterday                                   │
│ ┌─────────────────────────────────────────┐ │
│ │ Michael viewed your schedule            │ │
│ │ 7:45 PM                                 │ │
│ └─────────────────────────────────────────┘ │
└─────────────────────────────────────────────┘
```

**Interactions:**
- **Slide privacy level:** Adjust overall privacy
- **Tap category card:** Open detailed settings for that category
- **Tap person card:** Adjust settings for specific person
- **Toggle switches:** Enable/disable specific permissions
- **View access log:** Track who has accessed information

**Empty States:**
- **No access log entries:** "No one has accessed your private information recently."
- **No custom settings:** "You're using standard privacy settings. Tap to customize."

#### 3.3.4 Technical Implementation

**Data Model:**
```json
{
  "privacy_settings": {
    "care_recipient_id": "recipient123",
    "global_level": "standard",
    "category_settings": {
      "health_information": {
        "access_level": "caregivers_and_medical",
        "detail_level": "basic_metrics",
        "custom_access": []
      },
      "medication": {
        "access_level": "caregivers_only",
        "detail_level": "schedule_only",
        "custom_access": []
      },
      "personal_care": {
        "access_level": "caregivers_only",
        "detail_level": "general",
        "custom_access": []
      },
      "schedule": {
        "access_level": "circle",
        "detail_level": "basic",
        "custom_access": []
      },
      "journal": {
        "access_level": "shared_only",
        "detail_level": "full",
        "custom_access": [
          {
            "user_id": "user789",
            "access_level": "full",
            "detail_level": "full"
          }
        ]
      }
    },
    "person_overrides": [
      {
        "user_id": "user456",
        "name": "Sarah",
        "role": "caregiver",
        "category_overrides": {
          "health_information": {
            "access_level": "full",
            "detail_level": "detailed"
          },
          "medication": {
            "access_level": "full",
            "detail_level": "full"
          },
          "personal_care": {
            "access_level": "full",
            "detail_level": "detailed"
          }
        }
      },
      {
        "user_id": "user789",
        "name": "Michael",
        "role": "supporter",
        "category_overrides": {
          "health_information": {
            "access_level": "limited",
            "detail_level": "general"
          },
          "medication": {
            "access_level": "none",
            "detail_level": "none"
          },
          "personal_care": {
            "access_level": "none",
            "detail_level": "none"
          }
        }
      }
    ],
    "temporary_access": [
      {
        "user_id": "user101",
        "name": "Dr. Johnson",
        "role": "healthcare_provider",
        "categories": ["health_information", "medication"],
        "detail_level": "full",
        "expires_at": "2025-04-22T23:59:59Z",
        "granted_at": "2025-04-15T14:30:00Z",
        "reason": "Annual check-up follow up"
      }
    ],
    "access_log": [
      {
        "user_id": "user456",
        "name": "Sarah",
        "role": "caregiver",
        "accessed_category": "medication",
        "access_time": "2025-04-15T10:15:00Z",
        "context": "routine check"
      },
      {
        "user_id": "user101",
        "name": "Dr. Johnson",
        "role": "healthcare_provider",
        "accessed_category": "health_information",
        "access_time": "2025-04-15T14:30:00Z",
        "context": "medical appointment"
      },
      {
        "user_id": "user789",
        "name": "Michael",
        "role": "supporter",
        "accessed_category": "schedule",
        "access_time": "2025-04-14T19:45:00Z",
        "context": "coordination"
      }
    ],
    "updated_at": "2025-04-15T16:30:00Z"
  }
}
```

**API Endpoints:**
- `GET /privacy-settings` - Get current privacy settings
- `PUT /privacy-settings/global` - Update global privacy level
- `PUT /privacy-settings/categories/:category` - Update category settings
- `PUT /privacy-settings/person/:userId` - Update person-specific settings
- `POST /privacy-settings/temporary-access` - Grant temporary access
- `DELETE /privacy-settings/temporary-access/:id` - Revoke temporary access
- `GET /privacy-settings/access-log` - Get access history

**Permission Evaluation:**
```javascript
// Permission evaluation (pseudocode)
function evaluatePermission(userId, category, detailLevel) {
  // Get user
  const user = getUserById(userId);
  
  // Get privacy settings
  const privacySettings = getPrivacySettings();
  
  // Check for person override
  const personOverride = privacySettings.person_overrides.find(
    p => p.user_id === userId
  );
  
  if (personOverride && personOverride.category_overrides[category]) {
    const override = personOverride.category_overrides[category];
    
    // Check if no access
    if (override.access_level === 'none') {
      return false;
    }
    
    // Check detail level
    if (getDetailLevelValue(override.detail_level) < getDetailLevelValue(detailLevel)) {
      return false;
    }
    
    return true;
  }
  
  // Check for temporary access
  const tempAccess = privacySettings.temporary_access.find(
    t => t.user_id === userId && 
         t.categories.includes(category) &&
         new Date(t.expires_at) > new Date()
  );
  
  if (tempAccess) {
    // Check detail level
    if (getDetailLevelValue(tempAccess.detail_level) < getDetailLevelValue(detailLevel)) {
      return false;
    }
    
    return true;
  }
  
  // Check category settings
  const categorySetting = privacySettings.category_settings[category];
  
  // Check custom access
  const customAccess = categorySetting.custom_access.find(
    c => c.user_id === userId
  );
  
  if (customAccess) {
    // Check detail level
    if (getDetailLevelValue(customAccess.detail_level) < getDetailLevelValue(detailLevel)) {
      return false;
    }
    
    return true;
  }
  
  // Check against user role and category access level
  switch (categorySetting.access_level) {
    case 'none':
      return false;
    
    case 'caregivers_only':
      if (user.role !== 'caregiver' && user.role !== 'healthcare_provider') {
        return false;
      }
      break;
    
    case 'caregivers_and_medical':
      if (user.role !== 'caregiver' && 
          user.role !== 'healthcare_provider' && 
          !user.has_medical_support) {
        return false;
      }
      break;
    
    case 'shared_only':
      // For journal entries - check if explicitly shared
      if (category === 'journal' && !isJournalEntryShared(entryId, userId)) {
        return false;
      }
      break;
    
    case 'circle':
      // Everyone in care circle has access
      if (!isInCareCircle(userId)) {
        return false;
      }
      break;
  }
  
  // Check detail level
  if (getDetailLevelValue(categorySetting.detail_level) < getDetailLevelValue(detailLevel)) {
    return false;
  }
  
  return true;
}

// Log access attempt
function logAccessAttempt(userId, category, detailLevel, isPermitted, context) {
  const user = getUserById(userId);
  
  const logEntry = {
    user_id: userId,
    name: `${user.first_name} ${user.last_name}`,
    role: user.role,
    accessed_category: category,
    access_time: new Date(),
    context: context || 'general access',
    permitted: isPermitted
  };
  
  // Log the access attempt
  saveAccessLogEntry(logEntry);
  
  return logEntry;
}
```

#### 3.3.5 Integration Points

- **Permission Framework:** Enforces privacy settings across all data access
- **User Management:** Retrieves user roles and relationships
- **Communication Hub:** Applies privacy filters to updates and messages
- **Journal System:** Controls sharing of journal entries
- **Health Data System:** Manages access to health information

#### 3.3.6 Testing Requirements

**Functional Testing:**
- Verify privacy settings save and apply correctly
- Test person-specific overrides function properly
- Validate category settings control appropriate data
- Confirm temporary access works and expires correctly

**Privacy Testing:**
- Verify information access respects all privacy settings
- Test privacy boundary enforcement across all categories
- Confirm access logging accurately records viewing
- Validate that privacy changes take immediate effect

**Integration Testing:**
- Test privacy filters in communication hub
- Verify journal entry sharing respects privacy settings
- Confirm health data access follows privacy rules
- Validate that schedule visibility is properly controlled

**Security Testing:**
- Test permission enforcement on all API endpoints
- Verify access controls cannot be bypassed
- Validate access logging for security monitoring
- Test for unauthorized access prevention

### 3.4 Journal & Reflection Tools

#### 3.4.1 Purpose & Overview

The Journal & Reflection Tools provide Care Recipients with a private space for personal expression, emotional processing, and self-reflection. Unlike clinical observation tools, this system creates a truly personal space that centers the Care Recipient's own narrative, supporting emotional wellbeing alongside physical care.

These tools acknowledge the profound emotional and psychological dimensions of receiving care, offering healthy outlets for processing these experiences while maintaining privacy and control. They balance the practical benefits of journaling for health monitoring with the deeply personal nature of self-expression.

#### 3.4.2 User Stories

| As a... | I want to... | So that... | Priority |
|---------|-------------|------------|----------|
| Care Recipient | Privately record my thoughts and feelings | I can process my care experience emotionally | 🔴 MUST |
| Care Recipient | Document personal observations about my health | I can track patterns and share insights when ready | 🔴 MUST |
| Care Recipient | Control who can see specific journal entries | I maintain privacy while sharing when appropriate | 🔴 MUST |
| Care Recipient | Express myself in multiple formats (text, audio, images) | I can communicate in ways that work for me | 🔴 MUST |
| Care Recipient | Receive gentle reflection prompts | I'm supported in processing difficult experiences | 🟠 SHOULD |
| Care Recipient | See patterns in my mood and wellbeing over time | I gain personal insights about my health journey | 🟠 SHOULD |
| Care Recipient | Create themed journals for different purposes | I can organize my thoughts and observations | 🟡 COULD |
| Care Recipient | Share selected insights with healthcare providers | My personal observations inform my medical care | 🟡 COULD |

#### 3.4.3 UI Components & Interactions

**Primary Components:**
- **Journal Dashboard:** Overview of journal entries and mood tracking
- **Entry Creator:** Multi-format tools for creating entries
- **Reflection Prompts:** Optional guided reflection topics
- **Mood Tracker:** Simple emotional state recording
- **Entry Browsing:** Tools for reviewing past entries

**Journal Dashboard:**
```
┌─────────────────────────────────────────────┐
│ Your Journal                                │
│                                             │
│ How are you feeling today?                  │
│ ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐     │
│ │  😊  │ │  😐  │ │  😔  │ │  😤  │ │  😴  │     │
│ │Great │ │ Okay │ │ Down │ │Upset │ │Tired │     │
│ └─────┘ └─────┘ └─────┘ └─────┘ └─────┘     │
│                                             │
│ Recent Entries                              │
│ ┌─────────────────────────────────────────┐ │
│ │ Today, 10:30 AM                         │ │
│ │ Feeling better after a good night's...  │ │
│ └─────────────────────────────────────────┘ │
│                                             │
│ ┌─────────────────────────────────────────┐ │
│ │ Yesterday, 8:15 PM                      │ │
│ │ Frustrated with the medication side...  │ │
│ └─────────────────────────────────────────┘ │
│                                             │
│ ┌───────────────────────────────────────┐   │
│ │          New Journal Entry            │   │
│ └───────────────────────────────────────┘   │
└─────────────────────────────────────────────┘
```

**Entry Creator:**
```
┌─────────────────────────────────────────────┐
│ New Journal Entry                           │
│                                             │
│ Today, April 15 • 3:45 PM                   │
│                                             │
│ What's on your mind?                        │
│ ┌───────────────────────────────────────┐   │
│ │ I had a good visit with Sarah today.  │   │
│ │ We talked about my garden plans for   │   │
│ │ this summer. It felt nice to think    │   │
│ │ about future projects.                │   │
│ │                                       │ 🎤 │
│ └───────────────────────────────────────┘   │
│                                             │
│ Add to entry:                               │
│ ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐             │
│ │ 📷  │ │ 🎙️  │ │ 📑  │ │ 📝  │             │
│ │Photo│ │Voice│ │Form │ │Draw │             │
│ └─────┘ └─────┘ └─────┘ └─────┘             │
│                                             │
│ Who can see this entry?                     │
│ ● Just me                                   │
│ ○ Share with: [Select people...]            │
│                                             │
│ ┌───────────────────────────────────────┐   │
│ │              Save Entry               │   │
│ └───────────────────────────────────────┘   │
└─────────────────────────────────────────────┘
```

**Reflection Prompts:**
```
┌─────────────────────────────────────────────┐
│ Reflection Prompts                          │
│                                             │
│ Need some inspiration? Try one of these:    │
│                                             │
│ ┌─────────────────────────────────────────┐ │
│ │ What made you smile today?              │ │
│ └─────────────────────────────────────────┘ │
│                                             │
│ ┌─────────────────────────────────────────┐ │
│ │ How did your body feel when you woke up?│ │
│ └─────────────────────────────────────────┘ │
│                                             │
│ ┌─────────────────────────────────────────┐ │
│ │ What are you looking forward to?        │ │
│ └─────────────────────────────────────────┘ │
│                                             │
│ ┌─────────────────────────────────────────┐ │
│ │ What would you like help with tomorrow? │ │
│ └─────────────────────────────────────────┘ │
└─────────────────────────────────────────────┘
```

**Mood Tracking View:**
```
┌─────────────────────────────────────────────┐
│ Your Mood Patterns                          │
│                                             │
│ Last 7 Days                                 │
│ ┌─────────────────────────────────────────┐ │
│ │                                         │ │
│ │   😊                      😊             │ │
│ │        😐      😐                       │ │
│ │               😔                         │ │
│ │                                         │ │
│ │ Wed  Thu  Fri  Sat  Sun  Mon  Tue      │ │
│ └─────────────────────────────────────────┘ │
│                                             │
│ Insights:                                   │
│ • You tend to feel better in the mornings   │
│ • Your mood has been improving this week    │
│ • Socializing seems to boost your mood      │
│                                             │
│ ┌───────────────────────────────────────┐   │
│ │      Track Today's Mood               │   │
│ └───────────────────────────────────────┘   │
└─────────────────────────────────────────────┘
```

**Interactions:**
- **Tap mood icon:** Record current emotional state
- **Tap entry card:** View full journal entry
- **Voice recording:** Dictate journal entries
- **Swipe entries:** Navigate through journal history
- **Tap prompt:** Use suggestion for reflection

**Empty States:**
- **No journal entries:** "Your journal is private and waiting for your thoughts. Tap 'New Entry' to begin."
- **No mood data:** "Start tracking your mood to see patterns over time."

#### 3.4.4 Technical Implementation

**Data Model:**
```json
{
  "journal_entry": {
    "id": "entry123",
    "care_recipient_id": "recipient123",
    "title": null,
    "content": "I had a good visit with Sarah today. We talked about my garden plans for this summer. It felt nice to think about future projects.",
    "created_at": "2025-04-15T15:45:00Z",
    "updated_at": "2025-04-15T15:45:00Z",
    "mood": "great",
    "tags": ["social", "future_plans"],
    "media": [],
    "form_data": null,
    "privacy_level": "private",
    "shared_with": [],
    "associated_care_events": [
      {
        "event_id": "event456",
        "type": "visit",
        "title": "Visit with Sarah"
      }
    ],
    "prompt_used": null
  },
  
  "mood_entry": {
    "id": "mood123",
    "care_recipient_id": "recipient123",
    "mood": "great",
    "energy_level": "moderate",
    "notes": "Feeling positive after social visit",
    "timestamp": "2025-04-15T15:30:00Z",
    "factors": ["social", "rest"],
    "privacy_level": "private"
  },
  
  "journal_stats": {
    "care_recipient_id": "recipient123",
    "entry_count": 47,
    "first_entry_date": "2025-01-10T09:15:00Z",
    "last_entry_date": "2025-04-15T15:45:00Z",
    "mood_distribution": {
      "great": 12,
      "good": 20,
      "okay": 8,
      "down": 5,
      "upset": 2
    },
    "entry_frequency": {
      "daily": 0.8,
      "weekly": 5.2
    },
    "common_tags": [
      {"tag": "health", "count": 15},
      {"tag": "social", "count": 12},
      {"tag": "activities", "count": 10}
    ],
    "shared_entry_count": 8
  }
}
```

**API Endpoints:**
- `POST /journal-entries` - Create new journal entry
- `GET /journal-entries` - Get list of journal entries
- `GET /journal-entries/:id` - Get specific entry
- `PUT /journal-entries/:id` - Update an entry
- `POST /journal-entries/:id/share` - Share entry with specific people
- `POST /mood-entries` - Record mood
- `GET /mood-entries` - Get mood history
- `GET /journal-stats` - Get journaling statistics
- `GET /reflection-prompts` - Get suggested reflection prompts

**State Management:**
```javascript
// Journal state management (pseudocode)
const journalState = {
  // Journal entries
  entries: {
    byId: {},
    allIds: [],
    loading: false,
    error: null,
    pagination: {
      currentPage: 1,
      totalPages: 1,
      pageSize: 10
    }
  },
  
  // Mood entries
  moods: {
    byDate: {},
    allDates: [],
    loading: false,
    error: null
  },
  
  // Journal statistics
  stats: {
    data: null,
    loading: false,
    error: null
  },
  
  // Entry creation/editing
  editor: {
    isActive: false,
    currentEntry: null,
    content: '',
    mood: null,
    privacyLevel: 'private',
    sharedWith: [],
    media: [],
    isSubmitting: false,
    error: null
  },
  
  // Reflection prompts
  prompts: {
    items: [],
    loading: false,
    error: null
  }
};

// Journal actions (pseudocode)
const journalActions = {
  // Load journal entries
  loadEntries: (page = 1) => async (dispatch) => {
    dispatch({ type: 'LOAD_ENTRIES_START', payload: page });
    try {
      const response = await api.getJournalEntries(page);
      dispatch({ 
        type: 'LOAD_ENTRIES_SUCCESS', 
        payload: {
          entries: response.entries,
          pagination: response.pagination
        } 
      });
    } catch (error) {
      dispatch({ type: 'LOAD_ENTRIES_ERROR', payload: error });
    }
  },
  
  // Load single entry
  loadEntry: (entryId) => async (dispatch) => {
    dispatch({ type: 'LOAD_ENTRY_START', payload: entryId });
    try {
      const entry = await api.getJournalEntry(entryId);
      dispatch({ type: 'LOAD_ENTRY_SUCCESS', payload: entry });
    } catch (error) {
      dispatch({ type: 'LOAD_ENTRY_ERROR', payload: { entryId, error } });
    }
  },
  
  // Create journal entry
  createEntry: (entryData) => async (dispatch) => {
    dispatch({ type: 'CREATE_ENTRY_START', payload: entryData });
    try {
      const entry = await api.createJournalEntry(entryData);
      dispatch({ type: 'CREATE_ENTRY_SUCCESS', payload: entry });
      
      // Reset editor
      dispatch({ type: 'RESET_EDITOR' });
      
      // Show confirmation
      dispatch(uiActions.showNotification({
        type: 'success',
        message: 'Journal entry saved'
      }));
      
      return entry;
    } catch (error) {
      dispatch({ type: 'CREATE_ENTRY_ERROR', payload: error });
      throw error;
    }
  },
  
  // Update journal entry
  updateEntry: (entryId, changes) => async (dispatch) => {
    dispatch({ 
      type: 'UPDATE_ENTRY_START', 
      payload: { entryId, changes } 
    });
    try {
      const entry = await api.updateJournalEntry(entryId, changes);
      dispatch({ type: 'UPDATE_ENTRY_SUCCESS', payload: entry });
      
      // Reset editor
      dispatch({ type: 'RESET_EDITOR' });
      
      // Show confirmation
      dispatch(uiActions.showNotification({
        type: 'success',
        message: 'Journal entry updated'
      }));
      
      return entry;
    } catch (error) {
      dispatch({ 
        type: 'UPDATE_ENTRY_ERROR', 
        payload: { entryId, error } 
      });
      throw error;
    }
  },
  
  // Share journal entry
  shareEntry: (entryId, userIds) => async (dispatch) => {
    dispatch({ 
      type: 'SHARE_ENTRY_START', 
      payload: { entryId, userIds } 
    });
    try {
      const result = await api.shareJournalEntry(entryId, userIds);
      dispatch({ 
        type: 'SHARE_ENTRY_SUCCESS', 
        payload: { entryId, shared_with: result.shared_with } 
      });
      
      // Show confirmation
      dispatch(uiActions.showNotification({
        type: 'success',
        message: `Entry shared with ${userIds.length} people`
      }));
      
      return result;
    } catch (error) {
      dispatch({ 
        type: 'SHARE_ENTRY_ERROR', 
        payload: { entryId, error } 
      });
      throw error;
    }
  },
  
  // Start entry editing
  startEditing: (entry = null) => ({
    type: 'START_EDITING',
    payload: entry
  }),
  
  // Cancel editing
  cancelEditing: () => ({
    type: 'CANCEL_EDITING'
  }),
  
  // Update editor field
  updateEditorField: (field, value) => ({
    type: 'UPDATE_EDITOR_FIELD',
    payload: { field, value }
  }),
  
  // Record mood
  recordMood: (moodData) => async (dispatch) => {
    dispatch({ type: 'RECORD_MOOD_START', payload: moodData });
    try {
      const mood = await api.recordMood(moodData);
      dispatch({ type: 'RECORD_MOOD_SUCCESS', payload: mood });
      
      // Show confirmation
      dispatch(uiActions.showNotification({
        type: 'success',
        message: 'Mood recorded'
      }));
      
      return mood;
    } catch (error) {
      dispatch({ type: 'RECORD_MOOD_ERROR', payload: error });
      throw error;
    }
  },
  
  // Load mood history
  loadMoodHistory: (timespan = 'week') => async (dispatch) => {
    dispatch({ type: 'LOAD_MOOD_HISTORY_START', payload: timespan });
    try {
      const moods = await api.getMoodHistory(timespan);
      dispatch({ type: 'LOAD_MOOD_HISTORY_SUCCESS', payload: moods });
    } catch (error) {
      dispatch({ type: 'LOAD_MOOD_HISTORY_ERROR', payload: error });
    }
  },
  
  // Load journal statistics
  loadJournalStats: () => async (dispatch) => {
    dispatch({ type: 'LOAD_JOURNAL_STATS_START' });
    try {
      const stats = await api.getJournalStats();
      dispatch({ type: 'LOAD_JOURNAL_STATS_SUCCESS', payload: stats });
    } catch (error) {
      dispatch({ type: 'LOAD_JOURNAL_STATS_ERROR', payload: error });
    }
  },
  
  // Load reflection prompts
  loadReflectionPrompts: () => async (dispatch) => {
    dispatch({ type: 'LOAD_REFLECTION_PROMPTS_START' });
    try {
      const prompts = await api.getReflectionPrompts();
      dispatch({ type: 'LOAD_REFLECTION_PROMPTS_SUCCESS', payload: prompts });
    } catch (error) {
      dispatch({ type: 'LOAD_REFLECTION_PROMPTS_ERROR', payload: error });
    }
  }
};
```

#### 3.4.5 Integration Points

- **Privacy Control Center:** Manages sharing permissions for entries
- **Care Plan System:** Correlates journal entries with care events
- **Health Data System:** Optionally links mood tracking with health metrics
- **Dashboard System:** Provides journal summaries for personal dashboard

#### 3.4.6 Testing Requirements

**Functional Testing:**
- Verify journal entries save correctly with all content types
- Test mood tracking recording and visualization
- Validate sharing controls for entries
- Confirm reflection prompts display appropriately

**Usability Testing:**
- Test with care recipients of varying cognitive abilities
- Verify voice input functions reliably for journal entries
- Ensure mood tracking is simple and intuitive
- Test with various physical limitations

**Privacy Testing:**
- Verify entry privacy settings function correctly
- Test that shared entries only appear to designated people
- Confirm private entries remain completely private
- Validate that sharing changes take immediate effect

**Performance Testing:**
- Test journal with 100+ entries for loading performance
- Verify mood visualization rendering with extended history
- Test media attachment handling efficiency
- Validate offline capability for entry creation

## 4. User Experience Integration

### 4.1 Role-Based Adaptations

The Care Recipient Experience implements role-based adaptations across all components:

| Component | Adaptation Approach | Implementation |
|-----------|---------------------|----------------|
| Navigation | Simplified with large touch targets | Focus on Plan, Needs, Privacy, Journal |
| Information Display | Prioritized readability and clarity | Larger text, high contrast, straightforward language |
| Input Methods | Multiple pathways for expression | Touch, voice, media inputs for varying abilities |
| Settings | Fine-grained control with simple defaults | Layered approach with simple presets and detailed options |
| Notifications | Non-intrusive, dignified alerts | Gentle reminders that respect agency |

### 4.2 Emotional Intelligence Implementation

The Care Recipient Experience embodies the Emotional Intelligence Constitution through:

| Principle | Implementation | Examples |
|-----------|----------------|----------|
| Care Before Management | Focus on wellbeing over task completion | "How are you feeling?" preceding task reminders |
| Invisible Understanding | Contextual adaptations that anticipate needs | Medication reminders that incorporate preferences |
| Dignity Through Agency | Control and choice in all interactions | "Would you like..." instead of "You need to..." |
| Whole-Person Recognition | Support for emotional, social, and practical needs | Journal tools that address emotional wellbeing |

**Emotional Intelligence in Communication:**
- **Tone:** Respectful, never condescending or infantilizing
- **Request Framing:** "Would you like to..." rather than "You should..."
- **Privacy Language:** "Your information, your control" messaging
- **Agency Focus:** Clear ownership of decisions and preferences

### 4.3 Permission Framework Implementation

The Care Recipient Experience implements the Permission Framework with these key aspects:

**Core Permissions:**
- Full control over personal information
- Complete access to own care plan and journal
- Ability to set privacy levels for all data types
- Authority to grant and revoke access

**Permission Controls:**
- Category-based privacy settings
- Person-specific overrides
- Temporary access grants
- Default privacy presets

**Implementation Details:**
- All data requests filter through privacy settings
- User interface adapts to show privacy status
- Access logging for transparency
- Clear indication of what is shared

### 4.4 Navigation & Information Architecture

The Care Recipient Experience follows the platform's Information Architecture with role-specific adaptations:

**Primary Navigation:**
- **Today Tab:** Daily Plan View
- **Help Tab:** Express Needs System
- **Private Tab:** Journal & Reflection Tools
- **Settings Tab:** Privacy Control Center

**Secondary Navigation:**
- Today Tab sections: Timeline, Upcoming, Completed
- Help Tab sections: Quick Needs, Detailed Requests, Request Status
- Private Tab sections: Journal, Mood Tracking, Reflections
- Settings Tab sections: Privacy Controls, Access Log, Preferences

**Information Hierarchy:**
1. **Primary Focus:** Today's schedule and immediate needs
2. **Secondary Focus:** Needs expression and journal
3. **Tertiary Focus:** Privacy controls and settings

### 4.5 Notification Strategy

The Care Recipient Experience implements a respectful notification strategy:

**Notification Types:**
- **Plan Reminders:** Gentle reminders about scheduled activities
- **Request Updates:** Status changes for expressed needs
- **Privacy Alerts:** Information about who accessed data
- **Journal Prompts:** Optional reflection suggestions

**Delivery Rules:**
- Respect do-not-disturb preferences
- Allow complete notification customization
- Default to minimal essential notifications
- Ensure notifications preserve dignity

## 5. Technical Integration

### 5.1 Component Integration

The Care Recipient Experience integrates with these core platform components:

| Component | Integration Approach | Data Flow |
|-----------|----------------------|-----------|
| Authentication System | Secure login with accessibility | Login → role identification → experience adaptation |
| Calendar System | Two-way synchronization | Calendar → daily plan → activity completion → calendar update |
| Task Management System | Privacy-filtered view | Tasks → filtered plan view → completion feedback → task update |
| Communication Hub | Selective participation | Updates → privacy filter → visible updates |
| Permission System | Centralized control | Privacy settings → permission framework → data access control |

### 5.2 State Management Implementation

State management for the Care Recipient Experience follows these patterns:

**Core State Slices:**
- Plan state (daily activities and schedule)
- Needs state (requests and status)
- Privacy state (settings and access log)
- Journal state (entries and mood tracking)

**State Transitions:**
```
Need → Request → Status Updates → Resolution → Journal (optional)
```

**Persistence Strategy:**
- Plan: Local storage with sync
- Needs: Server-side with local cache
- Privacy: Server-side with secure storage
- Journal: Local with encrypted sync

### 5.3 Performance Considerations

**Key Performance Requirements:**
- Plan view initial load under 2 seconds
- Need request submission under 1 second
- Journal entry saving under 0.5 seconds
- Privacy setting changes applied immediately

**Optimization Strategies:**
- Prefetch daily plan during app startup
- Store journal entries locally before sync
- Cache privacy settings for offline access
- Background synchronization of non-critical data

### 5.4 Offline Capabilities

The Care Recipient Experience implements these offline capabilities:

| Feature | Offline Support | Implementation |
|---------|-----------------|----------------|
| Daily Plan | Full offline viewing | Store today's plan locally |
| Needs Expression | Queue for sync | Store requests locally until connection restored |
| Journal | Full offline functionality | Local storage with background sync |
| Privacy Settings | View-only offline | Apply cached settings, require connection for changes |

**Sync Resolution Strategy:**
- Plan changes sync with conflict resolution
- Need requests upload when connection restored
- Journal entries sync with merge resolution
- Privacy settings require online confirmation

## 6. Implementation Guidance

### 6.1 Development Approach

The recommended development sequence for the Care Recipient Experience is:

1. **Core Infrastructure:**
   - Role context implementation
   - Privacy framework integration
   - Accessibility foundation
   - Navigation structure

2. **Critical Path Features:**
   - Daily plan view and navigation
   - Basic needs expression
   - Essential privacy controls
   - Simple journal entry creation

3. **Supporting Features:**
   - Detailed plan integration
   - Advanced needs expression
   - Fine-grained privacy settings
   - Full journal and mood tracking

4. **Enhancement Features:**
   - Plan customization
   - Need request templates
   - Privacy presets and temporary access
   - Journal insights and reflection prompts

### 6.2 Quality Assurance Approach

**Testing Priorities:**
1. Accessibility for diverse abilities
2. Privacy setting enforcement
3. Dignity preservation in all interactions
4. Performance across device capabilities

**Test Case Focus Areas:**
- Various cognitive and physical ability scenarios
- Different privacy preference combinations
- Cross-component interaction flows
- Long-term usage patterns with extensive data

### 6.3 Success Metrics

The Care Recipient Experience should be measured against these key metrics:

**Engagement Metrics:**
- Daily plan view rate (how often care recipients check their plan)
- Need expression frequency and resolution time
- Journal usage rate and entry frequency
- Privacy setting adjustment activity

**Experience Metrics:**
- Perceived control rating (survey-based)
- Dignity preservation score (qualitative assessment)
- Task awareness accuracy (knowledge of daily plan)
- Overall satisfaction rating

**Target Goals:**
- 85% of care recipients view plan daily
- Average need resolution time under 30 minutes
- 70% journal at least weekly
- 90% report high satisfaction with privacy controls
- 95% report feeling respected and in control

## 7. Conclusion

The Care Recipient Experience is designed to preserve dignity, agency, and voice through an accessible, privacy-focused interface that empowers rather than diminishes. By putting the care recipient at the center of the experience as an active participant rather than a passive subject, this design honors the full personhood of those receiving care.

Key success factors include:
- Clarity without condescension
- Control without complexity
- Privacy without isolation
- Expression without burden
- Structure without rigidity

The implementation strategy prioritizes accessibility, dignity, and agency while providing practical tools that enhance care coordination and personal wellbeing, creating a platform where care recipients remain the authors of their own care narrative.

## 8. Appendices

### 8.1 User Personas

#### 8.1.1 Eleanor (Primary Care Recipient)
- **Age:** 78
- **Condition:** Recovering from hip replacement, mild cognitive impairment
- **Tech Comfort:** Moderate, uses tablet daily
- **Primary Concern:** Maintaining independence while accepting necessary help
- **Key Needs:** Clear daily structure, easy way to ask for help, privacy boundaries
- **Support Circle:** Daughter (primary caregiver), son (organizer), neighbor (supporter)

#### 8.1.2 Michael (Younger Care Recipient)
- **Age:** 42
- **Condition:** Multiple sclerosis, variable mobility
- **Tech Comfort:** High, tech-savvy
- **Primary Concern:** Balancing career and family roles with receiving care
- **Key Needs:** Flexible scheduling, discreet assistance requests, health tracking
- **Support Circle:** Spouse (caregiver), sister (organizer), colleagues (supporters)

#### 8.1.3 Thomas (Limited Mobility Care Recipient)
- **Age:** 65
- **Condition:** Parkinson's disease, limited fine motor control
- **Tech Comfort:** Moderate, prefers voice interfaces
- **Primary Concern:** Communication barriers due to physical limitations
- **Key Needs:** Voice-first interfaces, simple expression tools, clear feedback
- **Support Circle:** Wife (caregiver), daughter (organizer), friends (supporters)

### 8.2 Accessibility Considerations Details

| Limitation | Accommodation | Implementation |
|------------|---------------|----------------|
| Vision Impairment | Larger text, high contrast | Dynamic text sizing, 4.5:1 minimum contrast ratio |
| Dexterity Limitations | Large touch targets, voice input | 44x44px minimum touch areas, prominent voice controls |
| Cognitive Challenges | Simple layouts, clear language | Progressive disclosure, 6th-grade reading level |
| Tremor/Movement Issues | Forgiving touch targets, dwell clicking | Touch stabilization, adjustable timing |
| Fatigue Concerns | Efficient workflows, save states | Minimal steps, automatic progress saving |
| Hearing Limitations | Visual notifications, captions | Visual alerts, captioned media |

### 8.3 UI Component Specifications

Detailed component documentation has been moved to the Component Library for integration with the design system.

### 8.4 API Specification Details

Full API specifications are documented in the API Requirements Documentation.

## Change Log

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0.0 | 2025-04-14 | Product Lead | Initial document |
