<!--
Date: April 17, 2025
Purpose: Main README file for CareSupport iOS repository
Contains: Project overview, setup instructions, and documentation references
-->

# CareSupport iOS

## Project Overview

CareSupport is an emotionally intelligent mobile platform designed to transform the caregiving experience by connecting all stakeholders in a care situation. The app serves as a central hub where caregivers, care recipients, organizers, and supporters can coordinate care activities, communicate effectively, and manage tasks in a way that respects each user's role, relationship, and emotional needs.

### Key Features

- **Role-Based Experiences**: Tailored interfaces for Caregivers, Care Recipients, Organizers, and Supporters
- **Emotionally Intelligent Interactions**: Supportive interactions and adaptive interfaces
- **Privacy-Centric Design**: Built-in privacy controls that respect boundaries
- **Care Circle Coordination**: Organized around care circles to facilitate natural relationship structures
- **Health Tracking**: Medication management, symptom tracking, and health data visualization
- **Task Management**: Creation, assignment, and tracking of care-related tasks
- **Communication Hub**: Secure messaging between care circle members

## Getting Started

### Prerequisites

- Xcode 14.3+ 
- iOS 16.0+ target deployment
- Swift 5.8+
- CocoaPods or Swift Package Manager
- Active Apple Developer account for testing on devices

### Setup Instructions

1. Clone the repository:
   ```bash
   git clone https://github.com/CareSupport/CareSupport-iOS.git
   cd CareSupport-iOS
   ```

2. Install dependencies:
   ```bash
   # If using CocoaPods
   pod install
   
   # If using Swift Package Manager
   # Dependencies are resolved automatically when opening the Xcode project
   ```

3. Open the project:
   ```bash
   # If using CocoaPods
   open CareSupport.xcworkspace
   
   # If using Swift Package Manager
   open CareSupport.xcodeproj
   ```

4. Configure development team and signing in Xcode

5. Build and run the project on a simulator or device

### Configuration

The project includes multiple configuration environments:

- `Development`: Points to development API endpoints, includes debugging tools
- `Staging`: Points to staging servers for pre-release testing
- `Production`: Production configuration for App Store releases

To switch environments, select the appropriate scheme in Xcode.

## Project Architecture

CareSupport iOS follows Clean Architecture principles with MVVM presentation layer:

```
┌───────────────────────────────────────────────────────┐
│                    Presentation Layer                 │
│  ┌─────────────┐   ┌─────────────┐   ┌─────────────┐  │
│  │    Views    │◄─►│  ViewModels │◄─►│ Coordinators│  │
│  └─────────────┘   └─────────────┘   └─────────────┘  │
└───────────────────────────┬───────────────────────────┘
                            │
┌───────────────────────────▼───────────────────────────┐
│                     Domain Layer                      │
│  ┌─────────────┐   ┌─────────────┐   ┌─────────────┐  │
│  │   Use Cases │◄─►│   Entities  │◄─►│ Repositories│  │
│  └─────────────┘   └─────────────┘   └─────┬───────┘  │
└───────────────────────────────────────────┬───────────┘
                                            │
┌─────────────────────────────────────────────▼───────────┐
│                     Data Layer                        │
│  ┌─────────────┐   ┌─────────────┐   ┌─────────────┐  │
│  │API Services │   │Local Storage│   │ Data Models │  │
│  └─────────────┘   └─────────────┘   └─────────────┘  │
└───────────────────────────────────────────────────────┘
```

The project is organized into the following modules:

- **Domain**: Contains business logic, entities, and repository interfaces
- **Data**: Implements data access through network and local storage
- **Presentation**: Contains UI components, view models, and coordinators
- **Application**: App-specific configuration, dependency injection, and navigation

## Documentation

Detailed documentation is available in the `/docs` directory:

- [Project Overview](/docs/1_Project_Overview.md)
- [Architecture Overview](/docs/2_Architecture_Overview.md)
- [Key User Flows](/docs/3_Key_User_Flows.md)
- [Technical Specifications](/docs/4_Technical_Specifications.md)
- [Design Assets & Guidelines](/docs/5_Design_Assets_Guidelines.md)
- [Development Roadmap](/docs/6_Development_Roadmap.md)
- [Implementation Guide](/docs/CareSupport_iOS_Implementation_Guide.md)

## Development Workflow

1. Create a feature branch from `develop`
2. Implement the feature following the architecture guidelines
3. Write unit tests for new functionality
4. Submit a pull request for review
5. After approval, the PR will be merged into `develop`
6. Periodically, `develop` will be merged into `main` for releases

## Contact

For questions about this project, please contact:
- Product Owner: [name@caresupport.com]
- Technical Lead: [techlead@caresupport.com]
