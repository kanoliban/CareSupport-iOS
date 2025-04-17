<!--
Date: April 17, 2025
Purpose: README for Domain layer implementation in CareSupport iOS app
Contains: Overview, organization, and guidelines for the Domain layer
-->

# Domain Layer

## Overview

The Domain layer represents the core business logic of the CareSupport application. It contains business entities, use cases, and repository interfaces, but no implementation details. This layer is completely independent of any external frameworks or technologies, ensuring business rules are isolated from UI and data concerns.

## Directory Structure

```
Domain/
├── Entities/            # Business models that represent core concepts
├── Interfaces/          # Repository and service interfaces
├── UseCases/            # Application business rules
└── Errors/              # Domain-specific error types
```

## Key Components

### Entities

Domain entities represent the core business objects. They are simple, framework-independent Swift structs or classes:

```swift
struct User {
    let id: String
    let firstName: String
    let lastName: String
    let email: String
    let roles: [UserRole]
    // other properties
}

enum UserRole {
    case caregiver
    case careRecipient
    case organizer
    case supporter
}
```

### Repository Interfaces

Repositories define the data access contracts without implementation details:

```swift
protocol UserRepository {
    func getUser(id: String) async throws -> User
    func updateUser(_ user: User) async throws
    func getRoleContext(for user: User, role: UserRole) async throws -> RoleContext
    // other methods
}
```

### Use Cases

Use cases encapsulate business logic for a specific action or flow:

```swift
protocol GetUserProfileUseCase {
    func execute(userId: String) async throws -> User
}

class GetUserProfileUseCaseImpl: GetUserProfileUseCase {
    private let userRepository: UserRepository
    
    init(userRepository: UserRepository) {
        self.userRepository = userRepository
    }
    
    func execute(userId: String) async throws -> User {
        return try await userRepository.getUser(id: userId)
    }
}
```

## Guidelines

- Entities should be immutable when possible
- No UIKit, SwiftUI, or other framework imports in this layer
- No direct dependencies on data sources (network, database)
- Use protocol-based abstractions for all external dependencies
- All use cases should have a corresponding protocol
- Validate domain rules at this layer
- Keep entities focused on business concepts, not data transfer
