<!--
Date: April 17, 2025
Purpose: Technical Specifications Summary for CareSupport iOS app
Contains: Data models, API requirements, permission framework, and state management details
Created by: Cascade AI
-->

# CareSupport iOS: Technical Specifications Summary

**Document Version:** 1.0.0  
**Last Updated:** 2025-04-17  

## 1. Data Models

### 1.1 Core Entities

#### User

```swift
struct User {
    let id: String
    let authId: String
    let email: String
    let phone: String?
    let firstName: String
    let lastName: String
    let profilePhotoUrl: URL?
    let roles: [UserRole]
    let preferences: UserPreferences
    let lastActiveRole: RoleType
    let lastLogin: Date
    let createdAt: Date
    let onboardingComplete: Bool
    let setupComplete: Bool
}

struct UserRole {
    let type: RoleType
    let primary: Bool
    let careCircleIds: [String]
}

enum RoleType {
    case caregiver
    case careRecipient
    case organizer
    case supporter
}

struct UserPreferences {
    let notificationSettings: NotificationSettings
    let theme: ThemeType
    let accessibility: AccessibilitySettings
    let language: String
}
```

#### Care Circle

```swift
struct CareCircle {
    let id: String
    let name: String
    let careRecipient: CareRecipientInfo
    let members: [CircleMember]
    let pendingInvites: [PendingInvite]
    let careSituation: String
    let createdAt: Date
    let createdBy: String
}

struct CareRecipientInfo {
    let userId: String
    let firstName: String
    let lastName: String
    let profilePhotoUrl: URL?
}

struct CircleMember {
    let userId: String
    let roles: [RoleType]
    let primaryCaregiver: Bool
    let relationshipToRecipient: String
    let permissions: MemberPermissions
    let status: MemberStatus
    let joinedAt: Date
}

struct PendingInvite {
    let email: String
    let invitedRole: RoleType
    let invitedBy: String
    let invitedAt: Date
    let inviteCode: String
    let status: InviteStatus
    let expiration: Date
}
```

#### Task

```swift
struct Task {
    let id: String
    let title: String
    let description: String?
    let category: TaskCategory
    let priority: TaskPriority
    let dueDate: Date
    let recurrence: RecurrencePattern?
    let status: TaskStatus
    let createdAt: Date
    let createdBy: TaskActor
    let assignedTo: TaskAssignment?
    let careRecipient: CareRecipientInfo
    let careCircleId: String
    let notes: [TaskNote]
    let attachments: [Attachment]
    let reminders: [TaskReminder]
    let completionDetails: CompletionDetails?
    let tags: [String]
}

struct TaskActor {
    let userId: String
    let role: RoleType
}

struct TaskAssignment {
    let userId: String
    let role: RoleType
    let accepted: Bool
    let acceptedAt: Date?
}

struct TaskNote {
    let content: String
    let authorId: String
    let createdAt: Date
}

struct TaskReminder {
    let time: Date
    let sentTo: [String]
    let sent: Bool
}

struct CompletionDetails {
    let completedAt: Date
    let completedBy: String
    let notes: String?
    let attachments: [Attachment]
}
```

#### Health Record

```swift
struct HealthRecord {
    let id: String
    let type: HealthRecordType
    let title: String
    let description: String?
    let date: Date
    let careRecipientId: String
    let careCircleId: String
    let createdBy: TaskActor
    let createdAt: Date
    let privacyLevel: PrivacyLevel
    let visibleTo: [String]?
    let data: HealthData
    let attachments: [Attachment]
    let relatedEvents: [String]?
}

enum HealthRecordType {
    case medication
    case symptom
    case measurement
    case appointment
    case observation
    case document
}

struct HealthData {
    // Various type-specific properties based on HealthRecordType
}

enum PrivacyLevel {
    case private         // Only care recipient
    case restricted      // Care recipient + specified users
    case caregivers      // Care recipient + all caregivers
    case circle          // All circle members
}
```

#### Communication

```swift
struct Message {
    let id: String
    let conversationId: String
    let senderId: String
    let senderRole: RoleType
    let content: String
    let timestamp: Date
    let readBy: [MessageReadStatus]
    let attachments: [Attachment]
    let mentions: [String]?
}

struct Conversation {
    let id: String
    let type: ConversationType
    let participants: [ConversationParticipant]
    let careCircleId: String
    let title: String?
    let createdAt: Date
    let lastActivity: Date
}

enum ConversationType {
    case direct      // Between two users
    case group       // Selected group of users
    case circle      // Entire care circle
    case announcement // Broadcast style
}

struct Update {
    let id: String
    let type: UpdateType
    let content: String
    let authorId: String
    let authorRole: RoleType
    let careCircleId: String
    let timestamp: Date
    let visibleTo: VisibilityScope
    let reactions: [Reaction]
    let comments: [UpdateComment]
    let attachments: [Attachment]
}
```

### 1.2 Entity Relationships

```
┌─────────────┐        owns        ┌─────────────┐
│    User     │◄─────────────────► │  UserRole   │
└─────────────┘                    └─────────────┘
       │                                  │
       │ belongs to                       │ has
       │                                  │
       ▼                                  ▼
┌─────────────┐      contains      ┌─────────────┐
│ Care Circle │◄─────────────────► │CircleMember │
└─────────────┘                    └─────────────┘
       │                                  ▲
       │ has                              │ can be
       │                                  │
       ▼                                  │
┌─────────────┐                    ┌─────────────┐
│CareRecipient│                    │   User      │
└─────────────┘                    └─────────────┘
       │
       │ relates to
       │
       ▼
┌─────────────┐      assigned     ┌─────────────┐
│    Task     │◄─────────────────►│CircleMember │
└─────────────┘                   └─────────────┘
       │
       │ belongs to
       │
       ▼
┌─────────────┐
│ Care Circle │
└─────────────┘
```

## 2. API Requirements

### 2.1 API Architecture

- RESTful API with JSON request/response format
- JWT-based authentication
- Role-based authorization
- Versioned endpoints (`/v1/resource`)
- HTTP/HTTPS-only communication

### 2.2 Authentication Endpoints

| Endpoint | Method | Purpose | Request Data | Response Data |
|----------|--------|---------|--------------|---------------|
| `/auth/login` | POST | Log in with credentials | `{"email", "password"}` | `{"token", "refresh_token", "user"}` |
| `/auth/refresh` | POST | Refresh access token | `{"refresh_token"}` | `{"token", "refresh_token"}` |
| `/auth/logout` | POST | Invalidate token | `{}` | `{}` |
| `/auth/password/reset` | POST | Reset password | `{"email"}` | `{}` |

### 2.3 Core Resource Endpoints

| Resource | Endpoints | Methods | Description |
|----------|-----------|---------|-------------|
| Users | `/users`, `/users/{id}` | GET, PUT, PATCH | User profile management |
| Care Circles | `/circles`, `/circles/{id}` | GET, POST, PUT | Care circle CRUD operations |
| Members | `/circles/{id}/members` | GET, POST, PUT, DELETE | Circle membership management |
| Invites | `/invites`, `/invites/{code}` | GET, POST, PUT | Invitation management |
| Tasks | `/tasks`, `/tasks/{id}` | GET, POST, PUT, DELETE | Task CRUD operations |
| Health Records | `/health`, `/health/{id}` | GET, POST, PUT, DELETE | Health data management |
| Communication | `/messages`, `/conversations` | GET, POST, PUT | Messaging system |
| Updates | `/updates`, `/updates/{id}` | GET, POST, PUT, DELETE | Status updates |
| Calendar | `/events`, `/events/{id}` | GET, POST, PUT, DELETE | Calendar management |

### 2.4 Request Format

```json
{
  "data": {
    // Request payload
  },
  "meta": {
    "client_timestamp": "2025-04-14T15:30:00Z",
    "timezone": "America/Los_Angeles",
    "locale": "en-US",
    "app_version": "1.0.0"
  }
}
```

### 2.5 Response Format

```json
{
  "data": {
    // Response payload
  },
  "meta": {
    "server_timestamp": "2025-04-14T15:30:05Z",
    "request_id": "req123456",
    "status": "success",
    "pagination": {
      "total": 100,
      "page": 1,
      "per_page": 20,
      "next_page": 2
    }
  },
  "errors": []
}
```

### 2.6 Error Format

```json
{
  "data": null,
  "meta": {
    "server_timestamp": "2025-04-14T15:30:05Z",
    "request_id": "req123456",
    "status": "error"
  },
  "errors": [
    {
      "code": "invalid_input",
      "message": "The task title must not be empty",
      "field": "data.title",
      "severity": "error"
    }
  ]
}
```

## 3. Permission Framework

### 3.1 Permission Layers

The permission system uses a layered approach that combines:

1. **Role-Based Access Control (RBAC)**: Baseline permissions assigned by role
2. **Attribute-Based Access Control (ABAC)**: Contextual permission adjustments
3. **Relationship-Based Access**: Permissions based on connection to Care Recipient
4. **Object-Level Permissions**: Fine-grained control at the individual data object level
5. **User Consent Overrides**: Care Recipient permission controls that can override defaults

### 3.2 Permission Evaluation Flow

1. Check system-level restrictions (hard limits)
2. Check role-based default permissions
3. Apply relationship context modifiers
4. Apply care circle specific permissions
5. Check object-level permissions
6. Apply any user overrides

### 3.3 Role Permission Matrix

| Action | Caregiver | Organizer | Supporter | Care Recipient |
|--------|-----------|-----------|-----------|----------------|
| View Tasks | All | All | Assigned only | All |
| Create Tasks | ✓ | ✓ | - | Request only |
| Assign Tasks | ✓ | ✓ | - | - |
| View Health | ✓ | Limited | - | ✓ |
| Add Health | ✓ | - | - | ✓ |
| Invite Members | ✓ | Limited | - | Limited |
| Remove Members | ✓ | - | - | - |
| View Calendar | ✓ | ✓ | Limited | ✓ |
| Create Events | ✓ | ✓ | - | ✓ |

### 3.4 Privacy Levels

| Level | Description | Example |
|-------|-------------|---------|
| Private | Visible only to owner | Personal journal entries |
| Restricted | Visible to specific users | Sensitive health information |
| Caregivers | Visible to all caregivers | Medication schedule |
| Circle | Visible to all circle members | General updates |

## 4. State Management

### 4.1 State Categories

#### Persistent State
- User profile and preferences
- Care circle memberships
- Entity data (tasks, events, etc.)
- Privacy settings

#### Session State
- Active role context
- UI view state
- Navigation history
- Form progress

#### Ephemeral State
- Loading indicators
- Animations
- Temporary notifications

### 4.2 Client-Side State Architecture

```swift
// Application State
class AppState: ObservableObject {
    @Published var authState: AuthState
    @Published var userProfile: UserProfile
    @Published var activeCircle: CareCircle?
}

// Role Context
class RoleContext: ObservableObject {
    @Published var activeRole: RoleType
    @Published var permissions: [Permission]
    @Published var roleSettings: RoleSpecificSettings
}

// Entity State
class EntityState: ObservableObject {
    @Published var tasks: TaskState
    @Published var healthRecords: HealthState
    @Published var communications: CommunicationState
    // Other entity states
}

// View State
class ViewState: ObservableObject {
    @Published var currentView: AppView
    @Published var uiMode: UIMode
    @Published var formState: FormState?
}
```

### 4.3 State Synchronization

- **Initial Load**: Fetch core state on app launch
- **Pull Refresh**: Manual refresh of views
- **Push Updates**: Realtime updates via push notifications
- **Background Sync**: Periodic background synchronization
- **Conflict Resolution**: Last-write-wins with conflict detection

### 4.4 Offline Support Strategy

- **Essential Data Caching**: Core data available offline
- **Operation Queueing**: Actions queued when offline
- **Sync Status Indicators**: Clear UI indicators for sync state
- **Conflict Resolution UI**: User-friendly conflict resolution
- **Partial Sync**: Smart syncing of most relevant data first
