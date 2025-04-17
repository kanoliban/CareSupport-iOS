<!--
Date: April 17, 2025
Purpose: README for Application layer implementation in CareSupport iOS app
Contains: Overview, organization, and guidelines for the Application layer
-->

# Application Layer

## Overview

The Application layer is responsible for application-wide concerns such as dependency injection, navigation, app lifecycle, and configuration. This layer acts as the glue between the different layers of the application and handles app-specific functionality that doesn't belong in other layers.

## Directory Structure

```
Application/
├── DI/                    # Dependency injection container
├── Navigation/            # App-wide navigation infrastructure
├── AppLifecycle/          # App delegate and scene delegate
├── Configuration/         # Environment configurations
├── Extensions/            # App-wide extensions
└── Resources/             # Assets, localization, etc.
```

## Key Components

### Dependency Injection

The DI system manages object creation and dependencies:

```swift
class AppContainer {
    // Singletons
    let apiClient: ApiClient
    let coreDataManager: CoreDataManager
    let userDefaultsManager: UserDefaultsManager
    let keychainManager: KeychainManager
    
    // Repositories
    lazy var userRepository: UserRepository = {
        return UserRepositoryImpl(
            apiService: UserApiService(apiClient: apiClient),
            localDataSource: UserLocalDataSourceImpl(coreDataManager: coreDataManager),
            mapper: UserMapper()
        )
    }()
    
    lazy var taskRepository: TaskRepository = {
        return TaskRepositoryImpl(
            apiService: TaskApiService(apiClient: apiClient),
            localDataSource: TaskLocalDataSourceImpl(coreDataManager: coreDataManager),
            mapper: TaskMapper()
        )
    }()
    
    // Other repositories...
    
    // Use Cases factory methods
    func makeGetUserProfileUseCase() -> GetUserProfileUseCase {
        return GetUserProfileUseCaseImpl(userRepository: userRepository)
    }
    
    func makeGetTasksUseCase() -> GetTasksUseCase {
        return GetTasksUseCaseImpl(taskRepository: taskRepository)
    }
    
    // Other use case factory methods...
    
    // View Models factory methods
    func makeTaskListViewModel() -> TaskListViewModel {
        return TaskListViewModel(
            getTasksUseCase: makeGetTasksUseCase(),
            completeTaskUseCase: makeCompleteTaskUseCase()
        )
    }
    
    // Initialize container
    init() {
        self.apiClient = ApiClient(baseURL: AppConfiguration.apiBaseURL)
        self.coreDataManager = CoreDataManager()
        self.userDefaultsManager = UserDefaultsManager()
        self.keychainManager = KeychainManager()
    }
}
```

### Main Coordinator

The central navigation coordinator:

```swift
class AppCoordinator: Coordinator {
    var childCoordinators: [Coordinator] = []
    private let window: UIWindow
    private let container: AppContainer
    
    init(window: UIWindow, container: AppContainer) {
        self.window = window
        self.container = container
    }
    
    func start() {
        if let token = container.keychainManager.getAccessToken(), !token.isEmpty {
            showMainFlow()
        } else {
            showAuthFlow()
        }
        window.makeKeyAndVisible()
    }
    
    private func showAuthFlow() {
        let navigationController = UINavigationController()
        window.rootViewController = navigationController
        
        let authCoordinator = AuthCoordinator(
            navigationController: navigationController,
            container: container,
            completion: { [weak self] in
                self?.showMainFlow()
            }
        )
        
        childCoordinators.append(authCoordinator)
        authCoordinator.start()
    }
    
    private func showMainFlow() {
        let tabBarController = UITabBarController()
        window.rootViewController = tabBarController
        
        let mainCoordinator = MainCoordinator(
            tabBarController: tabBarController,
            container: container
        )
        
        childCoordinators.append(mainCoordinator)
        mainCoordinator.start()
    }
}
```

### App Lifecycle

The app and scene delegate setup:

```swift
@main
struct CareSupportApp: App {
    @UIApplicationDelegateAdaptor private var appDelegate: AppDelegate
    
    private let container = AppContainer()
    private let appCoordinator: AppCoordinator
    
    init() {
        let window = UIWindow(frame: UIScreen.main.bounds)
        appCoordinator = AppCoordinator(window: window, container: container)
    }
    
    var body: some Scene {
        WindowGroup {
            ContentView()
                .onAppear {
                    appCoordinator.start()
                }
        }
    }
}

class AppDelegate: NSObject, UIApplicationDelegate {
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]? = nil) -> Bool {
        // Configure app-wide services
        configureAppearance()
        configureLogging()
        configureNotifications()
        return true
    }
    
    private func configureAppearance() {
        // Set up global appearance
    }
    
    private func configureLogging() {
        // Set up logging system
    }
    
    private func configureNotifications() {
        // Set up notifications
    }
}
```

### Configuration

Environment-specific configurations:

```swift
enum Environment {
    case development
    case staging
    case production
}

struct AppConfiguration {
    static var current: Environment {
        #if DEBUG
        return .development
        #else
        if Bundle.main.bundleIdentifier?.contains("staging") == true {
            return .staging
        } else {
            return .production
        }
        #endif
    }
    
    static var apiBaseURL: URL {
        switch current {
        case .development:
            return URL(string: "https://dev-api.caresupport.com/v1")!
        case .staging:
            return URL(string: "https://staging-api.caresupport.com/v1")!
        case .production:
            return URL(string: "https://api.caresupport.com/v1")!
        }
    }
    
    // Other configuration parameters
}
```

## Guidelines

- Keep the application layer thin and focused on coordination
- Use dependency injection to manage object creation and dependencies
- Create a clear navigation structure using coordinators
- Implement proper app lifecycle handling
- Manage environment-specific configurations
- Keep resources organized and localized where appropriate
- Document app-wide extensions clearly
- Create reusable utility functions for common operations
