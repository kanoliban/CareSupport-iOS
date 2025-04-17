# API Requirements Documentation

**Version:** 1.0.0  
**Last Updated:** 2025-04-14  
**Status:** Draft  
**Document Location:** `/07_Technical/API_Requirements/Core_API_Specs.md`

## 1. Overview

This document defines the API requirements for CareSupport's unified platform, establishing the interface between frontend components and backend services. It builds upon the data models defined in the Data Model Extensions document, providing the technical specifications necessary for implementation.

### 1.1 Related Documents

- `/07_Technical/Data_Models/data-model-extensions.md`
- `/02_Research/Relationship_Model.md`
- `/03_Information_Architecture/Navigation_System.md`
- `/08_Design_Standards/Component_Library/Component_Overview.md`

### 1.2 Purpose & Goals

- Define the API architecture and principles
- Establish authorization and authentication requirements
- Specify core endpoints, request/response patterns, and error handling
- Ensure the API supports role-based content adaptation
- Provide implementation guidance for engineering

## 2. API Architecture

### 2.1 Architecture Overview

CareSupport's API follows a RESTful architecture with these key characteristics:

- Resource-oriented endpoints
- JSON request/response format
- JWT-based authentication
- Role-based authorization
- Versioned endpoints
- HTTPS-only communication

### 2.2 Base URL Structure

```
https://api.caresupport.com/v1/{resource}
```

### 2.3 API Versioning

Versions are specified in the URL path to ensure compatibility:

- `v1`: Initial API version

When breaking changes are introduced, a new version is created while maintaining backward compatibility for a defined period.

## 3. Authentication & Authorization

### 3.1 Authentication Mechanism

The API uses JSON Web Tokens (JWT) for authentication:

- **Token Acquisition**: OAuth2 flow with email/password or social provider
- **Token Format**: JWT with appropriate expiration and claims
- **Refresh Mechanism**: Refresh tokens for extended sessions
- **Session Management**: Support for multiple devices and token revocation

### 3.2 Authorization Framework

Authorization is based on:

1. **User Role**: Determined by the active role in the user's session
2. **Circle Membership**: Based on the user's membership in a care circle
3. **Permission Grants**: Specific permissions assigned to the user
4. **Privacy Settings**: Object-level visibility settings
5. **Override Rules**: Custom visibility overrides

### 3.3 Role Context

All API requests include the user's current active role:

```
Authorization: Bearer {jwt_token}
X-Role-Context: caregiver
```

This header ensures content is filtered appropriately for the user's current role.

## 4. Core API Patterns

### 4.1 Request Format

Standard request format for all endpoints:

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

### 4.2 Response Format

Standard response format for all endpoints:

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

### 4.3 Error Format

Standardized error responses:

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

### 4.4 Pagination Pattern

For list endpoints:

- **Page-based**: `?page=2&per_page=20`
- **Cursor-based**: `?cursor=abc123&limit=20` (for large datasets)
- **Default limit**: 20 items per page
- **Maximum limit**: 100 items per page

### 4.5 Filtering Pattern

Common query parameters for filtering:

```
?filter[status]=active            // Filter by field value
?filter[due_date][gt]=2025-04-15  // Comparison operators
?filter[tags]=medication,urgent   // Multiple values
?search=doctor                    // Full-text search
```

### 4.6 Sorting Pattern

```
?sort=due_date                    // Sort ascending
?sort=-due_date                   // Sort descending
?sort=-priority,due_date          // Multiple sort fields
```

### 4.7 Field Selection

```
?fields=id,title,due_date         // Include only specific fields
?include=assigned_to,notes        // Include related objects
```

## 5. Core API Endpoints

### 5.1 Authentication Endpoints

| Endpoint | Method | Purpose | Request Data | Response Data |
|----------|--------|---------|--------------|---------------|
| `/auth/login` | POST | Log in with credentials | `{"email", "password"}` | `{"token", "refresh_token", "user"}` |
| `/auth/refresh` | POST | Refresh access token | `{"refresh_token"}` | `{"token", "refresh_token"}` |
| `/auth/logout` | POST | Invalidate token | `{}` | `{}` |
| `/auth/password/reset` | POST | Reset password | `{"email"}` | `{}` |

### 5.2 User & Profile Endpoints

| Endpoint | Method | Purpose | Request Data | Response Data |
|----------|--------|---------|--------------|---------------|
| `/users/me` | GET | Get current user | - | User profile |
| `/users/me/roles` | GET | Get user roles | - | List of roles |
| `/users/me/roles/:role` | POST | Switch active role | `{"role"}` | Updated user profile |
| `/users/me/preferences` | GET | Get user preferences | - | User preferences |
| `/users/me/preferences` | PUT | Update preferences | Preference object | Updated preferences |

### 5.3 Care Circle Endpoints

| Endpoint | Method | Purpose | Request Data | Response Data |
|----------|--------|---------|--------------|---------------|
| `/care-circles` | GET | List user's circles | - | List of care circles |
| `/care-circles` | POST | Create new circle | Circle object | Created circle |
| `/care-circles/:id` | GET | Get circle details | - | Circle details |
| `/care-circles/:id` | PUT | Update circle | Circle object | Updated circle |
| `/care-circles/:id/members` | GET | List circle members | - | List of members |
| `/care-circles/:id/members` | POST | Add member | Member object | Added member |
| `/care-circles/:id/members/:member_id` | GET | Get member details | - | Member details |
| `/care-circles/:id/members/:member_id` | PUT | Update member | Member object | Updated member |
| `/care-circles/:id/members/:member_id` | DELETE | Remove member | - | Success status |
| `/care-circles/:id/invites` | GET | List pending invites | - | List of invites |
| `/care-circles/:id/invites` | POST | Create invite | Invite object | Created invite |
| `/care-circles/:id/invites/:invite_id` | DELETE | Cancel invite | - | Success status |

### 5.4 Task Endpoints

| Endpoint | Method | Purpose | Request Data | Response Data |
|----------|--------|---------|--------------|---------------|
| `/tasks` | GET | List tasks | - | List of tasks |
| `/tasks` | POST | Create task | Task object | Created task |
| `/tasks/:id` | GET | Get task details | - | Task details |
| `/tasks/:id` | PUT | Update task | Task object | Updated task |
| `/tasks/:id` | DELETE | Delete task | - | Success status |
| `/tasks/:id/assign` | POST | Assign task | `{"assignee_id"}` | Updated task |
| `/tasks/:id/complete` | POST | Complete task | `{"verification"}` | Updated task |
| `/tasks/:id/notes` | GET | Get task notes | - | List of notes |
| `/tasks/:id/notes` | POST | Add task note | Note object | Added note |

### 5.5 Event Endpoints

| Endpoint | Method | Purpose | Request Data | Response Data |
|----------|--------|---------|--------------|---------------|
| `/events` | GET | List events | - | List of events |
| `/events` | POST | Create event | Event object | Created event |
| `/events/:id` | GET | Get event details | - | Event details |
| `/events/:id` | PUT | Update event | Event object | Updated event |
| `/events/:id` | DELETE | Delete event | - | Success status |
| `/events/:id/attendees` | GET | List attendees | - | List of attendees |
| `/events/:id/attendees` | POST | Add attendee | Attendee object | Added attendee |
| `/events/:id/attendees/:user_id` | DELETE | Remove attendee | - | Success status |
| `/events/:id/respond` | POST | Respond to event | `{"status"}` | Updated event |

### 5.6 Update Endpoints

| Endpoint | Method | Purpose | Request Data | Response Data |
|----------|--------|---------|--------------|---------------|
| `/updates` | GET | List updates | - | List of updates |
| `/updates` | POST | Create update | Update object | Created update |
| `/updates/:id` | GET | Get update details | - | Update details |
| `/updates/:id` | PUT | Update update | Update object | Updated update |
| `/updates/:id` | DELETE | Delete update | - | Success status |
| `/updates/:id/comments` | GET | Get comments | - | List of comments |
| `/updates/:id/comments` | POST | Add comment | Comment object | Added comment |
| `/updates/:id/react` | POST | React to update | `{"reaction_type"}` | Updated reactions |

### 5.7 Health Data Endpoints

| Endpoint | Method | Purpose | Request Data | Response Data |
|----------|--------|---------|--------------|---------------|
| `/health-data` | GET | List health records | - | List of health records |
| `/health-data` | POST | Create health record | Health data object | Created record |
| `/health-data/:id` | GET | Get record details | - | Record details |
| `/health-data/:id` | PUT | Update record | Health data object | Updated record |
| `/health-data/metrics/:metric` | GET | Get metric history | - | Metric history |
| `/health-data/summary` | GET | Get health summary | - | Health summary |

### 5.8 Medication Endpoints

| Endpoint | Method | Purpose | Request Data | Response Data |
|----------|--------|---------|--------------|---------------|
| `/medications` | GET | List medications | - | List of medications |
| `/medications` | POST | Create medication | Medication object | Created medication |
| `/medications/:id` | GET | Get medication details | - | Medication details |
| `/medications/:id` | PUT | Update medication | Medication object | Updated medication |
| `/medications/:id` | DELETE | Archive medication | - | Success status |
| `/medications/:id/schedule` | GET | Get med schedule | - | Medication schedule |
| `/medications/:id/schedule` | PUT | Update schedule | Schedule object | Updated schedule |
| `/medications/:id/supply` | PUT | Update supply | Supply object | Updated supply |
| `/medications/:id/history` | GET | Get med history | - | Medication history |

## 6. Role-Based API Adaptations

### 6.1 Role-Based Content Filtering

Based on the user's active role (from `X-Role-Context` header), the API automatically:

1. **Filters list results** to show only appropriate items
2. **Adjusts detail level** in responses
3. **Enforces permission constraints** on actions
4. **Applies privacy rules** based on the privacy level of objects

### 6.2 Role-Specific Response Variations

#### Example: GET /tasks/:id

**Caregiver Response:**
```json
{
  "data": {
    "id": "task123",
    "title": "Pick up medications",
    "description": "Get refills from Walgreens on Main St",
    "category": "medication",
    "priority": "high",
    "due_date": "2025-04-15T15:00:00Z",
    "recurrence": { ... },
    "status": "assigned",
    "created_at": "2025-04-10T09:30:00Z",
    "created_by": { ... },
    "assigned_to": { ... },
    "care_recipient": { ... },
    "notes": [ ... ],
    "completion": { ... }
  },
  "meta": { ... }
}
```

**Supporter Response:**
```json
{
  "data": {
    "id": "task123",
    "title": "Pick up medications",
    "description": "Get refills from Walgreens on Main St",
    "due_date": "2025-04-15T15:00:00Z",
    "status": "assigned",
    "assigned_to": { ... },
    "care_recipient": {
      "first_name": "Elizabeth"
    },
    "notes": [ ... ]
  },
  "meta": { ... }
}
```

### 6.3 Permission-Based Action Availability

The API enforces permissions based on role:

```json
{
  "data": {
    "id": "task123",
    "title": "Pick up medications",
    "actions": {
      "can_edit": true,
      "can_delete": true,
      "can_assign": true,
      "can_complete": false
    },
    ...
  },
  "meta": { ... }
}
```

### 6.4 Privacy-Level Enforcement

The API respects privacy levels and overrides:

1. Objects with appropriate privacy level appear in list results
2. Object visibility is checked before returning detail responses
3. Privacy level checks consider role and individual overrides
4. Appropriate error responses for unauthorized access attempts

## 7. Error Handling

### 7.1 Error Codes

| Error Code | HTTP Status | Description | Example |
|------------|-------------|-------------|---------|
| `authentication_required` | 401 | Authentication is required | Missing or invalid token |
| `invalid_credentials` | 401 | Credentials are invalid | Wrong email/password |
| `permission_denied` | 403 | User doesn't have permission | Trying to edit another's task |
| `not_found` | 404 | Resource not found | Task doesn't exist |
| `validation_error` | 422 | Input validation failed | Invalid task data |
| `conflict` | 409 | Resource conflict | Create duplicate event |
| `rate_limited` | 429 | Too many requests | Exceeded API limits |
| `service_unavailable` | 503 | Service temporarily unavailable | Maintenance window |
| `internal_error` | 500 | Unexpected server error | Unknown system issue |

### 7.2 Field Validation Errors

For validation errors, the API returns detailed field errors:

```json
{
  "errors": [
    {
      "code": "required_field",
      "message": "Title is required",
      "field": "data.title",
      "severity": "error"
    },
    {
      "code": "invalid_date",
      "message": "Due date must be in the future",
      "field": "data.due_date",
      "severity": "error"
    }
  ]
}
```

### 7.3 Error Responses by Role

Error messages adjust based on user role:

| Role | Error Detail Level | Tone | Example |
|------|-------------------|------|---------|
| Caregiver | Technical + actionable | Direct, solution-oriented | "Task cannot be assigned because [Supporter] has declined tasks today. Try another supporter." |
| Organizer | System-level + coordination | Team-focused | "Schedule conflict detected. [Caregiver] is already assigned at this time." |
| Supporter | Minimal technical detail | Gentle, optional | "Unable to accept this task. Please contact the organizer for help." |
| Care Recipient | Non-technical, empathetic | Reassuring, simple | "We couldn't save your request. Your caregiver has been notified." |

## 8. Performance & Scalability

### 8.1 Rate Limiting

API implements rate limiting:

- **Default**: 60 requests per minute per user
- **Burst**: 100 requests per minute for short periods
- **Headers**: `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset`
- **Status Code**: 429 Too Many Requests when limits exceeded

### 8.2 Caching Strategy

```
Cache-Control: private, max-age=300
ETag: "abc123"
```

- GET requests are cacheable with appropriate headers
- PUT/POST/DELETE invalidate related cache entries
- Resource-specific cache expiration policies

### 8.3 Batch Operations

For high-volume operations:

```
POST /batch
{
  "operations": [
    { "method": "GET", "path": "/tasks/123" },
    { "method": "GET", "path": "/tasks/456" },
    { "method": "POST", "path": "/tasks", "body": { ... } }
  ]
}
```

### 8.4 Webhooks

For system integrations and real-time updates:

```
POST /webhooks
{
  "url": "https://example.com/webhook",
  "events": ["task.created", "task.completed"],
  "secret": "webhook_secret"
}
```

## 9. Implementation Guidance

### 9.1 API Development Approach

1. **Implement Core Resources First**:
   - Authentication & User endpoints
   - Care Circle endpoints
   - Task endpoints

2. **Add Role-Based Filtering**:
   - Implement permission checks
   - Add privacy-level filtering
   - Role-specific response adaptation

3. **Implement Secondary Resources**:
   - Event endpoints
   - Update endpoints
   - Health & Medication endpoints

4. **Enhance with Advanced Features**:
   - Batch operations
   - Webhooks
   - Real-time notifications

### 9.2 Testing Requirements

- **Unit Tests**: For all endpoint handlers
- **Integration Tests**: End-to-end API flows
- **Role-Based Tests**: Test each endpoint with different roles
- **Permission Tests**: Verify permission enforcement
- **Performance Tests**: Verify response times under load

### 9.3 Documentation Requirements

- **OpenAPI/Swagger**: Complete API documentation
- **Endpoint Examples**: Request/response examples by role
- **Error Documentation**: All possible error scenarios
- **Integration Guide**: For client developers

## 10. Next Steps

1. Create OpenAPI specification for all endpoints
2. Develop authentication and authorization implementation
3. Implement core API endpoints
4. Create role-based filtering middleware
5. Establish API testing framework

## Change Log

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0.0 | 2025-04-14 | Product Lead | Initial document |
