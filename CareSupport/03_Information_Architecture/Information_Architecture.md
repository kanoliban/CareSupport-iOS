# Information Architecture & Navigation System

**Version:** 1.0.0  
**Last Updated:** 2025-04-14  
**Status:** Draft  
**Document Location:** `/03_Information_Architecture/Navigation_System.md`

## 1. Overview

This document defines the core navigation framework and information architecture for CareSupport's unified platform. It establishes how users navigate through the application, how information is organized, and how the interface adapts based on role while maintaining a consistent structure.

### 1.1 Related Documents

- `/02_Research/Relationship_Model.md`
- `/06_Core_Platform/Post_Setup_Transition/Welcome_Home_Specification.md`
- `/08_Design_Standards/accessibility.md`
- `/01_Product_Strategy/CareSupport_Emotional_Intelligence_Constitution.md`

### 1.2 Purpose & Goals

- Establish a consistent navigation framework across all roles
- Define how information is organized and accessed
- Create patterns for role-based content adaptation
- Ensure intuitive wayfinding and information discovery
- Support emotional intelligence principles through information presentation

## 2. Navigation Framework

### 2.1 Primary Navigation System

CareSupport uses a **tab-based primary navigation** with role-adaptive content within each section.

#### 2.1.1 Core Navigation Tabs

| Tab | Label | Icon | Primary Purpose | Available to |
|-----|-------|------|-----------------|--------------|
| Home | "Home" | Home icon | Role-specific dashboard overview | All roles |
| Care | "Care" | Heart icon | Tasks, schedule, medication management | All roles |
| Connect | "Connect" | Message icon | Updates, messages, care circle | All roles |
| Profile | "Profile" | Person icon | Settings, preferences, help | All roles |

#### 2.1.2 Tab Content Adaptation

While the tabs remain consistent, their content adapts based on role:

| Tab | Caregiver View | Organizer View | Supporter View | Care Recipient View |
|-----|---------------|----------------|----------------|---------------------|
| Home | Care recipient status, Priority tasks, Quick actions | Team overview, Task status, Schedule conflicts | Available opportunities, Upcoming commitments | Today's plan, Upcoming events, Express needs |
| Care | Full care management, Task creation, Med tracking | Team task view, Assignment tools, Schedule management | Task acceptance, Completion tracking | Care schedule, Simple needs requests, Privacy controls |
| Connect | Full updates, Status sharing, Direct messaging | Team coordination, Group messages, Status overview | Limited updates, Task-related messages | Update feed, Direct messages, Privacy settings |
| Profile | Full account management, Preferences, Resources | Team settings, Role management, Contact list | Simple profile, Availability settings | Enhanced privacy controls, Personal journal, Support options |

### 2.2 Secondary Navigation

Each primary tab contains context-specific secondary navigation:

#### 2.2.1 Home Tab Navigation

| Section | Purpose | Available to |
|---------|---------|--------------|
| Overview | Quick status summary | All roles |
| Today | Today's plan and activities | All roles |
| Insights | Care patterns and observations | Caregiver, Organizer |
| Quick Wins | Suggested actions | All roles (content varies) |

#### 2.2.2 Care Tab Navigation

| Section | Purpose | Available to |
|---------|---------|--------------|
| Tasks | Task list and management | All roles (permissions vary) |
| Schedule | Calendar and appointments | All roles (detail varies) |
| Medications | Medication tracking | Caregiver, Care Recipient |
| Health | Health tracking and vitals | Caregiver, Care Recipient |

#### 2.2.3 Connect Tab Navigation

| Section | Purpose | Available to |
|---------|---------|--------------|
| Updates | Care status and notes | All roles (content varies) |
| Messages | Direct communication | All roles |
| Care Circle | Team view and management | All roles (actions vary) |
| Resources | Knowledge and support | All roles |

#### 2.2.4 Profile Tab Navigation

| Section | Purpose | Available to |
|---------|---------|--------------|
| My Account | Core profile settings | All roles |
| Preferences | System preferences | All roles |
| Privacy | Privacy settings | All roles (enhanced for Care Recipient) |
| Help & Support | Support resources | All roles |

### 2.3 Context-Specific Navigation

Beyond standard navigation, context-specific elements appear based on state:

| Element | Appears When | Purpose | Behavior |
|---------|--------------|---------|----------|
| Quick Action Button | On most screens | Context-specific actions | Floating action button with most relevant action |
| Notification Panel | When notifications exist | Show alerts & updates | Swipe down or tap notification icon |
| Role Switcher | For dual-role users | Toggle between roles | Appears in Profile & persistent dropdown |
| Setup Reminder | Setup incomplete | Guide to completion | Banner at top of relevant sections |
| Context Menu | On content items | Item-specific actions | Three-dot menu or long press |

## 3. Information Organization

### 3.1 Content Modules

Content is organized into discrete modules that maintain consistent structure but adapt content by role:

#### 3.1.1 Core Modules

| Module | Purpose | Role Adaptation |
|--------|---------|-----------------|
| Care Status Card | Shows care recipient wellbeing | Detail level varies by role |
| Task Module | Displays and manages tasks | Permissions and view vary by role |
| Schedule Module | Calendar and time management | Displays role-appropriate events |
| Care Circle Module | Team view and communication | Shows role-appropriate team info |
| Medication Module | Medication tracking | Limited or full based on role |
| Updates Module | Status and communication | Content filtered by role |

#### 3.1.2 Module States

Each module has multiple states:

| State | Description | Behavior |
|-------|-------------|----------|
| Collapsed | Summary view only | Shows top-level information, tap to expand |
| Expanded | Full content visible | Shows complete information and actions |
| Empty | No content available | Shows helpful prompts for next steps |
| Error | Unable to load | Shows friendly error and recovery option |
| Loading | Content fetching | Shows skeleton screen during loading |
| Highlight | Important content | Visual emphasis for priority items |

### 3.2 Information Hierarchy

Information is organized in a consistent hierarchy across the platform:

#### 3.2.1 Visual Hierarchy

1. **Primary Focus** (largest, most prominent)
   - Critical status information
   - Time-sensitive actions
   - Key metrics and status

2. **Contextual Information** (medium emphasis)
   - Supporting details
   - Secondary metrics
   - Related information

3. **Reference Information** (lowest emphasis)
   - Historical data
   - Additional options
   - Supplementary content

#### 3.2.2 Content Prioritization by Role

| Role | Primary Focus | Secondary Focus | Tertiary Focus |
|------|--------------|-----------------|----------------|
| Caregiver | Care recipient status, Critical tasks | Medication schedule, Updates | Resources, History |
| Organizer | Team coordination, Task status | Schedule conflicts, Task assignment | Resource allocation, Reports |
| Supporter | Available opportunities, My tasks | Upcoming commitments, Messages | Care circle updates, Resources |
| Care Recipient | Today's plan, Immediate needs | Upcoming visits, Messages | Privacy controls, Journal |

### 3.3 Navigation Patterns

#### 3.3.1 Wayfinding System

| Pattern | Implementation | Purpose |
|---------|----------------|---------|
| Breadcrumbs | Small text path at top | Show location in nested sections |
| Section Headers | Large text headings | Clearly label current section |
| Tab Highlighting | Active tab visual indicator | Show current primary section |
| Back Navigation | Left-pointing arrow | Return to previous screen |
| Search | Search icon and field | Find content across sections |

#### 3.3.2 Progressive Disclosure

Information is revealed progressively to reduce cognitive load:

| Level | What's Shown | When |
|-------|-------------|------|
| Level 1 | Critical information only | Default view |
| Level 2 | Supporting details | After user interaction |
| Level 3 | Full details and options | After deliberate expansion |
| Level 4 | Advanced features | Through explicit menu selection |

## 4. Role-Adaptive Interfaces

### 4.1 Shared Component Framework

All roles use the same component framework with these adaptive behaviors:

#### 4.1.1 Component Adaptation Methods

| Method | Description | Example |
|--------|-------------|---------|
| Content Filtering | Same structure, different content | Task list shows only assigned tasks for Supporters |
| Permission Controls | Visible but disabled actions | Edit button visible but disabled for Supporters |
| Progressive Disclosure | Role-appropriate level of detail | Care Recipient sees simplified health metrics |
| Contextual Emphasis | Different priorities highlighted | Organizer sees coordination conflicts highlighted |
| Tone Adaptation | Language adjusts to role | Caregiver sees supportive, empathetic language |

#### 4.1.2 Key Adaptive Components

| Component | Base Structure | Adaptation Method |
|-----------|----------------|-------------------|
| Dashboard | Card-based overview | Content filtering + contextual emphasis |
| Task List | Sortable, filterable list | Permission controls + content filtering |
| Calendar | Day/week/month views | Content filtering + permission controls |
| Care Circle | Team member cards | Content filtering + progressive disclosure |
| Updates Feed | Chronological posts | Content filtering + tone adaptation |
| Care Status | Metric summary cards | Progressive disclosure + tone adaptation |

### 4.2 Role-Specific Views

#### 4.2.1 Caregiver Interface Focus

- **Primary Dashboard:** Care Recipient status, medication schedule, priority tasks
- **Key Modules:** Full task management, medication tracking, health status
- **Unique Features:** Care recipient diary, health history, resource library
- **Navigation Emphasis:** Quick care actions, status monitoring, communication

#### 4.2.2 Care Recipient Interface Focus

- **Primary Dashboard:** Today's plan, upcoming visits, easy help requests
- **Key Modules:** Simplified schedule, needs expression, privacy settings
- **Unique Features:** Express needs tool, journey diary, private notes
- **Navigation Emphasis:** Autonomy controls, communication, personal well-being

#### 4.2.3 Organizer Interface Focus

- **Primary Dashboard:** Team overview, task status, schedule conflicts
- **Key Modules:** Task assignment matrix, team schedule, resource allocation
- **Unique Features:** Team status board, conflict detection, coordination tools
- **Navigation Emphasis:** Team management, schedule coordination, task distribution

#### 4.2.4 Supporter Interface Focus

- **Primary Dashboard:** Available opportunities, my commitments, availability toggle
- **Key Modules:** My tasks, upcoming schedule, simple updates
- **Unique Features:** Availability calendar, contribution history, skill preferences
- **Navigation Emphasis:** Task management, availability settings, simple communication

### 4.3 Multi-Role User Experience

For users with multiple roles (e.g., Caregiver + Organizer):

- **Role Switcher:** Prominent toggle in header and profile
- **Context Preservation:** When switching roles, stay in same section when possible
- **Visual Distinction:** Clear visual indicators of current active role
- **Notification Handling:** Combined notifications with role indicators
- **Settings Separation:** Role-specific preferences maintained separately

## 5. User Flows

### 5.1 Core Navigation Flows

#### 5.1.1.First-Time Navigation Flow

1. Post-onboarding welcome overlay presents key sections
2. Initial focus on Home dashboard with simplified navigation
3. Guided tooltip tour highlights key navigation elements
4. Progressive introduction to additional features after basic interaction
5. "Getting Started" card remains on dashboard until dismissed

#### A5.1.2. Daily User Flow

1. Opens app to role-specific Home dashboard
2. Priority items displayed immediately in focus area
3. Quick access to frequently used sections through recents or favorites
4. Context-switching provisions for multi-role users
5. Session state preserved when returning to app

### 5.2 Content Discovery Flows

#### 5.2.1. Information Finding Flow

1. Primary path: Navigate through tab hierarchy
2. Secondary path: Use search with role-filtered results
3. Tertiary path: Follow related content links between modules
4. Advanced: Saved favorites and recent activity history

#### 5.2.2. New Content Discovery Flow

1. Notification indicator on relevant tab
2. New content markers within sections
3. Dashboard highlights for important new information
4. "What's New" section in Profile tab

### 5.3 Task-Based Flows

#### 5.3.1. Task Creation Flow (Caregiver/Organizer)

1. Quick add from floating action button
2. Expanded creation from Tasks section
3. Context-specific creation from related content
4. Bulk creation from Tasks management screen

#### 5.3.2. Task Acceptance Flow (Supporter)

1. Notification of new task opportunity
2. Review task details screen
3. Accept/decline with optional message
4. Confirmation and addition to "My Tasks"

## 6. Accessibility Considerations

### 6.1 Navigation Accessibility

- **Keyboard Navigation:** Full keyboard support with logical tab order
- **Screen Reader Support:** ARIA labels and roles for all navigation elements
- **Touch Targets:** Minimum 44x44px for all interactive elements
- **Focus States:** Clear visual indicators of focus location
- **Skip Navigation:** Option to bypass repeated navigation elements
- **Voice Navigation:** Support for system voice control

### 6.2 Information Access Accessibility

- **Text Scaling:** All content supports dynamic text sizing
- **Color Independence:** Information conveyed by more than just color
- **Contrast Ratios:** Minimum 4.5:1 contrast for all text
- **Landmark Regions:** Proper HTML5 semantic structure
- **Reading Order:** Logical content flow for screen readers
- **Reduced Motion:** Respects system setting for animations

## 7. Implementation Guidelines

### 7.1 Technical Approach

- **Component System:** Build role-adaptive UI components in a shared library
- **State Management:** Implement role-based state management with permission checks
- **Navigation Framework:** Use consistent router with role-aware guards
- **Responsive Design:** Design for all iOS device sizes
- **Offline Support:** Core navigation functions offline with graceful data syncing

### 7.2 Performance Considerations

- **Navigation Speed:** Tab switches complete in <100ms
- **Memory Usage:** Smart module loading to minimize memory footprint
- **Transition Smoothness:** 60fps minimum for all navigation animations
- **Initial Load:** Critical path optimization for first meaningful paint
- **Progressive Enhancement:** Core functionality available before full load

## 8. Next Steps

1. Create detailed specifications for each core module
2. Develop component adaptation patterns for the design system
3. Build navigation prototypes for user testing
4. Map detailed permissions to UI states

## Change Log

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0.0 | 2025-04-14 | Product Lead | Initial document |
