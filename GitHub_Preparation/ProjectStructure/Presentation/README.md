<!--
Date: April 17, 2025
Purpose: README for Presentation layer implementation in CareSupport iOS app
Contains: Overview, organization, and guidelines for the Presentation layer
-->

# Presentation Layer

## Overview

The Presentation layer handles all UI-related concerns, including views, view models, and UI-related state management. This layer is responsible for displaying data to the user and handling user interactions, using the MVVM (Model-View-ViewModel) pattern along with SwiftUI.

## Directory Structure

```
Presentation/
├── Common/                # Shared UI components and utilities
│   ├── Components/        # Reusable UI components
│   ├── Extensions/        # UI-related extensions
│   ├── Modifiers/         # SwiftUI view modifiers
│   └── Styles/            # SwiftUI style definitions
├── Features/              # Feature-specific modules
│   ├── Authentication/    # Login, registration, etc.
│   ├── Onboarding/        # User onboarding flows
│   ├── Dashboard/         # Role-specific dashboards
│   ├── Tasks/             # Task management
│   ├── Health/            # Health tracking features
│   ├── Communication/     # Messaging and updates
│   └── Settings/          # User preferences and settings
└── Coordinators/          # Navigation coordinators
```

## Key Components

### View Models

View models handle presentation logic and serve as a bridge between the Domain and UI:

```swift
class TaskListViewModel: ObservableObject {
    private let getTasksUseCase: GetTasksUseCase
    private let completeTaskUseCase: CompleteTaskUseCase
    
    @Published var tasks: [TaskViewModel] = []
    @Published var isLoading: Bool = false
    @Published var error: String? = nil
    
    init(getTasksUseCase: GetTasksUseCase, completeTaskUseCase: CompleteTaskUseCase) {
        self.getTasksUseCase = getTasksUseCase
        self.completeTaskUseCase = completeTaskUseCase
    }
    
    func loadTasks() async {
        isLoading = true
        error = nil
        
        do {
            let domainTasks = try await getTasksUseCase.execute()
            await MainActor.run {
                self.tasks = domainTasks.map { TaskViewModel(from: $0) }
                self.isLoading = false
            }
        } catch {
            await MainActor.run {
                self.error = error.localizedDescription
                self.isLoading = false
            }
        }
    }
    
    func completeTask(_ taskId: String) async {
        do {
            try await completeTaskUseCase.execute(taskId: taskId)
            await loadTasks()
        } catch {
            await MainActor.run {
                self.error = error.localizedDescription
            }
        }
    }
}

struct TaskViewModel: Identifiable {
    let id: String
    let title: String
    let description: String
    let dueDate: Date
    let priority: TaskPriority
    let isCompleted: Bool
    
    init(from domain: Task) {
        self.id = domain.id
        self.title = domain.title
        self.description = domain.description ?? ""
        self.dueDate = domain.dueDate
        self.priority = domain.priority
        self.isCompleted = domain.status == .completed
    }
}
```

### Views

SwiftUI views that display data and handle user interactions:

```swift
struct TaskListView: View {
    @StateObject private var viewModel: TaskListViewModel
    
    init(viewModel: TaskListViewModel) {
        _viewModel = StateObject(wrappedValue: viewModel)
    }
    
    var body: some View {
        ZStack {
            if viewModel.isLoading {
                ProgressView()
            } else {
                content
            }
        }
        .task {
            await viewModel.loadTasks()
        }
        .alert("Error", isPresented: .constant(viewModel.error != nil)) {
            Button("OK") {
                viewModel.error = nil
            }
        } message: {
            if let error = viewModel.error {
                Text(error)
            }
        }
    }
    
    private var content: some View {
        List {
            ForEach(viewModel.tasks) { task in
                TaskRowView(task: task) {
                    Task {
                        await viewModel.completeTask(task.id)
                    }
                }
            }
        }
    }
}
```

### Coordinators

Handle navigation flow between screens:

```swift
protocol Coordinator: AnyObject {
    var childCoordinators: [Coordinator] { get set }
    func start()
}

class TaskCoordinator: Coordinator {
    var childCoordinators: [Coordinator] = []
    private let navigationController: UINavigationController
    private let taskRepository: TaskRepository
    
    init(navigationController: UINavigationController, taskRepository: TaskRepository) {
        self.navigationController = navigationController
        self.taskRepository = taskRepository
    }
    
    func start() {
        let getTasksUseCase = GetTasksUseCaseImpl(taskRepository: taskRepository)
        let completeTaskUseCase = CompleteTaskUseCaseImpl(taskRepository: taskRepository)
        let viewModel = TaskListViewModel(getTasksUseCase: getTasksUseCase, completeTaskUseCase: completeTaskUseCase)
        let viewController = UIHostingController(rootView: TaskListView(viewModel: viewModel))
        navigationController.pushViewController(viewController, animated: true)
    }
    
    func showTaskDetail(taskId: String) {
        // Implementation to navigate to task detail
    }
}
```

## Guidelines

- Keep views as simple as possible, delegating logic to view models
- Use SwiftUI's declarative programming model effectively
- Structure components for maximum reuse
- Apply role-based adaptations at the view model layer
- Use Combine for reactive UI updates
- Implement proper error handling with user-friendly messages
- Follow accessibility best practices
- Implement proper state management for UI states (loading, error, success)
- Use coordinators for navigation between screens
