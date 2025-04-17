<!--
Date: April 17, 2025
Purpose: Contributing guidelines for CareSupport iOS development
Contains: Coding standards, pull request process, and development workflow
-->

# Contributing to CareSupport iOS

Thank you for your interest in contributing to the CareSupport iOS application. This document provides guidelines and instructions for contributing to this project.

## Development Process

### Branch Strategy

We follow a modified GitFlow workflow:

- `main`: Production-ready code, always stable
- `develop`: Integration branch for features, primary working branch
- `feature/*`: Individual feature branches (e.g., `feature/task-management`)
- `bugfix/*`: Bug fix branches (e.g., `bugfix/login-crash`)
- `release/*`: Release preparation branches (e.g., `release/1.0.0`)

### Development Workflow

1. Create a feature branch from `develop`
   ```bash
   git checkout develop
   git pull
   git checkout -b feature/your-feature-name
   ```

2. Develop and test your feature
   - Follow the architecture outlined in the [Architecture Overview](/docs/2_Architecture_Overview.md)
   - Ensure your code conforms to the style guidelines below
   - Write unit tests for business logic

3. Submit a pull request to `develop`
   - Use the pull request template
   - Ensure all automated checks pass
   - Request reviews from appropriate team members

4. After approval, your branch will be merged into `develop`

5. Periodically, `develop` will be merged into `main` for releases

## Coding Standards

### Swift Style Guidelines

We follow the [Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/) with some project-specific additions:

- **Naming**:
  - Use descriptive names with camel case
  - Methods and variables should start with lowercase letters (e.g., `calculateTotal()`)
  - Types (classes, structs, enums) should start with uppercase letters (e.g., `TaskManager`)
  - Acronyms should be all uppercase when they are all caps (e.g., `APIClient`) or all lowercase (e.g., `getUrl()`)

- **Code Organization**:
  - Group related properties and methods together
  - Use MARK comments to organize code sections
  - Follow the order: properties, initializers, lifecycle methods, public methods, private methods

- **Documentation**:
  - All public interfaces should have documentation comments
  - Use the triple-slash format for documentation (`///`)
  - Include parameter descriptions and return value descriptions

### SwiftUI Guidelines

- Use declarative style consistently
- Extract reusable views into separate components
- Follow view composition pattern for complex views
- Use view modifiers consistently

### Architecture Guidelines

- Follow the Clean Architecture + MVVM pattern as outlined in the architecture documentation
- Keep view models free of business logic
- Business logic belongs in use cases
- Data access logic belongs in repositories
- Strictly follow dependency direction (outer layers depend on inner layers)

## Pull Request Process

1. **Title**: Use a descriptive title with the format: `[Feature/Fix/Refactor]: Brief description`

2. **Description**: Include:
   - Summary of changes
   - Link to relevant issue(s)
   - Screenshots or videos for UI changes
   - Testing steps
   - Breaking changes if any

3. **Acceptance Criteria**:
   - Code review by at least one team member
   - All automated tests pass
   - No regression in existing functionality
   - Documentation updated if needed

4. **Code Review Guidelines**:
   - Be respectful and constructive
   - Focus on code, not the coder
   - Provide specific, actionable feedback
   - Approve once satisfied, or request changes with clear explanations

## Testing Guidelines

### Unit Testing

- Use XCTest framework
- Follow Arrange-Act-Assert (AAA) pattern
- Mock external dependencies
- Test happy path and edge cases
- Aim for high business logic test coverage (80%+)

### UI Testing

- Test critical user flows
- Use accessibility identifiers for test automation
- Test on multiple device sizes when appropriate

## Getting Help

If you have questions about contributing, please contact:
- Technical Lead: [techlead@caresupport.com]
- iOS Lead Developer: [ios-lead@caresupport.com]
