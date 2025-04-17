# Data Model Extensions for Unified Platform

**Version:** 1.0.0  
**Last Updated:** 2025-04-14  
**Status:** Draft  
**Document Location:** `/07_Technical/Data_Models/data-model-extensions.md`

## 1. Overview

This document details the data model extensions required to support CareSupport's unified platform. It builds upon the foundation established during onboarding and care plan setup, extending those models to enable cross-role interactions, permission controls, and the adaptive functionality of our unified platform.

### 1.1 Related Documents

- `/07_Technical/Data_Models/data-fields.json`
- `/02_Research/Relationship_Model.md`
- `/08_Design_Standards/Component_Library/Component_Overview.md`
- `/06_Core_Platform/Post_Setup_Transition/Welcome_Home_Specification.md`

### 1.2 Purpose & Goals

- Extend existing data models from onboarding and setup
- Define data structures for cross-role interactions
- Establish permission and privacy frameworks at the data level
- Create relationship schema for core platform objects
- Provide implementation guidance for database architecture

## 2. Core Data Model Extensions

### 2.1 User Model Extension

The base user model from onboarding is extended to support the unified platform experience:

```json
{
  "user": {
    "id": "user123",
    "auth_id": "auth_provider_id",
    "email": "user@example.com",
    "phone": "+1234567890",
    "first_name": "Jane",
    "last_name": "Smith",
    "profile_photo_url": "https://storage.example.com/photos/user123.jpg",
    "roles": [
      {
        "type": "caregiver",
        "primary": true,
        "care_circles": ["circle123"]
      },
      {
        "type": "organizer",
        "primary": false,
        "care_circles": ["circle123"]
      }
    ],
    "preferences": {
      "notification_settings": {
        "push_enabled": true,
        "email_enabled": true,
        "sms_enabled": false
      },
      "theme": "default",
      "accessibility": {
        "reduced_motion": false,
        "high_contrast": false,
        "large_text": false
      },
      "language": "en-US"
    },
    "last_active_role": "caregiver",
    "last_login": "2025-04-14T12:30:00Z",
    "created_at": "2025-03-01T09:15:00Z",
    "onboarding_complete": true,
    "setup_complete": true
  }
}
```

#### Key Extensions:
- **Multiple roles support**: Users can now have multiple roles across care circles
- **Role context persistence**: Tracks which role the user was last active in
- **Enhanced preferences**: Platform-wide settings for notifications and accessibility
- **Setup state tracking**: Flags for onboarding and setup completion

### 2.2 Care Circle Model

The Care Circle becomes a central organizing entity for the platform:

```json
{
  "care_circle": {
    "id": "circle123",
    "name": "Mom's Care Circle",
    "care_recipient": {
      "user_id": "recipient456",
      "first_name": "Elizabeth",
      "last_name": "Smith",
      "profile_photo_url": "https://storage.example.com/photos/recipient456.jpg"
    },
    "members": [
      {
        "user_id": "user123",
        "roles": ["caregiver", "organizer"],
        "primary_caregiver": true,
        "relationship_to_recipient": "daughter",
        "permissions": {
          "admin": true,
          "invite": true,
          "health_write": true,
          "task_assign": true
        },
        "status": "active",
        "joined_at": "2025-03-01T09:15:00Z"
      },
      {
        "user_id": "user789",
        "roles": ["supporter"],
        "primary_caregiver": false,
        "relationship_to_recipient": "friend",
        "permissions": {
          "admin": false,
          "invite": false,
          "health_write": false,
          "task_assign": false
        },
        "status": "active",
        "joined_at": "2025-03-05T14:30:00Z"
      }
    ],
    "pending_invites": [
      {
        "email": "john@example.com",
        "invited_role": "supporter",
        "invited_by": "user123",
        "invited_at": "2025-04-10T11:20:00Z",
        "invite_code": "inv123abc",
        "status": "pending",
        "expiration": "2025-04-17T11:20:00Z"
      }
    ],
    "care_situation": "Recovering from hip surgery",
    "created_at": "2025-03-01T09:15:00Z",
    "created_by": "user123"
  }
}
```

#### Key Extensions:
- **Member management**: Complete member roster with roles and permissions
- **Invitation system**: Tracking pending invites with expiration logic
- **Permission framework**: Granular permissions for each member
- **Relationship context**: Preserves relationship data from onboarding

### 2.3 Task Model

Tasks are the primary activity coordination object in the platform:

```json
{
  "task": {
    "id": "task123",
    "title": "Pick up medications",
    "description": "Get refills from Walgreens on Main St",
    "category": "medication",
    "priority": "high",
    "due_date": "2025-04-15T15:00:00Z",
    "recurrence": {
      "pattern": "weekly",
      "days": ["monday"],
      "end_date": null,
      "count": null
    },
    "status": "assigned",
    "created_at": "2025-04-10T09:30:00Z",
    "created_by": {
      "user_id": "user123",
      "role": "caregiver"
    },
    "assigned_to": {
      "user_id": "user789",
      "role": "supporter",
      "accepted": true,
      "accepted_at": "2025-04-10T10:15:00Z"
    },
    "care_recipient": {
      "user_id": "recipient456"
    },
    "care_circle_id": "circle123",
    "notes": [
      {
        "content": "Please get the generic version if available",
        "author_id": "user123",
        "created_at": "2025-04-10T09:32:00Z"
      }
    ],
    "attachments": [],
    "reminders": [
      {
        "time": "2025-04-15T11:00:00Z",
        "sent_to": ["user789"],
        "sent": false
      }
    ],
    "completion": {
      "completed": false,
      "completed_at": null,
      "completed_by": null,
      "verification": null
    },
    "privacy_level": "circle",
    "visibility_overrides": [], 
    "history": [
      {
        "action": "created",
        "actor_id": "user123",
        "timestamp": "2025-04-10T09:30:00Z"
      },
      {
        "action": "assigned",
        "actor_id": "user123",
        "timestamp": "2025-04-10T09:31:00Z",
        "details": {
          "assigned_to": "user789"
        }
      }
    ]
  }
}
```

#### Key Features:
- **Comprehensive task metadata**: Details, category, priority, etc.
- **Assignment system**: Tracking who's responsible with acceptance state
- **Recurrence patterns**: Support for recurring tasks 
- **Reminders**: Time-based notification system
- **Privacy controls**: Granular visibility settings
- **History tracking**: Complete audit trail
- **Notes and attachments**: Collaborative information sharing

### 2.4 Event Model

Events represent time-based calendar entries:

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
    "attachments": [],
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
  }
}
```

#### Key Features:
- **Comprehensive event details**: Location, time, category
- **Attendee management**: Who's involved with status tracking
- **Multi-level reminders**: Different timings and messages for different roles
- **Transportation coordination**: Tracking arrangements for mobility
- **Privacy framework**: Controls who can see the event
- **Notes and history**: Complete context preservation

### 2.5 Update Model

Updates provide the communication and status sharing foundation:

```json
{
  "update": {
    "id": "update123",
    "type": "observation",
    "content": "Mom seemed more tired than usual today, but ate a full lunch",
    "media": [
      {
        "type": "image",
        "url": "https://storage.example.com/media/img123.jpg",
        "thumbnail_url": "https://storage.example.com/media/thumb_img123.jpg",
        "created_at": "2025-04-14T15:30:00Z"
      }
    ],
    "timestamp": "2025-04-14T15:30:00Z",
    "author": {
      "user_id": "user123",
      "role": "caregiver"
    },
    "about": {
      "user_id": "recipient456",
      "role": "care_recipient"
    },
    "care_circle_id": "circle123",
    "mood_indicator": "concerned",
    "tags": ["energy", "appetite"],
    "related_items": [
      {
        "type": "task",
        "id": "task456",
        "title": "Monitor energy levels"
      }
    ],
    "privacy_level": "caregivers_only",
    "visibility_overrides": [],
    "reactions": [
      {
        "user_id": "user789",
        "type": "acknowledge",
        "timestamp": "2025-04-14T15:45:00Z"
      }
    ],
    "comments": [
      {
        "id": "comment123",
        "content": "Thanks for the update. I'll check in on her this evening.",
        "author_id": "user789",
        "timestamp": "2025-04-14T15:46:00Z",
        "edited": false
      }
    ]
  }
}
```

#### Key Features:
- **Multiple update types**: Observations, status reports, journal entries
- **Media support**: Photos and other attachments
- **Emotional context**: Mood indicators and contextual tags
- **Privacy controls**: Granular visibility settings
- **Social features**: Reactions and comments
- **Relationship tracking**: Updates can be about a care recipient or activity

### 2.6 Health Data Model

Specialized model for health-related information:

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
  }
}
```

#### Key Features:
- **Structured health metrics**: Type-specific value formats
- **Contextual information**: Circumstances of measurement
- **Relationship mapping**: Connections to conditions and medications
- **Enhanced privacy**: Stricter default controls for health data
- **History tracking**: Audit trail for medical information

### 2.7 Medication Model

Detailed tracking for medication management:

```json
{
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
    "history": [
      {
        "action": "created",
        "actor_id": "user123",
        "timestamp": "2025-01-15T09:30:00Z"
      },
      {
        "action": "updated",
        "actor_id": "user123",
        "timestamp": "2025-03-01T14:15:00Z",
        "details": {
          "field": "supply.refills",
          "old_value": 3,
          "new_value": 2
        }
      }
    ]
  }
}
```

#### Key Features:
- **Comprehensive medication details**: Name, dosage, form, instructions
- **Scheduling system**: When and how to take medications
- **Supply tracking**: Inventory management with reminders
- **Prescription information**: Prescriber, pharmacy, Rx number
- **Historical tracking**: Complete medication history
- **Visual identification**: Image references for pills

## 3. Privacy & Permissions Framework

### 3.1 Privacy Levels

| Level | Code | Description | Default Visibility | Override Capability |
|-------|------|-------------|---------------------|---------------------|
| Private | `private` | Visible only to creator and Care Recipient | Creator, Care Recipient | None |
| Caregivers Only | `caregivers_only` | Limited to Caregivers and Care Recipient | Caregivers, Care Recipient | Individual overrides |
| Care Team | `care_team` | Caregivers, Organizers, and Care Recipient | Caregivers, Organizers, Care Recipient | Individual overrides |
| Circle | `circle` | All members including Supporters | All circle members | Individual overrides |
| Public | `public` | Anyone with link, including future members | Everyone | Blacklist only |

### 3.2 Permission Framework

Base permission structure for Care Circle members:

```json
{
  "permissions": {
    "admin": {
      "description": "Can manage circle settings and members",
      "default_roles": ["caregiver"],
      "capabilities": [
        "manage_members",
        "invite_members",
        "remove_members",
        "edit_circle_settings"
      ]
    },
    "health_write": {
      "description": "Can record health information",
      "default_roles": ["caregiver"],
      "capabilities": [
        "create_health_records",
        "update_health_records",
        "create_medication",
        "update_medication"
      ]
    },
    "health_read": {
      "description": "Can view health information",
      "default_roles": ["caregiver", "organizer"],
      "capabilities": [
        "view_health_records",
        "view_medications",
        "view_health_updates"
      ]
    },
    "task_create": {
      "description": "Can create tasks",
      "default_roles": ["caregiver", "organizer"],
      "capabilities": [
        "create_tasks",
        "edit_own_tasks",
        "delete_own_tasks"
      ]
    },
    "task_assign": {
      "description": "Can assign tasks to others",
      "default_roles": ["caregiver", "organizer"],
      "capabilities": [
        "assign_tasks",
        "reassign_tasks"
      ]
    },
    "event_create": {
      "description": "Can create events",
      "default_roles": ["caregiver", "organizer", "care_recipient"],
      "capabilities": [
        "create_events",
        "edit_own_events",
        "delete_own_events"
      ]
    },
    "update_create": {
      "description": "Can post updates",
      "default_roles": ["caregiver", "organizer", "care_recipient", "supporter"],
      "capabilities": [
        "create_updates",
        "edit_own_updates",
        "delete_own_updates"
      ]
    }
  }
}
```

### 3.3 Role-Based Default Permissions

| Permission | Caregiver | Organizer | Supporter | Care Recipient |
|------------|-----------|-----------|-----------|----------------|
| admin | ✅ | 🔶* | ❌ | ❌ |
| health_write | ✅ | ❌ | ❌ | ✅ |
| health_read | ✅ | 🔶** | ❌ | ✅ |
| task_create | ✅ | ✅ | ❌ | ✅ (needs only) |
| task_assign | ✅ | ✅ | ❌ | ❌ |
| event_create | ✅ | ✅ | ❌ | ✅ (personal only) |
| update_create | ✅ | ✅ | ✅ (limited) | ✅ |

*Organizer admin rights are limited to task and schedule management
**Organizer health read permission depends on Care Recipient privacy settings

### 3.4 Visibility Override Structure

Visibility can be customized with user-specific overrides:

```json
"visibility_overrides": [
  {
    "type": "include",
    "user_id": "user789",
    "timestamp": "2025-04-14T10:00:00Z",
    "override_by": "recipient456"
  },
  {
    "type": "exclude",
    "user_id": "user101",
    "timestamp": "2025-04-14T10:00:00Z",
    "override_by": "recipient456"
  }
]
```

## 4. Relationship Schema

### 4.1 Primary Entity Relationships

```
User --< Roles >-- Care Circle
     \           /
      \         /
       v       v
       Member Role
           |
           |
  +--------+--------+
  |        |        |
Tasks    Events   Updates
  |        |        |
  +---+----+--------+
      |
Health Data & Medications
```

### 4.2 Key Relationships

| Entity A | Relationship | Entity B | Implementation |
|----------|--------------|----------|----------------|
| User | Has many | Roles | Array of roles in user object |
| User | Belongs to many | Care Circles | Through user roles |
| Care Circle | Has one | Care Recipient | Direct reference in circle |
| Care Circle | Has many | Members | Array of members in circle |
| Member | Creates many | Tasks | `created_by` reference |
| Member | Assigned many | Tasks | `assigned_to` reference |
| Task | Belongs to | Care Circle | Direct reference |
| Event | Has many | Attendees | Array of attendees |
| Update | Created by | Member | Author reference |
| Health Data | About | Care Recipient | Direct reference |
| Medication | For | Care Recipient | Direct reference |

## 5. Migration Strategy

### 5.1 Onboarding to Unified Platform Data Migration

| Onboarding Data | Migration Strategy | Unified Platform Location |
|-----------------|-------------------|---------------------------|
| Role | Create User role record | User.roles array |
| Relationship | Create Care Circle member record | CareCircle.members relationship data |
| Care Recipient Details | Create Care Recipient user record | User + CareCircle.care_recipient |
| Care Situation | Transfer directly | CareCircle.care_situation |
| Primary Challenge | Create initial Task records | Tasks with appropriate categories |
| Quick Win | Create completed Task record | Tasks with completion status |
| Care Circle Invites | Create Pending Invites | CareCircle.pending_invites |
| Privacy Preferences | Create Privacy Settings | User permissions + default privacy levels |
| Expressed Needs | Create initial Tasks | Tasks assigned to appropriate roles |
| Daily Rhythm | Create recurring Events | Events with appropriate recurrence |
| Privacy Acknowledgment | Set permission defaults | User permission settings |

### 5.2 Incremental Data Extensions

For existing users migrating from onboarding to unified platform:

1. **Creation of Care Circle**
   - Generate Care Circle object using onboarding data
   - Establish primary relationships

2. **User Role Expansion**
   - Convert single-role users to multi-role model
   - Set appropriate permissions

3. **Task & Event Bootstrapping**
   - Generate initial task set from setup data
   - Create schedule from daily rhythm

4. **Privacy Settings Application**
   - Apply user-defined privacy preferences
   - Set default visibility levels

## 6. Technical Implementation Considerations

### 6.1 Database Recommendations

| Data Store Type | Recommended Usage | Considerations |
|-----------------|-------------------|----------------|
| Document DB | User profiles, Tasks, Events, Updates | Flexible schema, good for evolving objects |
| Graph DB | Relationships, Permissions | Efficient for complex relationship queries |
| Relational DB | Medications, Health Records | ACID compliance for medical data |
| Time Series DB | Health metrics, Activity logs | Efficient for temporal analytics |
| Cache Layer | Active sessions, Recent activity | Performance optimization |

### 6.2 Security Requirements

- **Encryption**: All data encrypted at rest and in transit
- **Access Control**: Role-based access implemented at API and database levels
- **Audit Logging**: Complete history for all sensitive operations
- **Data Partitioning**: Separate storage for different privacy levels

### 6.3 Performance Considerations

- **Denormalization**: Strategic duplication for read performance
- **Indexing Strategy**: Optimize for common queries by role
- **Caching**: Multi-level cache for frequently accessed objects
- **Query Optimization**: Role-specific query patterns

## 7. API Structure

### 7.1 Core API Endpoints

| Resource | GET | POST | PUT | DELETE |
|----------|-----|------|-----|--------|
| /api/users/:id | Get user profile | n/a | Update user | n/a |
| /api/care-circles | List user's circles | Create circle | n/a | n/a |
| /api/care-circles/:id | Get circle details | n/a | Update circle | Archive circle |
| /api/care-circles/:id/members | List members | Add member | n/a | n/a |
| /api/care-circles/:id/members/:member_id | Get member details | n/a | Update member | Remove member |
| /api/care-circles/:id/invites | List invites | Create invite | n/a | n/a |
| /api/tasks | List tasks | Create task | n/a | n/a |
| /api/tasks/:id | Get task details | n/a | Update task | Delete task |
| /api/events | List events | Create event | n/a | n/a |
| /api/events/:id | Get event details | n/a | Update event | Delete event |
| /api/updates | List updates | Create update | n/a | n/a |
| /api/updates/:id | Get update details | n/a | Update update | Delete update |
| /api/health-data | List health records | Create health record | n/a | n/a |
| /api/medications | List medications | Create medication | n/a | n/a |
| /api/medications/:id | Get medication details | n/a | Update medication | Archive medication |

### 7.2 API Parameter Patterns

Common query parameters for role-based content adaptation:

```
?role=caregiver           // View content as specific role
?view=assigned            // Filter to assigned items only
?include=health           // Include health-related data
?exclude=completed        // Exclude completed items
?privacy=caregivers_only  // Filter by privacy level
?since=2025-04-10T00:00:00Z  // Only items since timestamp
```

## 8. Next Steps

1. Review data model extensions with engineering team
2. Create database schema diagrams
3. Define detailed API specifications
4. Develop migration scripts for onboarding data
5. Implement data validation procedures

## Change Log

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0.0 | 2025-04-14 | Product Lead | Initial document |
