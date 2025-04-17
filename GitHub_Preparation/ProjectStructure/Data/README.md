<!--
Date: April 17, 2025
Purpose: README for Data layer implementation in CareSupport iOS app
Contains: Overview, organization, and guidelines for the Data layer
-->

# Data Layer

## Overview

The Data layer is responsible for implementing the repository interfaces defined in the Domain layer. It manages data retrieval, storage, and synchronization between different data sources (remote API, local database, etc.). This layer translates between domain entities and data transfer objects (DTOs).

## Directory Structure

```
Data/
├── Network/               # Remote data source implementations
│   ├── ApiClient/         # Base API networking code
│   ├── DTOs/              # Data Transfer Objects
│   ├── Services/          # API service implementations
│   └── Endpoints/         # API endpoint definitions
├── Local/                 # Local persistence implementations
│   ├── CoreData/          # CoreData stack and models
│   ├── UserDefaults/      # User preferences storage
│   └── Keychain/          # Secure credential storage
├── Repositories/          # Repository implementations
└── Mappers/               # Mapping between DTOs and Domain entities
```

## Key Components

### Network Components

The network layer handles all API communication:

```swift
// API Client
class ApiClient {
    func request<T: Decodable>(_ endpoint: Endpoint) async throws -> T {
        // Implementation
    }
}

// DTO Example
struct UserDTO: Decodable {
    let id: String
    let first_name: String
    let last_name: String
    let email: String
    let roles: [String]
    // other properties
}

// API Service
class UserApiService {
    private let apiClient: ApiClient
    
    init(apiClient: ApiClient) {
        self.apiClient = apiClient
    }
    
    func getUser(id: String) async throws -> UserDTO {
        return try await apiClient.request(UserEndpoints.getUser(id: id))
    }
}
```

### Local Storage

Handles local data persistence:

```swift
// CoreData Manager
class CoreDataManager {
    // Core Data stack implementation
}

// User Defaults Manager
class UserDefaultsManager {
    // User defaults access implementation
}

// Keychain Manager
class KeychainManager {
    // Secure storage implementation
}
```

### Repository Implementations

Concrete implementations of Domain repository interfaces:

```swift
class UserRepositoryImpl: UserRepository {
    private let apiService: UserApiService
    private let localDataSource: UserLocalDataSource
    private let mapper: UserMapper
    
    init(apiService: UserApiService, 
         localDataSource: UserLocalDataSource,
         mapper: UserMapper) {
        self.apiService = apiService
        self.localDataSource = localDataSource
        self.mapper = mapper
    }
    
    func getUser(id: String) async throws -> User {
        do {
            let userDTO = try await apiService.getUser(id: id)
            let user = mapper.mapToDomain(userDTO)
            try await localDataSource.saveUser(userDTO)
            return user
        } catch {
            if let localUser = try? await localDataSource.getUser(id: id) {
                return mapper.mapToDomain(localUser)
            }
            throw error
        }
    }
    
    // Other repository method implementations
}
```

### Mappers

Transform between data models and domain entities:

```swift
class UserMapper {
    func mapToDomain(_ dto: UserDTO) -> User {
        return User(
            id: dto.id,
            firstName: dto.first_name,
            lastName: dto.last_name,
            email: dto.email,
            roles: dto.roles.compactMap { mapRole($0) }
            // other properties
        )
    }
    
    func mapToData(_ entity: User) -> UserDTO {
        // Reverse mapping implementation
    }
    
    private func mapRole(_ role: String) -> UserRole? {
        switch role {
        case "caregiver": return .caregiver
        case "care_recipient": return .careRecipient
        case "organizer": return .organizer
        case "supporter": return .supporter
        default: return nil
        }
    }
}
```

## Guidelines

- Implement proper error handling and mapping to domain errors
- Use dependency injection for all services and data sources
- Implement caching strategies for improved performance
- Handle offline capabilities with appropriate synchronization
- Keep repository implementations hidden from the UI layer
- Use mappers to transform between data and domain models
- Follow Swift concurrency patterns (async/await) for asynchronous operations
