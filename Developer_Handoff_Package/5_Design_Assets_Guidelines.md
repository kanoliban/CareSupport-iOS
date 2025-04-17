<!--
Date: April 17, 2025
Purpose: Design Assets & Guidelines for CareSupport iOS app
Contains: UI component library, style guide, and sample screens
Created by: Cascade AI
-->

# CareSupport iOS: Design Assets & Guidelines

**Document Version:** 1.0.0  
**Last Updated:** 2025-04-17  

## 1. UI Component Library

### 1.1 Core Components

#### Buttons

```swift
// Primary Button
struct CSPrimaryButton: View {
    let title: String
    let action: () -> Void
    var isLoading: Bool = false
    var isDisabled: Bool = false
    
    var body: some View {
        Button(action: action) {
            HStack {
                if isLoading {
                    ProgressView()
                        .progressViewStyle(CircularProgressViewStyle(tint: .white))
                        .padding(.trailing, 8)
                }
                Text(title)
                    .fontWeight(.semibold)
            }
            .frame(maxWidth: .infinity)
            .padding(.vertical, 16)
            .background(isDisabled ? Color.gray.opacity(0.3) : Color("Primary"))
            .foregroundColor(.white)
            .cornerRadius(12)
        }
        .disabled(isDisabled || isLoading)
    }
}

// Secondary Button
struct CSSecondaryButton: View {
    let title: String
    let action: () -> Void
    var isDisabled: Bool = false
    
    var body: some View {
        Button(action: action) {
            Text(title)
                .fontWeight(.semibold)
                .frame(maxWidth: .infinity)
                .padding(.vertical, 16)
                .background(Color.white)
                .foregroundColor(Color("Primary"))
                .cornerRadius(12)
                .overlay(
                    RoundedRectangle(cornerRadius: 12)
                        .stroke(Color("Primary"), lineWidth: 1)
                )
        }
        .disabled(isDisabled)
    }
}

// Text Button
struct CSTextButton: View {
    let title: String
    let action: () -> Void
    var isDestructive: Bool = false
    
    var body: some View {
        Button(action: action) {
            Text(title)
                .fontWeight(.medium)
                .foregroundColor(isDestructive ? Color("Destructive") : Color("Primary"))
        }
    }
}
```

#### Text Fields

```swift
struct CSTextField: View {
    let placeholder: String
    @Binding var text: String
    var keyboardType: UIKeyboardType = .default
    var isSecure: Bool = false
    var icon: String? = nil
    @State private var isFocused: Bool = false
    var errorMessage: String? = nil
    
    var body: some View {
        VStack(alignment: .leading, spacing: 4) {
            HStack {
                if let icon = icon {
                    Image(systemName: icon)
                        .foregroundColor(isFocused ? Color("Primary") : Color.gray)
                        .frame(width: 24)
                }
                
                if isSecure {
                    SecureField(placeholder, text: $text)
                        .keyboardType(keyboardType)
                } else {
                    TextField(placeholder, text: $text)
                        .keyboardType(keyboardType)
                }
            }
            .padding(.vertical, 12)
            .padding(.horizontal, 16)
            .background(Color("Background"))
            .cornerRadius(12)
            .overlay(
                RoundedRectangle(cornerRadius: 12)
                    .stroke(
                        errorMessage != nil ? Color("Destructive") :
                            (isFocused ? Color("Primary") : Color.gray.opacity(0.3)),
                        lineWidth: 1
                    )
            )
            .onTapGesture {
                isFocused = true
            }
            
            if let error = errorMessage {
                Text(error)
                    .font(.caption)
                    .foregroundColor(Color("Destructive"))
                    .padding(.leading, 4)
            }
        }
    }
}
```

#### Cards

```swift
// Task Card
struct CSTaskCard: View {
    let task: Task
    let onTap: () -> Void
    
    var body: some View {
        Button(action: onTap) {
            VStack(alignment: .leading, spacing: 12) {
                HStack {
                    priorityIndicator(for: task.priority)
                    Spacer()
                    categoryBadge(for: task.category)
                }
                
                Text(task.title)
                    .fontWeight(.semibold)
                    .foregroundColor(.primary)
                    .lineLimit(2)
                
                if let description = task.description, !description.isEmpty {
                    Text(description)
                        .font(.subheadline)
                        .foregroundColor(.secondary)
                        .lineLimit(2)
                }
                
                HStack {
                    Image(systemName: "clock")
                        .foregroundColor(.secondary)
                    Text(formattedDueDate(task.dueDate))
                        .font(.caption)
                        .foregroundColor(.secondary)
                    
                    Spacer()
                    
                    if let assignee = task.assignedTo {
                        HStack(spacing: 4) {
                            Image(systemName: "person")
                                .foregroundColor(.secondary)
                            Text(assignee.name)
                                .font(.caption)
                                .foregroundColor(.secondary)
                        }
                    }
                }
            }
            .padding(16)
            .background(Color.white)
            .cornerRadius(12)
            .shadow(color: Color.black.opacity(0.05), radius: 2, x: 0, y: 1)
        }
        .buttonStyle(PlainButtonStyle())
    }
    
    // Helper views for task card components
    private func priorityIndicator(for priority: TaskPriority) -> some View {
        Circle()
            .fill(priorityColor(priority))
            .frame(width: 12, height: 12)
    }
    
    private func categoryBadge(for category: TaskCategory) -> some View {
        Text(category.rawValue)
            .font(.caption)
            .fontWeight(.medium)
            .padding(.horizontal, 8)
            .padding(.vertical, 4)
            .background(categoryColor(category).opacity(0.1))
            .foregroundColor(categoryColor(category))
            .cornerRadius(8)
    }
}

// Update Card
struct CSUpdateCard: View {
    let update: Update
    let onTap: () -> Void
    
    var body: some View {
        Button(action: onTap) {
            VStack(alignment: .leading, spacing: 12) {
                HStack {
                    CSUserAvatar(user: update.author, size: .small)
                    
                    VStack(alignment: .leading, spacing: 2) {
                        Text(update.author.name)
                            .fontWeight(.medium)
                            .foregroundColor(.primary)
                        
                        Text("\(update.author.role.displayName) • \(timeAgo(update.timestamp))")
                            .font(.caption)
                            .foregroundColor(.secondary)
                    }
                    
                    Spacer()
                    
                    updateTypeBadge(update.type)
                }
                
                Text(update.content)
                    .foregroundColor(.primary)
                    .multilineTextAlignment(.leading)
                
                if !update.attachments.isEmpty {
                    attachmentsPreview(update.attachments)
                }
                
                HStack {
                    Button(action: {}) {
                        Image(systemName: "heart")
                            .foregroundColor(.secondary)
                    }
                    Text("\(update.reactions.count)")
                        .font(.caption)
                        .foregroundColor(.secondary)
                    
                    Spacer()
                    
                    Button(action: {}) {
                        Image(systemName: "bubble.right")
                            .foregroundColor(.secondary)
                    }
                    Text("\(update.comments.count)")
                        .font(.caption)
                        .foregroundColor(.secondary)
                }
            }
            .padding(16)
            .background(Color.white)
            .cornerRadius(12)
            .shadow(color: Color.black.opacity(0.05), radius: 2, x: 0, y: 1)
        }
        .buttonStyle(PlainButtonStyle())
    }
}
```

#### Navigation Components

```swift
// Tab Bar
struct CSTabBar: View {
    @Binding var selectedTab: Int
    let roleType: RoleType
    
    var body: some View {
        HStack {
            ForEach(0..<tabs(for: roleType).count, id: \.self) { index in
                let tab = tabs(for: roleType)[index]
                VStack(spacing: 4) {
                    Image(systemName: index == selectedTab ? tab.iconSelected : tab.icon)
                        .font(.system(size: 22))
                    Text(tab.title)
                        .font(.caption2)
                        .fontWeight(index == selectedTab ? .medium : .regular)
                }
                .foregroundColor(index == selectedTab ? Color("Primary") : Color.gray)
                .frame(maxWidth: .infinity)
                .padding(.vertical, 8)
                .onTapGesture {
                    withAnimation {
                        selectedTab = index
                    }
                }
            }
        }
        .background(Color.white)
        .cornerRadius(16, corners: [.topLeft, .topRight])
        .shadow(color: Color.black.opacity(0.05), radius: 5, x: 0, y: -2)
    }
    
    // Return appropriate tabs based on role
    private func tabs(for role: RoleType) -> [(title: String, icon: String, iconSelected: String)] {
        switch role {
        case .caregiver:
            return [
                ("Home", "house", "house.fill"),
                ("Tasks", "checklist", "checklist.checked"),
                ("Care", "heart", "heart.fill"),
                ("Updates", "bubble.left.and.bubble.right", "bubble.left.and.bubble.right.fill"),
                ("Profile", "person", "person.fill")
            ]
        case .careRecipient:
            return [
                ("Home", "house", "house.fill"),
                ("Needs", "hand.raised", "hand.raised.fill"),
                ("Journal", "book", "book.fill"),
                ("Circle", "person.3", "person.3.fill"),
                ("Profile", "person", "person.fill")
            ]
        case .organizer:
            return [
                ("Home", "house", "house.fill"),
                ("Schedule", "calendar", "calendar.badge.clock"),
                ("Tasks", "checklist", "checklist.checked"),
                ("Team", "person.3", "person.3.fill"),
                ("Profile", "person", "person.fill")
            ]
        case .supporter:
            return [
                ("Home", "house", "house.fill"),
                ("Tasks", "checklist", "checklist.checked"),
                ("Updates", "bubble.left", "bubble.left.fill"),
                ("Availability", "clock", "clock.fill"),
                ("Profile", "person", "person.fill")
            ]
        }
    }
}

// Navigation Header
struct CSNavigationHeader: View {
    let title: String
    var subtitle: String? = nil
    var leadingAction: (() -> Void)? = nil
    var trailingAction: (() -> Void)? = nil
    var leadingIcon: String? = "chevron.left"
    var trailingIcon: String? = nil
    
    var body: some View {
        HStack(spacing: 16) {
            if let action = leadingAction, let icon = leadingIcon {
                Button(action: action) {
                    Image(systemName: icon)
                        .font(.system(size: 20))
                        .foregroundColor(Color("Primary"))
                        .frame(width: 40, height: 40)
                }
            }
            
            VStack(alignment: .leading, spacing: 2) {
                Text(title)
                    .font(.headline)
                    .foregroundColor(.primary)
                
                if let subtitle = subtitle {
                    Text(subtitle)
                        .font(.subheadline)
                        .foregroundColor(.secondary)
                }
            }
            
            Spacer()
            
            if let action = trailingAction, let icon = trailingIcon {
                Button(action: action) {
                    Image(systemName: icon)
                        .font(.system(size: 20))
                        .foregroundColor(Color("Primary"))
                        .frame(width: 40, height: 40)
                }
            }
        }
        .padding(.horizontal, 16)
        .padding(.vertical, 12)
        .background(Color.white)
    }
}
```

### 1.2 Role-Adaptive Components

```swift
// Role-Adaptive Dashboard Header
struct CSDashboardHeader: View {
    let activeRole: RoleType
    let userName: String
    let careCircleName: String
    let onRoleSwitch: () -> Void
    
    var body: some View {
        VStack(spacing: 16) {
            HStack {
                VStack(alignment: .leading, spacing: 4) {
                    Text("Hello, \(userName)")
                        .font(.title2)
                        .fontWeight(.bold)
                    
                    Button(action: onRoleSwitch) {
                        HStack(spacing: 4) {
                            Text(roleText(for: activeRole))
                                .font(.subheadline)
                                .foregroundColor(Color("Primary"))
                            
                            Image(systemName: "chevron.down")
                                .font(.system(size: 12))
                                .foregroundColor(Color("Primary"))
                        }
                    }
                }
                
                Spacer()
                
                CSRoleBadge(role: activeRole)
            }
            
            if needsCircleInfo(activeRole) {
                HStack {
                    Image(systemName: "person.3.fill")
                        .foregroundColor(Color("Primary").opacity(0.7))
                    
                    Text(careCircleName)
                        .font(.subheadline)
                        .foregroundColor(.secondary)
                    
                    Spacer()
                }
                .padding(.vertical, 8)
                .padding(.horizontal, 12)
                .background(Color("Background"))
                .cornerRadius(8)
            }
        }
        .padding(16)
    }
    
    private func roleText(for role: RoleType) -> String {
        switch role {
        case .caregiver: return "You're in Caregiver mode"
        case .careRecipient: return "You're in Care Recipient mode"
        case .organizer: return "You're in Organizer mode"
        case .supporter: return "You're in Supporter mode"
        }
    }
    
    private func needsCircleInfo(_ role: RoleType) -> Bool {
        role != .careRecipient
    }
}

// Role Badge Component
struct CSRoleBadge: View {
    let role: RoleType
    
    var body: some View {
        Text(role.shortDisplayName)
            .font(.caption)
            .fontWeight(.medium)
            .padding(.horizontal, 8)
            .padding(.vertical, 4)
            .background(roleColor(role).opacity(0.15))
            .foregroundColor(roleColor(role))
            .cornerRadius(8)
    }
    
    private func roleColor(_ role: RoleType) -> Color {
        switch role {
        case .caregiver: return Color("CaregiverAccent")
        case .careRecipient: return Color("RecipientAccent")
        case .organizer: return Color("OrganizerAccent")
        case .supporter: return Color("SupporterAccent")
        }
    }
}
```

## 2. Style Guide

### 2.1 Color Palette

```swift
extension Color {
    // Brand Colors
    static let primary = Color("Primary")           // #356BF8 - Main brand color
    static let secondary = Color("Secondary")       // #6C89D5 - Secondary brand color
    
    // Role-Specific Colors
    static let caregiverAccent = Color("CaregiverAccent")     // #4A89E8
    static let recipientAccent = Color("RecipientAccent")     // #7C4DFF
    static let organizerAccent = Color("OrganizerAccent")     // #43A047
    static let supporterAccent = Color("SupporterAccent")     // #FF7043
    
    // UI Colors
    static let background = Color("Background")     // #F5F7FA - Background color
    static let surface = Color("Surface")           // #FFFFFF - Surface color
    static let textPrimary = Color("TextPrimary")   // #1A1A1A - Primary text
    static let textSecondary = Color("TextSecondary") // #757575 - Secondary text
    
    // Semantic Colors
    static let success = Color("Success")           // #43A047 - Success state
    static let warning = Color("Warning")           // #FFA000 - Warning state
    static let error = Color("Error")               // #E53935 - Error state
    static let info = Color("Info")                 // #039BE5 - Information state
    
    // Task Priority Colors
    static let priorityHigh = Color("PriorityHigh")       // #E53935
    static let priorityMedium = Color("PriorityMedium")   // #FFA000
    static let priorityLow = Color("PriorityLow")         // #43A047
}
```

### 2.2 Typography

```swift
extension Font {
    // Headings
    static let largeTitle = Font.system(size: 34, weight: .bold)
    static let title1 = Font.system(size: 28, weight: .bold)
    static let title2 = Font.system(size: 22, weight: .bold)
    static let title3 = Font.system(size: 20, weight: .semibold)
    
    // Body Text
    static let body = Font.system(size: 17)
    static let bodyBold = Font.system(size: 17, weight: .semibold)
    static let callout = Font.system(size: 16)
    static let subheadline = Font.system(size: 15)
    
    // Detail Text
    static let footnote = Font.system(size: 13)
    static let caption1 = Font.system(size: 12)
    static let caption2 = Font.system(size: 11)
}
```

### 2.3 Spacing System

```swift
struct CSSpacing {
    // Base spacing unit: 4pt
    static let xxs: CGFloat = 4
    static let xs: CGFloat = 8
    static let sm: CGFloat = 12
    static let md: CGFloat = 16
    static let lg: CGFloat = 24
    static let xl: CGFloat = 32
    static let xxl: CGFloat = 48
    
    // Specific spacing applications
    static let pagePadding: CGFloat = 16
    static let cardPadding: CGFloat = 16
    static let itemSpacing: CGFloat = 12
    static let sectionSpacing: CGFloat = 24
}
```

### 2.4 Shadow Styles

```swift
extension View {
    func cardShadow() -> some View {
        self.shadow(color: Color.black.opacity(0.05), radius: 8, x: 0, y: 2)
    }
    
    func elevatedShadow() -> some View {
        self.shadow(color: Color.black.opacity(0.1), radius: 16, x: 0, y: 4)
    }
    
    func subtleShadow() -> some View {
        self.shadow(color: Color.black.opacity(0.03), radius: 4, x: 0, y: 1)
    }
}
```

### 2.5 Border Radius

```swift
struct CSCornerRadius {
    static let small: CGFloat = 4
    static let medium: CGFloat = 8
    static let large: CGFloat = 12
    static let extraLarge: CGFloat = 16
    static let circular: CGFloat = 9999
}
```

## 3. Sample Screens

### 3.1 Caregiver Dashboard

```swift
struct CaregiverDashboardView: View {
    @ObservedObject var viewModel: CaregiverDashboardViewModel
    
    var body: some View {
        ScrollView {
            VStack(spacing: CSSpacing.sectionSpacing) {
                // Dashboard Header
                CSDashboardHeader(
                    activeRole: .caregiver,
                    userName: viewModel.userName,
                    careCircleName: viewModel.careCircleName,
                    onRoleSwitch: viewModel.showRoleSwitcher
                )
                
                // Today's Overview Section
                VStack(alignment: .leading, spacing: CSSpacing.itemSpacing) {
                    SectionHeader(title: "Today's Overview", actionText: "See All")
                    
                    VStack(spacing: CSSpacing.xs) {
                        // Care Recipient Status
                        StatusCard(
                            icon: "heart.fill",
                            iconColor: Color.recipientAccent,
                            title: "Mom's Status",
                            subtitle: "Last updated 30 minutes ago",
                            content: viewModel.recipientStatus,
                            actionText: "Check In"
                        )
                        
                        // Medication Tracker
                        StatusCard(
                            icon: "pill.fill",
                            iconColor: Color.info,
                            title: "Medication",
                            subtitle: "Next dose in 2 hours",
                            content: "3/4 medications taken today",
                            actionText: "View Details"
                        )
                    }
                }
                .padding(.horizontal, CSSpacing.pagePadding)
                
                // Tasks Section
                VStack(alignment: .leading, spacing: CSSpacing.itemSpacing) {
                    SectionHeader(title: "Tasks", actionText: "Add New")
                    
                    ScrollView(.horizontal, showsIndicators: false) {
                        HStack(spacing: CSSpacing.sm) {
                            ForEach(viewModel.tasks) { task in
                                TaskCardCompact(task: task)
                            }
                        }
                        .padding(.horizontal, CSSpacing.pagePadding)
                    }
                }
                
                // Care Circle Section
                VStack(alignment: .leading, spacing: CSSpacing.itemSpacing) {
                    SectionHeader(title: "Care Circle", actionText: "Manage")
                    
                    ScrollView(.horizontal, showsIndicators: false) {
                        HStack(spacing: CSSpacing.sm) {
                            ForEach(viewModel.careCircleMembers) { member in
                                CareCircleMemberCard(member: member)
                            }
                            
                            AddMemberCard()
                        }
                        .padding(.horizontal, CSSpacing.pagePadding)
                    }
                }
                
                // Recent Updates Section
                VStack(alignment: .leading, spacing: CSSpacing.itemSpacing) {
                    SectionHeader(title: "Recent Updates", actionText: "See All")
                    
                    ForEach(viewModel.recentUpdates) { update in
                        CSUpdateCard(update: update, onTap: {
                            viewModel.showUpdateDetail(update)
                        })
                    }
                }
                .padding(.horizontal, CSSpacing.pagePadding)
                
                Spacer(minLength: 80) // Space for tab bar
            }
        }
        .background(Color.background.edgesIgnoringSafeArea(.all))
        .overlay(
            VStack {
                Spacer()
                CSTabBar(selectedTab: $viewModel.selectedTab, roleType: .caregiver)
            }
            .edgesIgnoringSafeArea(.bottom)
        )
    }
}
```

### 3.2 Care Recipient Dashboard

```swift
struct CareRecipientDashboardView: View {
    @ObservedObject var viewModel: CareRecipientDashboardViewModel
    
    var body: some View {
        ScrollView {
            VStack(spacing: CSSpacing.sectionSpacing) {
                // Dashboard Header
                CSDashboardHeader(
                    activeRole: .careRecipient,
                    userName: viewModel.userName,
                    careCircleName: "",
                    onRoleSwitch: viewModel.showRoleSwitcher
                )
                
                // How are you feeling?
                VStack(alignment: .leading, spacing: CSSpacing.sm) {
                    Text("How are you feeling today?")
                        .font(.title3)
                        .padding(.horizontal, CSSpacing.pagePadding)
                    
                    ScrollView(.horizontal, showsIndicators: false) {
                        HStack(spacing: CSSpacing.md) {
                            ForEach(viewModel.moodOptions, id: \.self) { mood in
                                MoodButton(
                                    mood: mood,
                                    isSelected: viewModel.selectedMood == mood,
                                    action: { viewModel.selectedMood = mood }
                                )
                            }
                        }
                        .padding(.horizontal, CSSpacing.pagePadding)
                    }
                }
                
                // Need Help With
                VStack(alignment: .leading, spacing: CSSpacing.itemSpacing) {
                    Text("Need help with")
                        .font(.title3)
                    
                    LazyVGrid(columns: [GridItem(.flexible()), GridItem(.flexible())], spacing: CSSpacing.sm) {
                        ForEach(viewModel.needCategories) { category in
                            NeedCategoryButton(
                                icon: category.icon,
                                title: category.title,
                                action: { viewModel.showNeedRequest(category) }
                            )
                        }
                    }
                }
                .padding(.horizontal, CSSpacing.pagePadding)
                
                // Today's Care
                VStack(alignment: .leading, spacing: CSSpacing.itemSpacing) {
                    SectionHeader(title: "Today's Care", actionText: "See All")
                    
                    ForEach(viewModel.todayCareEvents) { event in
                        CareEventCard(event: event)
                    }
                }
                .padding(.horizontal, CSSpacing.pagePadding)
                
                // Your Caregivers
                VStack(alignment: .leading, spacing: CSSpacing.itemSpacing) {
                    SectionHeader(title: "Your Care Circle", actionText: nil)
                    
                    ScrollView(.horizontal, showsIndicators: false) {
                        HStack(spacing: CSSpacing.sm) {
                            ForEach(viewModel.caregivers) { caregiver in
                                CaregiverCard(caregiver: caregiver)
                            }
                        }
                        .padding(.horizontal, CSSpacing.pagePadding)
                    }
                }
                
                // Journal Prompt
                JournalPromptCard(
                    prompt: viewModel.journalPrompt,
                    action: viewModel.showJournalEntry
                )
                .padding(.horizontal, CSSpacing.pagePadding)
                
                Spacer(minLength: 80) // Space for tab bar
            }
        }
        .background(Color.background.edgesIgnoringSafeArea(.all))
        .overlay(
            VStack {
                Spacer()
                CSTabBar(selectedTab: $viewModel.selectedTab, roleType: .careRecipient)
            }
            .edgesIgnoringSafeArea(.bottom)
        )
    }
}
```

### 3.3 UI Adaptation Examples

The user interface adapts based on user role while maintaining consistent design patterns:

#### Task Detail Screen

- **Caregiver View**: Full editing capabilities, assignment options, detailed history
- **Organizer View**: Assignment and scheduling options, limited editing
- **Supporter View**: Task acceptance, completion reporting, limited viewing
- **Care Recipient View**: Status viewing, preference indication, privacy controls

#### Communication Hub

- **Caregiver View**: All conversation types, broadcast capabilities, full history
- **Organizer View**: Coordination-focused conversations, announcement capabilities
- **Supporter View**: Direct messages, assigned task discussions
- **Care Recipient View**: Privacy-controlled conversations, need expression tools

Additional screen examples and detailed component specifications are available in the Design System documentation.
