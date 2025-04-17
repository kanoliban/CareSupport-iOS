<!--
Date: April 17, 2025
Purpose: Architecture overview diagram for CareSupport iOS app
Contains: App structure, data flow, and role-based adaptation mechanisms
Created by: Cascade AI
-->

# CareSupport iOS: Architecture Overview

**Document Version:** 1.0.0  
**Last Updated:** 2025-04-17  

## Application Architecture

CareSupport iOS follows Clean Architecture principles with MVVM presentation layer. This diagram illustrates the high-level architecture and core components:

```
┌─────────────────────────────────────────────────────────────────────┐
│                         PRESENTATION LAYER                          │
│                                                                     │
│  ┌─────────────┐       ┌─────────────┐       ┌─────────────┐       │
│  │             │       │             │       │             │       │
│  │    Views    │◄────► │ ViewModels  │◄────► │ Coordinators│       │
│  │             │       │             │       │             │       │
│  └─────────────┘       └─────────────┘       └─────────────┘       │
│         ▲                     ▲                     ▲               │
└─────────┼─────────────────────┼─────────────────────┼───────────────┘
          │                     │                     │
┌─────────┼─────────────────────┼─────────────────────┼───────────────┐
│         │                     │                     │               │
│         │               DOMAIN LAYER                │               │
│         │                                                           │
│  ┌─────────────┐       ┌─────────────┐       ┌─────────────┐       │
│  │             │       │             │       │             │       │
│  │  Use Cases  │◄────► │  Entities   │◄────► │ Repositories│       │
│  │             │       │             │       │   (Intf)    │       │
│  └─────────────┘       └─────────────┘       └──────┬──────┘       │
│                                                     │               │
└─────────────────────────────────────────────────────┼───────────────┘
                                                      │
┌─────────────────────────────────────────────────────┼───────────────┐
│                                                     │               │
│                          DATA LAYER                 ▼               │
│                                                                     │
│  ┌─────────────┐       ┌─────────────┐       ┌─────────────┐       │
│  │             │       │             │       │             │       │
│  │Remote Source│◄────► │ Local Source│◄────► │ Repositories│       │
│  │  (API)      │       │ (CoreData)  │       │   (Impl)    │       │
│  └─────────────┘       └─────────────┘       └─────────────┘       │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

## Core Modules

The application is organized into the following modules:

```
┌────────────────┐ ┌────────────────┐ ┌────────────────┐ ┌────────────────┐
│                │ │                │ │                │ │                │
│ Authentication │ │  Onboarding    │ │   Care Plan    │ │  Care Circle   │
│                │ │                │ │                │ │                │
└────────────────┘ └────────────────┘ └────────────────┘ └────────────────┘
        │                  │                  │                  │
        └──────────────────┴──────────────────┴──────────────────┘
                                    │
                                    ▼
┌────────────────┐ ┌────────────────┐ ┌────────────────┐ ┌────────────────┐
│                │ │                │ │                │ │                │
│     Tasks      │ │ Communication  │ │ Health Tracking│ │   Calendar     │
│                │ │                │ │                │ │                │
└────────────────┘ └────────────────┘ └────────────────┘ └────────────────┘
        │                  │                  │                  │
        └──────────────────┴──────────────────┴──────────────────┘
                                    │
                                    ▼
┌────────────────┐ ┌────────────────┐ ┌────────────────┐
│                │ │                │ │                │
│   Settings     │ │   Reporting    │ │ Notifications  │
│                │ │                │ │                │
└────────────────┘ └────────────────┘ └────────────────┘
```

## Data Flow Architecture

This diagram illustrates how data flows through the system:

```
┌──────────────────┐     ┌───────────────┐     ┌───────────────────┐
│                  │     │               │     │                   │
│  REST API Server │◄───►│ API Service   │◄───►│  Repository       │
│                  │     │               │     │                   │
└──────────────────┘     └───────────────┘     └─────────┬─────────┘
                                                         │
                                                         ▼
┌──────────────────┐     ┌───────────────┐     ┌───────────────────┐
│                  │     │               │     │                   │
│    CoreData      │◄───►│ Local Storage │◄───►│  Domain Entities  │
│                  │     │               │     │                   │
└──────────────────┘     └───────────────┘     └─────────┬─────────┘
                                                         │
                                                         ▼
┌──────────────────┐     ┌───────────────┐     ┌───────────────────┐
│                  │     │               │     │                   │
│    Use Cases     │◄───►│  ViewModels   │◄───►│     Views         │
│                  │     │               │     │                   │
└──────────────────┘     └───────────────┘     └───────────────────┘
```

## Role-Based Adaptation System

The user experience adapts based on the user's active role through a layered context system:

```
┌──────────────────────────────────────────────────────────────────────┐
│                          USER SESSION                                │
│                                                                      │
│  ┌─────────────────┐   ┌─────────────────┐   ┌─────────────────┐    │
│  │ Authentication  │   │ User Profile    │   │ Care Circle     │    │
│  │ Context         │   │ Context         │   │ Context         │    │
│  └─────────────────┘   └─────────────────┘   └─────────────────┘    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
┌──────────────────────────────────────────────────────────────────────┐
│                         ROLE CONTEXT                                 │
│                                                                      │
│   ┌─────────────┐   ┌─────────────┐   ┌─────────────┐   ┌─────────┐ │
│   │ Caregiver   │   │ Care        │   │ Organizer   │   │Supporter│ │
│   │ Context     │   │ Recipient   │   │ Context     │   │Context  │ │
│   └─────────────┘   └─────────────┘   └─────────────┘   └─────────┘ │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
┌──────────────────────────────────────────────────────────────────────┐
│                      FEATURE ADAPTATION                              │
│                                                                      │
│  ┌─────────────────┐   ┌─────────────────┐   ┌─────────────────┐    │
│  │ UI Components   │   │ Permissions     │   │ Content         │    │
│  │ Adaptation      │   │ Adaptation      │   │ Adaptation      │    │
│  └─────────────────┘   └─────────────────┘   └─────────────────┘    │
│                                                                      │
└──────────────────────────────────────────────────────────────────────┘
```

## Permission Framework Architecture

The permission system uses a layered approach to determine available actions:

```
┌─────────────────────────────────────────────────────────────────┐
│                    PERMISSION LAYERS                            │
│                   (Highest to Lowest)                           │
│                                                                 │
│  ┌─────────────────┐                                            │
│  │ User Overrides  │  Care Recipient explicit sharing controls  │
│  └─────────────────┘                                            │
│            │                                                    │
│            ▼                                                    │
│  ┌─────────────────┐                                            │
│  │ Object-Level    │  Permissions on specific data objects      │
│  └─────────────────┘                                            │
│            │                                                    │
│            ▼                                                    │
│  ┌─────────────────┐                                            │
│  │ Care Circle     │  Permissions specific to a care circle     │
│  └─────────────────┘                                            │
│            │                                                    │
│            ▼                                                    │
│  ┌─────────────────┐                                            │
│  │ Relationship    │  Based on relationship to Care Recipient   │
│  └─────────────────┘                                            │
│            │                                                    │
│            ▼                                                    │
│  ┌─────────────────┐                                            │
│  │ Role-Level      │  Default permissions for each role         │
│  └─────────────────┘                                            │
│            │                                                    │
│            ▼                                                    │
│  ┌─────────────────┐                                            │
│  │ System-Level    │  Core permissions enforced globally        │
│  └─────────────────┘                                            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

## State Management Architecture

The application state is managed through a combination of:

```
┌───────────────────────────────────────────────────────────────┐
│                    STATE MANAGEMENT                           │
│                                                               │
│  ┌─────────────┐   ┌─────────────┐   ┌─────────────┐         │
│  │  Combine    │   │  SwiftUI    │   │ UserDefaults│         │
│  │ Framework   │   │  State      │   │ & CoreData  │         │
│  └─────────────┘   └─────────────┘   └─────────────┘         │
│                                                               │
│  ┌─────────────────────────────────────────────────────────┐  │
│  │                                                         │  │
│  │  ┌─────────────┐   ┌─────────────┐   ┌─────────────┐   │  │
│  │  │ Application │   │    Role     │   │   View      │   │  │
│  │  │    State    │   │   Context   │   │   State     │   │  │
│  │  └─────────────┘   └─────────────┘   └─────────────┘   │  │
│  │                                                         │  │
│  │  ┌─────────────┐   ┌─────────────┐                     │  │
│  │  │   Entity    │   │   Session   │                     │  │
│  │  │    State    │   │    State    │                     │  │
│  │  └─────────────┘   └─────────────┘                     │  │
│  │                                                         │  │
│  └─────────────────────────────────────────────────────────┘  │
│                                                               │
└───────────────────────────────────────────────────────────────┘
```

## Key Implementation Patterns

- **Dependency Injection** for service and repository access
- **Coordinator Pattern** for navigation management
- **Repository Pattern** for data access abstraction
- **Factory Pattern** for view and component creation
- **Adapter Pattern** for role-based UI adaptation
- **Observer Pattern** via Combine for reactive state updates
- **Command Pattern** for undoable actions and history

## Technology Stack Recommendations

- **UI Framework**: SwiftUI with UIKit integration where needed
- **Reactive Programming**: Combine
- **Persistence**: CoreData + CloudKit
- **Networking**: URLSession with async/await
- **Authentication**: OAuth2 with JWT token handling
- **Analytics**: Firebase Analytics
- **Crash Reporting**: Firebase Crashlytics
- **Dependency Management**: Swift Package Manager
