# CareSupport Core Platform
**Production-Ready Documentation & Development Guide**

## 1. Introduction

This repository contains the comprehensive documentation for CareSupport's unified core platform development. Following role-specific onboarding and care plan setup, this documentation guides the implementation of the main application experience - the central hub for caregiving coordination, communication, and support.

### 1.1 Project Context

CareSupport is an emotionally intelligent platform that connects caregivers, care recipients, organizers, and supporters in a coordinated care experience. The platform has been designed with:

- Role-specific onboarding flows that establish identity and context
- Care plan setup processes that capture preferences and needs
- A unified, role-adaptive application experience (this development phase)

### 1.2 Development Approach

We're implementing a unified platform approach where:
- All roles share the same component system and layout framework
- Content, functionality, and permissions adapt based on role
- Interactions maintain consistency while respecting role-specific needs

## 2. Repository Structure

```
CareSupport iOS/
├── CareSupport/
│   ├── 00_README.md/
│   │   └── care-support-readme.md
│   ├── 01_Product_Strategy/
│   │   ├── Product_Vision.md
│   │   ├── Experience_Principles.md
│   │   ├── CareSupport_Emotional_Intelligence_Constitution.md
│   │   ├── Phase_One_Completion_Summary.md
│   │   ├── Product_Development_Plan.md
│   │   └── Roadmap.md
│   ├── 02_Research/
│   │   ├── Relationship_Interaction_Model.md
│   │   ├── User_Flows/
│   │   │   ├── Cross_Role_Interactions.md
│   │   │   └── Post_Setup_Journey.md
│   │   └── Legacy_PRDs/
│   │       ├── PRD_Master_Onboarding.md
│   │       └── CareSupport_B2C_PRD.md
│   ├── 03_Information_Architecture/
│   │   ├── Content_Model.md
│   │   ├── Information_Architecture.md
│   │   ├── Navigation_System.md
│   │   └── Permission_Framework.md
│   ├── 04_Onboarding/
│   │   ├── Role_Declaration_README.md
│   │   ├── Onboarding_Engineer_Handoff.md
│   │   ├── Universal_Screens.md
│   │   ├── Caregiver_Flow/
│   │   │   ├── [01]_Define-Relationship.md
│   │   │   ├── [02]_Identify-Recipient.md
│   │   │   ├── [03]_Care-Situation.md
│   │   │   ├── [04]_Select-Primary-Challenge.md
│   │   │   ├── [05]_Quick-Win-Action.md
│   │   │   ├── [06]_Invite-Care-Recipient.md
│   │   │   ├── [07]_Add-to-Care-Circle.md
│   │   │   ├── [08]_Privacy-and-Permissions-Overview.md
│   │   │   └── [09]_Emotional-Framing.md
│   │   ├── Care_Recipient_Flow/
│   │   │   ├── [01]_Confirm-Invite.md
│   │   │   ├── [02]_Privacy-Preferences.md
│   │   │   ├── [03]_Express-Needs.md
│   │   │   ├── [04]_Todays-Plan-Preview.md
│   │   │   ├── [05]_Consent-Confirmation.md
│   │   │   ├── [06]_Emotional-Framing.md
│   │   │   ├── [06]_Emotional-Reinforcement.md
│   │   │   └── CareRecipient-FinalMessage.md
│   │   ├── Organizer_Flow/
│   │   │   ├── [06]_Emotional-Framing_Organizer.md
│   │   │   └── Organizer-FinalMessage.md
│   │   └── Supporter_Flow/
│   │       ├── [05]_Emotional-Framing_Supporter.md
│   │       └── Supporter-FinalMessage.md
│   ├── 05_Care_Plan_Setup/
│   │   ├── [00]_Welcome-Overlay.md
│   │   ├── Care_Plan_Setup_Dashboard_Reminders.md
│   │   ├── Care_Recipient/
│   │   │   ├── [01]_CarePlan-Welcome.md
│   │   │   ├── [02]_Ideal-Day.md
│   │   │   ├── [03]_Communication-Style.md
│   │   │   ├── [04]_Help-Preferences.md
│   │   │   ├── [05]_Journal-Introduction.md
│   │   │   └── [06]_CarePlan-Complete.md
│   │   ├── Caregiver/
│   │   │   ├── [01]_Care-Plan-Welcome.md
│   │   │   ├── [02]_Starting-Point.md
│   │   │   ├── [03]_Quick-Win.md
│   │   │   ├── [04]_Daily-Rhythm.md
│   │   │   ├── [05]_Circle-Involvement.md
│   │   │   └── [06]_CarePlan-Complete.md
│   │   ├── Organizer/
│   │   │   ├── [01]_CarePlan-Welcome_Organizer_Final.md
│   │   │   ├── [02]_Care-Team-Overview.md
│   │   │   ├── [03]_Task-Assignment.md
│   │   │   ├── [04]_Master-Schedule.md
│   │   │   ├── [05]_Communication-Protocol.md
│   │   │   └── [06]_CarePlan-Complete.md
│   │   └── Supporter/
│   │       ├── [01] welcome-circle.md
│   │       ├── [02] help.ways.md
│   │       ├── [03]_Availability.md
│   │       └── [04]_Communication-Complete.md
│   ├── 06_Core_Platform/
│   │   ├── Care_Coordination/
│   │   │   └── care-coordination-center-spec.md
│   │   ├── Communication_Hub/
│   │   │   └── communication-hub-specification.md
│   │   ├── Feature_Specifications/
│   │   ├── Integration/
│   │   │   ├── comprehensive-testing-spec.md
│   │   │   ├── Cross_Role_Integration.md
│   │   │   ├── Performance_Optimization.md
│   │   │   └── Personalization_System.md
│   │   ├── Post_Setup_Transition/
│   │   │   └── Welcome_Home_Specification.md
│   │   ├── Role_Specific/
│   │   │   ├── Care_Recipient/
│   │   │   │   └── care-recipient-experience-spec.md
│   │   │   ├── Caregiver/
│   │   │   │   └── caregiver-experience-spec.md
│   │   │   ├── Organizer/
│   │   │   │   └── organizer-experience-spec.md
│   │   │   └── Supporter/
│   │   │       └── supporter-experience-spec.md
│   │   ├── Task_Management/
│   │   │   └── task-management-system-specification.md
│   │   └── Welcome_Experience/
│   │       └── welcome-experience-specification.md
│   ├── 07_Technical/
│   │   ├── API_Requirements/
│   │   │   └── api-requirements-documentation.md
│   │   ├── Data_Models/
│   │   │   └── data-model-extensions.md
│   │   └── Implementation_Guides/
│   │       ├── role-based-permission-framework.md
│   │       └── state-management-system.md
│   └── 08_Design_Standards/
│       ├── Component_Library/
│       │   └── core-component-system.md
│       └── Style_Guide/
```

## 3. Documentation Standards

### 3.1 Versioning System

All documents follow semantic versioning:
- **Major version (X.0.0)**: Significant changes that affect implementation
- **Minor version (0.X.0)**: Feature additions or modifications
- **Patch version (0.0.X)**: Bug fixes, clarifications, or minor updates

Example: `Version: 1.2.3 | Last Updated: 2025-04-14`

### 3.2 Change Log Standards

Each document contains a change log at the bottom:

```
## Change Log

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0.0   | 2025-04-14 | A. Designer | Initial document |
| 1.0.1   | 2025-04-15 | A. Designer | Fixed typo in requirements |
| 1.1.0   | 2025-04-20 | A. Designer | Added support for voice input |
```

### 3.3 Priority Framework

Feature requirements use the MoSCoW system with visual indicators:

- **🔴 MUST**: Critical for launch; non-negotiable requirements
- **🟠 SHOULD**: Important but not critical; strong business case
- **🟡 COULD**: Desirable if resources permit; enhances experience
- **⚪ WON'T**: Not planned for current scope; documented for future

Example:
```
## Requirements

| ID | Requirement | Priority | Notes |
|----|-------------|----------|-------|
| TASK-01 | System shall enable task creation and assignment | 🔴 MUST | Core functionality |
| TASK-02 | System shall support recurring tasks | 🟠 SHOULD | Important for medication |
| TASK-03 | System shall enable batch task creation | 🟡 COULD | Efficiency enhancement |
| TASK-04 | System shall support task delegation chains | ⚪ WON'T | Future consideration |
```

## 4. Document Templates

### 4.1 Feature Specification Template

```markdown
# [Feature Name] Specification

**Version:** 1.0.0  
**Last Updated:** YYYY-MM-DD  
**Status:** [Draft/In Review/Approved/Implemented]

## 1. Overview

[Brief description of the feature and its purpose]

### 1.1 Related Documents

- [Links to related specifications]

### 1.2 Feature Goals

- [Primary goals this feature aims to achieve]

## 2. User Stories

| As a... | I want to... | So that... | Priority |
|---------|-------------|------------|----------|
| Caregiver | Assign tasks to supporters | I can distribute the workload | 🔴 MUST |
| [Additional user stories] | | | |

## 3. Functional Requirements

### 3.1 Core Functionality

| ID | Requirement | Priority | Notes |
|----|-------------|----------|-------|
| [FEAT]-01 | [Requirement description] | 🔴 MUST | [Implementation notes] |
| [Additional requirements] | | | |

### 3.2 Role-Specific Adaptations

| Role | View/Access | Interactions | Permissions |
|------|-------------|--------------|-------------|
| Caregiver | [What they see] | [What they can do] | [Permission level] |
| [Other roles] | | | |

## 4. UI Components

[Key UI components with descriptions and links to designs]

### 4.1 States & Behaviors

[Different states, interactions, and behaviors]

## 5. Data Requirements

### 5.1 Data Objects

[Data objects, fields, and relationships]

### 5.2 API Requirements

[Endpoints, methods, and parameters]

## 6. Edge Cases & Error States

| Scenario | System Behavior | User Feedback |
|----------|----------------|---------------|
| [Edge case description] | [System response] | [What user sees] |
| [Additional edge cases] | | |

## 7. Accessibility Considerations

[Key accessibility requirements and implementations]

## 8. Implementation Notes

[Technical guidance, dependencies, and phasing]

## Change Log

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0.0 | YYYY-MM-DD | [Author] | Initial document |
```

### 4.2 Component Documentation Template

```markdown
# [Component Name] Documentation

**Version:** 1.0.0  
**Last Updated:** YYYY-MM-DD  
**Status:** [Draft/In Review/Approved/Implemented]

## 1. Component Purpose

[Brief description of the component's function and use cases]

## 2. Visual Reference

[Link to Figma design or embedded image]

## 3. Variants & States

| Variant | Purpose | Visual Reference |
|---------|---------|-----------------|
| [Variant name] | [When to use] | [Link/image] |
| [Additional variants] | | |

## 4. Role-Based Adaptations

| Role | Appearance | Behavior | Permissions |
|------|------------|----------|-------------|
| Caregiver | [How it looks] | [How it behaves] | [What they can do] |
| [Other roles] | | | |

## 5. Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| [Property name] | [Data type] | [Default value] | [Description] |
| [Additional properties] | | | |

## 6. Accessibility Requirements

[WCAG compliance, keyboard navigation, screen reader support, etc.]

## 7. Implementation Guidance

[Technical notes, dependencies, example usage]

## Change Log

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0.0 | YYYY-MM-DD | [Author] | Initial document |
```

### 4.3 User Flow Documentation Template

```markdown
# [Flow Name] User Flow

**Version:** 1.0.0  
**Last Updated:** YYYY-MM-DD  
**Status:** [Draft/In Review/Approved/Implemented]

## 1. Flow Overview

[Brief description of the user flow and its purpose]

### 1.1 Entry Points

| Entry Point | Context | Initial State |
|-------------|---------|--------------|
| [Entry point description] | [When/where this occurs] | [What user sees first] |
| [Additional entry points] | | |

## 2. Flow Steps

### Step 1: [Step Name]

**Screen/Component:** [Screen or component name]  
**User Action:** [What user does]  
**System Response:** [What system does]  
**Next Step:** [Where flow goes next]  
**Alternative Paths:** [Branches or variations]

[Repeat for additional steps]

## 3. Decision Points

| Decision | Options | Result |
|----------|---------|--------|
| [Decision description] | [Available choices] | [Outcome for each choice] |
| [Additional decisions] | | |

## 4. Success & Error States

| State | Trigger | System Behavior | User Feedback |
|-------|---------|----------------|---------------|
| Success | [What causes success] | [System response] | [What user sees] |
| Error | [What causes error] | [System handling] | [What user sees] |
| [Additional states] | | | |

## 5. Role-Specific Variations

| Role | Flow Differences | Rationale |
|------|-----------------|-----------|
| Caregiver | [How flow differs] | [Why it's different] |
| [Other roles] | | |

## 6. Edge Cases

[Unusual scenarios and how they're handled]

## Change Log

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0.0 | YYYY-MM-DD | [Author] | Initial document |
```

## 5. Core Terminology Glossary

This terminology builds on the existing `terminology.md` document, focusing on platform-specific terms.

| Term | Definition | Usage Context |
|------|------------|---------------|
| **Care Circle** | The complete group of people providing support to a care recipient | Used system-wide to describe the care network |
| **Care Plan** | The structured set of preferences, needs, and arrangements established during setup | Referenced when discussing personalization |
| **Quick Win** | A task completed early in the experience to build momentum and confidence | Used in onboarding and initial engagement |
| **Task** | A discrete care activity that can be assigned, tracked, and completed | Core object in the coordination system |
| **Journal** | Personal or shared record of observations, reflections, and care notes | Feature for documentation and reflection |
| **Care Recipient** | The person receiving care | Used instead of "patient" or other clinical terms |
| **Caregiver** | Primary person coordinating or providing hands-on care | Distinguished from other supporting roles |
| **Organizer** | Person who manages logistics and coordination | Focuses on system and schedule |
| **Supporter** | Person providing occasional or specific help | Limited, focused engagement |
| **Daily Rhythm** | The general pattern of a care recipient's day | Used for scheduling and expectations |
| **Care Situation** | The overall health context and care needs | Provides framework for care approach |
| **Check-in** | Brief status update or wellness verification | Used for monitoring and connection |
| **Primary Challenge** | Most pressing caregiving difficulty identified during setup | Drives personalization and recommendations |
| **Privacy Preference** | Care recipient's choices about information sharing | Controls visibility and permissions |
| **Notification** | System alert about important care events or updates | Communication mechanism |
| **Dashboard** | Main view that presents relevant care information and actions | Primary interface for all roles |

## 6. Getting Started

### 6.1 Development Priorities

1. **Post-Setup Transition Experience** - Critical handoff from setup to active usage
2. **Core Navigation Framework** - Unified system that adapts to roles
3. **Shared Component System** - Adaptive components with role states
4. **Task Management System** - Foundation for care coordination
5. **Communication Hub** - Connected messaging and updates
6. **Care Recipient Status** - Health tracking and monitoring

### 6.2 Key Development Principles

1. **Role-Adaptive, Not Role-Specific** - Build one system that adapts, not separate systems
2. **Emotional Intelligence** - Maintain EI principles from the Constitution document
3. **Progressive Disclosure** - Show what's needed when it's needed
4. **Accessibility First** - Follow standards from `accessibility.md`
5. **Data Continuity** - Leverage data from onboarding and setup phases

## 7. Change Log

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0.0 | 2025-04-14 | Product Lead | Initial document |
