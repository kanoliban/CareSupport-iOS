# Task Management System Specification

**Version:** 1.0.0  
**Last Updated:** 2025-04-14  
**Status:** Draft  
**Document Location:** `/06_Core_Platform/Task_Management/Task_Management_Specification.md`

## 1. Overview

### 1.1 Purpose

The Task Management System serves as the central coordination mechanism for the CareSupport platform, enabling care activities to be created, assigned, tracked, and completed across all roles. This system translates care intentions into actionable tasks while respecting role boundaries, permissions, and privacy preferences. It provides the foundation for effective care coordination while adapting to the specific needs and abilities of each role.

### 1.2 Related Documents

- `/02_Research/Relationship_Model.md` - Role relationships and task flow patterns
- `/03_Information_Architecture/Navigation_System.md` - Task-related navigation flows
- `/07_Technical/Data_Models/data-model-extensions.md` - Task data structures
- `/07_Technical/API_Requirements/Core_API_Specs.md` - Task API endpoints
- `/07_Technical/Implementation_Guides/Permission_Framework.md` - Task-related permissions
- `/07_Technical/Implementation_Guides/State_Management_System.md` - Task state management
- `/08_Design_Standards/Component_Library/Component_Overview.md` - Component system
- `/05_Care_Plan_Setup/Caregiver/[05]_Quick-Win-Action.md` - Task-related onboarding

### 1.3 Module Goals

- Provide a unified task system that adapts to each role's capabilities and needs
- Enable effective coordination between caregivers, organizers, supporters, and care recipients
- Support both simple and complex task workflows with appropriate privacy controls
- Create visibility of care activities while respecting role-specific permissions
- Reduce caregiver cognitive load through efficient task delegation and tracking
- Enable care recipients to maintain autonomy through visibility and input

## 2. Core Components

### 2.1 Task Creation Components

#### 2.1.1 Purpose
Provides interfaces for creating and defining tasks across multiple contexts, adapted to each role's permissions and capabilities.

#### 2.1.2 Technical Specifications

| Attribute | Specification |
|-----------|---------------|
| **Types** | Quick add, Full form, Template-based, Voice input |
| **Contexts** | Dashboard action, Task list, Calendar integration, Care reminder |
| **Complexity Levels** | Simple (title + date), Standard, Detailed (with subtasks) |
| **Required Fields** | Title, Due date |
| **Optional Fields** | Description, Priority, Assignee, Category, Recurrence, Notifications |

#### 2.1.3 Component Structure

```json
{
  "task_creation": {
    "form_types": {
      "quick_add": {
        "fields": ["title", "due_date", "assignee"]
      },
      "standard": {
        "fields": ["title", "description", "due_date", "assignee", "priority", "category", "reminders"]
      },
      "detailed": {
        "fields": ["title", "description", "due_date", "assignee", "priority", "category", "reminders", "subtasks", "attachments"]
      },
      "template_based": {
        "template_id": "template123",
        "customizable_fields": ["due_date", "assignee", "notes"]
      }
    }
  }
}
```

#### 2.1.4 Role Adaptations

| Role | Creation Capabilities | Default Form Type | Field Adaptations | Special Features |
|------|----------------------|-------------------|-------------------|-----------------|
| **Caregiver** | All task types | Standard | All fields available | Template library, Subtasks, Recurrence |
| **Care Recipient** | Need requests, Personal tasks | Simple | Limited fields, Focus on title and timing | Dignity-focused language, Privacy toggle |
| **Organizer** | Team tasks, Coordination tasks | Standard | Team-oriented fields, Multiple assignees | Bulk creation, Conflict detection |
| **Supporter** | Personal commitments only | Simple | Very limited fields | Availability check integration |

#### 2.1.5 Implementation Example

```jsx
// Task creation component with role adaptations
function TaskCreationForm({ userRole, onSubmit, initialValues = {} }) {
  // Get form configuration based on role
  const formConfig = useTaskFormConfig(userRole);
  const [formValues, setFormValues] = useState({
    title: initialValues.title || '',
    description: initialValues.description || '',
    dueDate: initialValues.dueDate || null,
    priority: initialValues.priority || 'medium',
    category: initialValues.category || 'general',
    assignee: initialValues.assignee || null,
    // Additional fields
  });
  
  // Handle form submission
  const handleSubmit = (e) => {
    e.preventDefault();
    
    // Validate form based on role-specific requirements
    const errors = validateTaskForm(formValues, formConfig);
    
    if (Object.keys(errors).length === 0) {
      onSubmit(formValues);
    } else {
      // Handle validation errors
      setFormErrors(errors);
    }
  };
  
  // Handle voice input for task title
  const handleVoiceInput = async () => {
    try {
      const text = await startVoiceRecognition();
      setFormValues({
        ...formValues,
        title: text
      });
    } catch (error) {
      // Handle voice recognition errors
    }
  };
  
  return (
    <form onSubmit={handleSubmit} className="task-creation-form">
      <h2>{formConfig.title}</h2>
      
      <div className="form-field">
        <label htmlFor="title">Task Title</label>
        <div className="input-with-voice">
          <input
            id="title"
            type="text"
            value={formValues.title}
            onChange={(e) => setFormValues({...formValues, title: e.target.value})}
            placeholder={formConfig.placeholders.title}
            required
          />
          {formConfig.voiceEnabled && (
            <VoiceInputButton onClick={handleVoiceInput} />
          )}
        </div>
      </div>
      
      {formConfig.showField('description') && (
        <div className="form-field">
          <label htmlFor="description">Description</label>
          <textarea
            id="description"
            value={formValues.description}
            onChange={(e) => setFormValues({...formValues, description: e.target.value})}
            placeholder={formConfig.placeholders.description}
          />
        </div>
      )}
      
      <div className="form-field">
        <label htmlFor="dueDate">Due Date</label>
        <DatePicker
          id="dueDate"
          selected={formValues.dueDate}
          onChange={(date) => setFormValues({...formValues, dueDate: date})}
          minDate={new Date()}
        />
      </div>
      
      {formConfig.showField('assignee') && (
        <div className="form-field">
          <label htmlFor="assignee">Assign To</label>
          <AssigneeSelector
            id="assignee"
            value={formValues.assignee}
            onChange={(assignee) => setFormValues({...formValues, assignee})}
            multiSelect={formConfig.multiAssignee}
          />
        </div>
      )}
      
      {formConfig.showField('priority') && (
        <div className="form-field">
          <label htmlFor="priority">Priority</label>
          <PrioritySelector
            id="priority"
            value={formValues.priority}
            onChange={(priority) => setFormValues({...formValues, priority})}
          />
        </div>
      )}
      
      {formConfig.showField('category') && (
        <div className="form-field">
          <label htmlFor="category">Category</label>
          <CategorySelector
            id="category"
            value={formValues.category}
            onChange={(category) => setFormValues({...formValues, category})}
            options={formConfig.categoryOptions}
          />
        </div>
      )}
      
      {userRole === 'care_recipient' && (
        <div className="form-field">
          <PrivacyToggle
            value={formValues.isPrivate}
            onChange={(isPrivate) => setFormValues({...formValues, isPrivate})}
            label="Keep this request private"
          />
        </div>
      )}
      
      <div className="form-actions">
        <Button type="submit" primary>{formConfig.submitLabel}</Button>
        <Button type="button" secondary onClick={onCancel}>Cancel</Button>
      </div>
    </form>
  );
}
```

### 2.2 Task Assignment Components

#### 2.2.1 Purpose
Enables tasks to be assigned to appropriate care circle members, with role-specific workflows and permissions.

#### 2.2.2 Technical Specifications

| Attribute | Specification |
|-----------|---------------|
| **Assignment Types** | Direct, Self-assign, Request, Suggestion |
| **Assignment Flows** | Single assignee, Multiple assignees, Team |
| **Acceptance Options** | Auto-accept, Manual accept, With conditions |
| **Notification Types** | In-app, Email, SMS, Push |
| **Permission Requirements** | `assign_task` permission |

#### 2.2.3 Component Structure

```json
{
  "task_assignment": {
    "assignment_types": {
      "direct": {
        "requires_acceptance": false,
        "needs_permission": "assign_task"
      },
      "request": {
        "requires_acceptance": true,
        "needs_permission": "request_task_help"
      },
      "suggestion": {
        "requires_acceptance": true,
        "needs_permission": "suggest_task"
      }
    },
    "assignment_states": [
      "unassigned",
      "pending_acceptance",
      "accepted",
      "declined",
      "reassigned"
    ]
  }
}
```

#### 2.2.4 Role Adaptations

| Role | Assignment Capabilities | Default Assignment Type | Acceptance Requirements | Special Features |
|------|------------------------|-------------------------|-------------------------|-----------------|
| **Caregiver** | Can assign to any role | Direct | None for Supporters/Organizers | Bulk assignment, Favorites |
| **Care Recipient** | Can request help only | Request | Always requires acceptance | Privacy-aware requests, Preferred helpers |
| **Organizer** | Team assignments | Direct | None for team members | Workload balancing, Conflict resolution |
| **Supporter** | Self-assign only | Self-assign | N/A (self) | Availability check, Opt-in tasks |

#### 2.2.5 Implementation Example

```jsx
// Task assignment component with role adaptations
function TaskAssignmentPanel({ task, userRole, onAssign, onCancel }) {
  const { activeRole } = useRoleContext();
  const { careCircleMembers } = useCareCircle();
  const [selectedAssignee, setSelectedAssignee] = useState(null);
  const [assignmentType, setAssignmentType] = useState(getDefaultAssignmentType(userRole));
  const [message, setMessage] = useState('');
  
  // Get available members based on role
  const availableMembers = useMemo(() => {
    // Filter members based on role permissions
    return careCircleMembers.filter(member => {
      // Implement role-specific filtering logic
      if (userRole === 'caregiver') {
        // Caregivers can assign to all roles
        return true;
      } else if (userRole === 'organizer') {
        // Organizers can assign to supporters and other organizers
        return ['supporter', 'organizer'].includes(member.role);
      } else if (userRole === 'care_recipient') {
        // Care recipients can request from caregivers and supporters
        return ['caregiver', 'supporter'].includes(member.role);
      } else {
        // Supporters can only self-assign
        return member.id === currentUserId;
      }
    });
  }, [careCircleMembers, userRole]);
  
  // Handle assignment submission
  const handleSubmit = () => {
    if (!selectedAssignee) return;
    
    onAssign({
      taskId: task.id,
      assigneeId: selectedAssignee.id,
      assignmentType,
      message,
      requiresAcceptance: getRequiresAcceptance(assignmentType, selectedAssignee.role)
    });
  };
  
  // Get assignment component configuration based on role
  const config = useAssignmentConfig(userRole);
  
  return (
    <div className="task-assignment-panel">
      <h3>{config.title}</h3>
      
      {/* Assignment type selector - only shown for roles that can choose */}
      {config.showAssignmentTypeSelector && (
        <div className="assignment-type-selector">
          <label>Assignment Type</label>
          <RadioGroup
            value={assignmentType}
            onChange={setAssignmentType}
            options={config.assignmentTypeOptions}
          />
        </div>
      )}
      
      {/* Assignee selector */}
      <div className="assignee-selector">
        <label>{config.assigneeLabel}</label>
        {userRole === 'supporter' ? (
          // Supporters can only self-assign, show confirmation
          <div className="self-assignment-confirm">
            <Checkbox
              checked={selectedAssignee !== null}
              onChange={() => setSelectedAssignee(
                selectedAssignee ? null : getCurrentUser()
              )}
              label="I'll handle this task"
            />
          </div>
        ) : (
          // Other roles can select assignees
          <MemberSelector
            members={availableMembers}
            selected={selectedAssignee}
            onChange={setSelectedAssignee}
            placeholder={config.assigneePlaceholder}
            multiSelect={config.allowMultiAssign}
          />
        )}
      </div>
      
      {/* Optional message - shown for request type assignments */}
      {(assignmentType === 'request' || config.alwaysShowMessage) && (
        <div className="assignment-message">
          <label>{config.messageLabel}</label>
          <textarea
            value={message}
            onChange={(e) => setMessage(e.target.value)}
            placeholder={config.messagePlaceholder}
          />
          <VoiceInputButton 
            onRecording={(text) => setMessage(text)}
          />
        </div>
      )}
      
      {/* Show workload info for organizers */}
      {userRole === 'organizer' && selectedAssignee && (
        <MemberWorkloadIndicator memberId={selectedAssignee.id} />
      )}
      
      {/* Show privacy notice for care recipients */}
      {userRole === 'care_recipient' && (
        <PrivacyNotice
          message={config.privacyMessage}
          icon="privacy-shield"
        />
      )}
      
      {/* Actions */}
      <div className="actions">
        <Button 
          primary 
          onClick={handleSubmit}
          disabled={!selectedAssignee}
        >
          {config.submitLabel}
        </Button>
        <Button secondary onClick={onCancel}>
          Cancel
        </Button>
      </div>
    </div>
  );
}
```

### 2.3 Task List Components

#### 2.3.1 Purpose
Provides role-appropriate views of tasks with filtering, sorting, and interaction capabilities adapted to each user's needs and permissions.

#### 2.3.2 Technical Specifications

| Attribute | Specification |
|-----------|---------------|
| **View Types** | List, Board, Calendar, Agenda |
| **Filtering Options** | By status, assignee, date, priority, category |
| **Sorting Options** | Due date, Priority, Creation date, Alphabetical |
| **Grouping Options** | By assignee, By category, By status, By date |
| **Item Density** | Compact, Standard, Comfortable |

#### 2.3.3 Component Structure

```json
{
  "task_list": {
    "view_types": ["list", "board", "calendar", "agenda"],
    "filter_options": {
      "status": ["all", "incomplete", "complete", "overdue"],
      "assignee": ["all", "me", "unassigned", "[specific_user]"],
      "timeframe": ["all", "today", "tomorrow", "this_week", "next_week", "custom"]
    },
    "sort_options": ["due_date_asc", "due_date_desc", "priority_desc", "created_at_desc"],
    "group_options": ["none", "assignee", "category", "status", "date"]
  }
}
```

#### 2.3.4 Role Adaptations

| Role | Default View | Filtering Focus | Action Capabilities | Special Features |
|------|--------------|-----------------|---------------------|-----------------|
| **Caregiver** | List or Board | All tasks, Status, Priority | Full edit, assign, complete | Task reminders, Recurrence indicators |
| **Care Recipient** | Agenda | Today's tasks, My requests | Limited to own tasks | Privacy filter, Need request tracking |
| **Organizer** | Board | Team view, Category, Assignee | Assign, reassign, track | Team workload view, Conflict indicators |
| **Supporter** | List | My tasks, Upcoming | Accept, complete only | Simple focused view, Time commitment info |

#### 2.3.5 Implementation Example

```jsx
// Task list component with role adaptations
function TaskList({ userRole, initialFilters = {} }) {
  // Get role-specific configuration
  const config = useTaskListConfig(userRole);
  
  // State for filters, sorting, grouping
  const [viewType, setViewType] = useState(config.defaultViewType);
  const [filters, setFilters] = useState({
    status: initialFilters.status || config.defaultFilters.status,
    assignee: initialFilters.assignee || config.defaultFilters.assignee,
    timeframe: initialFilters.timeframe || config.defaultFilters.timeframe,
    category: initialFilters.category || config.defaultFilters.category
  });
  const [sortOption, setSortOption] = useState(config.defaultSortOption);
  const [groupOption, setGroupOption] = useState(config.defaultGroupOption);
  
  // Fetch tasks with current filters
  const { tasks, loading, error } = useTasks({ filters, sort: sortOption });
  
  // Handle task actions based on role permissions
  const handleTaskAction = (taskId, action) => {
    switch (action) {
      case 'view':
        // All roles can view task details
        navigateToTaskDetail(taskId);
        break;
      case 'edit':
        // Only roles with edit permission
        if (canEditTask(taskId)) {
          navigateToTaskEdit(taskId);
        }
        break;
      case 'complete':
        // Only assignee or roles with complete permission
        if (canCompleteTask(taskId)) {
          completeTask(taskId);
        }
        break;
      case 'assign':
        // Only roles with assign permission
        if (canAssignTask(taskId)) {
          openAssignmentPanel(taskId);
        }
        break;
      case 'delete':
        // Only creator or admin roles
        if (canDeleteTask(taskId)) {
          confirmDeleteTask(taskId);
        }
        break;
    }
  };
  
  // Group tasks if grouping is enabled
  const groupedTasks = useMemo(() => {
    if (groupOption === 'none') return { 'all': tasks };
    
    return tasks.reduce((groups, task) => {
      const groupKey = getGroupKey(task, groupOption);
      if (!groups[groupKey]) groups[groupKey] = [];
      groups[groupKey].push(task);
      return groups;
    }, {});
  }, [tasks, groupOption]);
  
  // Render appropriate view type
  const renderTaskView = () => {
    switch (viewType) {
      case 'list':
        return (
          <TaskListView
            tasks={tasks}
            grouped={groupOption !== 'none'}
            groupedTasks={groupedTasks}
            onTaskAction={handleTaskAction}
            actionConfig={config.actionConfig}
          />
        );
      case 'board':
        return (
          <TaskBoardView
            tasks={tasks}
            grouped={groupOption !== 'none'}
            groupedTasks={groupedTasks}
            onTaskAction={handleTaskAction}
            actionConfig={config.actionConfig}
          />
        );
      case 'calendar':
        return (
          <TaskCalendarView
            tasks={tasks}
            onTaskAction={handleTaskAction}
            actionConfig={config.actionConfig}
          />
        );
      case 'agenda':
        return (
          <TaskAgendaView
            tasks={tasks}
            onTaskAction={handleTaskAction}
            actionConfig={config.actionConfig}
          />
        );
    }
  };
  
  return (
    <div className="task-list-container">
      {/* Header with title and action button */}
      <div className="task-list-header">
        <h2>{config.title}</h2>
        
        {/* Quick add button - only shown for roles that can create */}
        {config.canCreate && (
          <Button primary onClick={() => navigateToTaskCreate()}>
            {config.createButtonText}
          </Button>
        )}
      </div>
      
      {/* Controls bar with filters, view toggle, etc. */}
      <div className="task-list-controls">
        {/* View type selector - simplified for some roles */}
        {config.showViewTypeSelector && (
          <ViewTypeSelector
            options={config.availableViewTypes}
            value={viewType}
            onChange={setViewType}
          />
        )}
        
        {/* Filter controls - adapted to role */}
        <FilterControls
          filters={filters}
          onChange={setFilters}
          options={config.filterOptions}
        />
        
        {/* Sort selector */}
        <SortSelector
          value={sortOption}
          onChange={setSortOption}
          options={config.sortOptions}
        />
        
        {/* Group selector - not shown for all roles */}
        {config.showGroupOptions && (
          <GroupSelector
            value={groupOption}
            onChange={setGroupOption}
            options={config.groupOptions}
          />
        )}
      </div>
      
      {/* Task list content */}
      {loading ? (
        <LoadingIndicator />
      ) : error ? (
        <ErrorMessage error={error} />
      ) : tasks.length === 0 ? (
        <EmptyState
          message={config.emptyStateMessage}
          action={config.canCreate ? {
            label: config.createButtonText,
            onClick: () => navigateToTaskCreate()
          } : null}
        />
      ) : (
        renderTaskView()
      )}
    </div>
  );
}
```

### 2.4 Task Detail Components

#### 2.4.1 Purpose
Provides comprehensive task information with role-appropriate details and actions, serving as the primary interface for interacting with individual tasks.

#### 2.4.2 Technical Specifications

| Attribute | Specification |
|-----------|---------------|
| **View Modes** | Full detail, Quick view, Edit mode |
| **Sections** | Basic info, Details, Status history, Comments, Attachments |
| **Action Types** | View, Edit, Complete, Assign, Delete, Comment |
| **State Tracking** | Creation, Updates, Assignment, Completion, Deletion |
| **Privacy Controls** | Visibility settings, Information access limits |

#### 2.4.3 Component Structure

```json
{
  "task_detail": {
    "sections": {
      "basic_info": {
        "fields": ["title", "status", "due_date", "assignee"]
      },
      "details": {
        "fields": ["description", "category", "priority", "attachments"]
      },
      "activity": {
        "fields": ["created_by", "last_updated", "status_changes"]
      },
      "comments": {
        "fields": ["comment_list", "add_comment"]
      }
    },
    "actions": {
      "view": { "permission": "view_task" },
      "edit": { "permission": "edit_task" },
      "complete": { "permission": "complete_task" },
      "assign": { "permission": "assign_task" },
      "delete": { "permission": "delete_task" },
      "comment": { "permission": "comment_task" }
    }
  }
}
```

#### 2.4.4 Role Adaptations

| Role | Detail Focus | Action Capabilities | Information Display | Special Features |
|------|--------------|---------------------|---------------------|-----------------|
| **Caregiver** | Full details | All actions | Complete history | Task dependencies, Care context |
| **Care Recipient** | Simplified details | Request updates, Mark complete | Limited history | Privacy indicators, Help request |
| **Organizer** | Coordination details | Assign, track, comment | Assignment focus | Team context, Conflicts |
| **Supporter** | Task instructions | Accept, complete, comment | Need-to-know basis | Effort indicator, Time estimate |

#### 2.4.5 Implementation Example

```jsx
// Task detail component with role adaptations
function TaskDetail({ taskId, userRole, mode = 'view' }) {
  // Get role-specific configuration
  const config = useTaskDetailConfig(userRole);
  
  // Fetch task with permissions check
  const { task, loading, error } = useTask(taskId);
  
  // State for comments, edit mode
  const [isEditing, setIsEditing] = useState(mode === 'edit');
  const [editValues, setEditValues] = useState({});
  
  // Permission checks for actions
  const canEdit = usePermission('edit_task', taskId);
  const canComplete = usePermission('complete_task', taskId);
  const canAssign = usePermission('assign_task', taskId);
  const canDelete = usePermission('delete_task', taskId);
  const canComment = usePermission('comment_task', taskId);
  
  // Handle edit mode toggle
  const handleEditToggle = () => {
    if (isEditing) {
      // Save changes
      updateTask(taskId, editValues);
      setIsEditing(false);
    } else {
      // Enter edit mode
      setEditValues({
        title: task.title,
        description: task.description,
        dueDate: task.dueDate,
        priority: task.priority,
        category: task.category
      });
      setIsEditing(true);
    }
  };
  
  // Handle task completion
  const handleComplete = () => {
    completeTask(taskId);
  };
  
  // Handle task assignment
  const handleAssign = () => {
    openAssignmentPanel(taskId);
  };
  
  // Handle task deletion
  const handleDelete = () => {
    confirmDeleteTask(taskId);
  };
  
  // Handle adding comment
  const handleAddComment = (comment) => {
    addTaskComment(taskId, comment);
  };
  
  if (loading) return <LoadingIndicator />;
  if (error) return <ErrorMessage error={error} />;
  if (!task) return <NotFoundMessage resourceType="task" />;
  
  return (
    <div className="task-detail">
      {/* Header with title and primary actions */}
      <div className="task-detail-header">
        {isEditing ? (
          <input
            type="text"
            value={editValues.title}
            onChange={(e) => setEditValues({...editValues, title: e.target.value})}
            className="task-title-edit"
          />
        ) : (
          <h2 className="task-title">{task.title}</h2>
        )}
        
        {/* Status badge */}
        <StatusBadge status={task.status} />
        
        {/* Primary actions based on permissions */}
        <div className="task-actions">
          {canEdit && (
            <Button onClick={handleEditToggle}>
              {isEditing ? "Save" : "Edit"}
            </Button>
          )}
          
          {canComplete && task.status !== 'completed' && (
            <Button primary onClick={handleComplete}>
              Complete
            </Button>
          )}
          
          {canAssign && (
            <Button onClick={handleAssign}>
              {task.assignee ? "Reassign" : "Assign"}
            </Button>
          )}
          
          {canDelete && (
            <Button danger onClick={handleDelete}>
              Delete
            </Button>
          )}
        </div>
      </div>
      
      {/* Basic info section */}
      <div className="task-basic-info">
        <div className="task-due-date">
          <span className="label">Due:</span>
          {isEditing ? (
            <DatePicker
              selected={editValues.dueDate}
              onChange={(date) => setEditValues({...editValues, dueDate: date})}
            />
          ) : (
            <span className="value">{formatDate(task.dueDate)}</span>
          )}
        </div>
        
        <div className="task-assignee">
          <span className="label">Assigned to:</span>
          {isEditing && canAssign ? (
            <AssigneeSelector
              value={editValues.assignee}
              onChange={(assignee) => setEditValues({...editValues, assignee})}
            />
          ) : (
            <span className="value">
              {task.assignee ? (
                <UserChip user={task.assignee} />
              ) : (
                <span className="unassigned">Unassigned</span>
              )}
            </span>
          )}
        </div>
        
        {/* Only show priority for roles that use it */}
        {config.showPriority && (
          <div className="task-priority">
            <span className="label">Priority:</span>
            {isEditing ? (
              <PrioritySelector
                value={editValues.priority}
                onChange={(priority) => setEditValues({...editValues, priority})}
              />
            ) : (
              <PriorityBadge priority={task.priority} />
            )}
          </div>
        )}
        
        {/* Only show category for roles that use it */}
        {config.showCategory && (
          <div className="task-category">
            <span className="label">Category:</span>
            {isEditing ? (
              <CategorySelector
                value={editValues.category}
                onChange={(category) => setEditValues({...editValues, category})}
              />
            ) : (
              <CategoryBadge category={task.category} />
            )}
          </div>
        )}
      </div>
      
      {/* Description section - collapsed for some roles by default */}
      <Collapsible 
        title="Description" 
        defaultExpanded={config.expandDescription}
      >
        {isEditing ? (
          <textarea
            value={editValues.description}
            onChange={(e) => setEditValues({...editValues, description: e.target.value})}
            className="task-description-edit"
          />
        ) : (
          <div className="task-description">
            {task.description || "No description provided."}
          </div>
        )}
      </Collapsible>
      
      {/* History section - only shown for some roles */}
      {config.showHistory && (
        <Collapsible 
          title="History" 
          defaultExpanded={config.expandHistory}
        >
          <TaskHistoryTimeline task={task} />
        </Collapsible>
      )}
      
      {/* Comments section - adapted to role */}
      {config.showComments && (
        <div className="task-comments">
          <h3>Comments</h3>
          <TaskCommentList 
            comments={task.comments}
            showAuthorDetails={config.showCommentAuthorDetails}
          />
          
          {canComment && (
            <AddCommentForm onSubmit={handleAddComment} />
          )}
        </div>
      )}
      
      {/* Role-specific sections */}
      {userRole === 'caregiver' && (
        <CareContextSection task={task} />
      )}
      
      {userRole === 'organizer' && (
        <TeamContextSection task={task} />
      )}
      
      {userRole === 'care_recipient' && (
        <PrivacySection task={task} />
      )}
      
      {userRole === 'supporter' && (
        <EffortSection task={task} />
      )}
    </div>
  );
}
```

## 3. Technical Implementation

### 3.1 Data Model

The Task Management System builds on the data model extensions defined in `/07_Technical/Data_Models/data-model-extensions.md`, with these key structures:

```json
{
  "task": {
    "id": "task123",
    "title": "Pick up medications",
    "description": "Get refills from Walgreens on Main St",
    "category": "medication",
    "priority": "high",
    "due_date": "2025-04-15T15:00:00Z",
    "recurrence": {
      "pattern": "weekly",
      "days": ["monday"],
      "end_date": null,
      "count": null
    },
    "status": "assigned",
    "created_at": "2025-04-10T09:30:00Z",
    "created_by": {
      "user_id": "user123",
      "role": "caregiver"
    },
    "assigned_to": {
      "user_id": "user789",
      "role": "supporter",
      "accepted": true,
      "accepted_at": "2025-04-10T10:15:00Z"
    },
    "care_recipient": {
      "user_id": "recipient456"
    },
    "care_circle_id": "circle123",
    "subtasks": [
      {
        "id": "subtask1",
        "title": "Check prescription status",
        "completed": true
      },
      {
        "id": "subtask2",
        "title": "Bring insurance card",
        "completed": false
      }
    ],
    "comments": [
      {
        "id": "comment1",
        "content": "Called ahead, they'll have it ready",
        "author_id": "user123",
        "created_at": "2025-04-10T14:30:00Z"
      }
    ],
    "privacy_level": "circle",
    "visibility_overrides": [],
    "history": [
      {
        "action": "created",
        "actor_id": "user123",
        "timestamp": "2025-04-10T09:30:00Z"
      },
      {
        "action": "assigned",
        "actor_id": "user123",
        "timestamp": "2025-04-10T09:31:00Z",
        "details": {
          "assigned_to": "user789"
        }
      }
    ]
  }
}
```

### 3.2 State Management

```javascript
// Task state management (pseudocode)
const taskState = {
  // Lists of tasks
  tasks: {
    byId: {}, // Normalized task objects by ID
    allIds: [], // All task IDs
    // Caching filter results
    filterResults: {
      // e.g. 'all:incomplete:today'
      'filter:key': [] // Task IDs
    }
  },
  
  // UI state
  ui: {
    currentView: 'list', // list, board, calendar, agenda
    filters: {
      status: 'incomplete',
      assignee: 'all',
      timeframe: 'today',
      category: 'all'
    },
    sort: 'due_date_asc',
    group: 'none',
    selectedTaskId: null,
    editingTaskId: null
  },
  
  // Creation form state
  creation: {
    currentForm: null, // quick, standard, detailed
    values: {}, // Form values
    errors: {}, // Validation errors
    isSubmitting: false
  },
  
  // Assignment panel state
  assignment: {
    taskId: null,
    assigneeId: null,
    assignmentType: 'direct',
    message: '',
    isSubmitting: false
  }
};

// Task actions (pseudocode)
const taskActions = {
  // CRUD actions
  fetchTasks: (filters) => async (dispatch) => {
    dispatch({ type: 'FETCH_TASKS_START', payload: filters });
    try {
      const tasks = await api.getTasks(filters);
      dispatch({ type: 'FETCH_TASKS_SUCCESS', payload: tasks });
    } catch (error) {
      dispatch({ type: 'FETCH_TASKS_ERROR', payload: error });
    }
  },
  
  createTask: (taskData) => async (dispatch) => {
    dispatch({ type: 'CREATE_TASK_START', payload: taskData });
    try {
      const task = await api.createTask(taskData);
      dispatch({ type: 'CREATE_TASK_SUCCESS', payload: task });
      return task;
    } catch (error) {
      dispatch({ type: 'CREATE_TASK_ERROR', payload: error });
      throw error;
    }
  },
  
  updateTask: (taskId, changes) => async (dispatch) => {
    dispatch({ type: 'UPDATE_TASK_START', payload: { taskId, changes } });
    try {
      const task = await api.updateTask(taskId, changes);
      dispatch({ type: 'UPDATE_TASK_SUCCESS', payload: task });
      return task;
    } catch (error) {
      dispatch({ type: 'UPDATE_TASK_ERROR', payload: error });
      throw error;
    }
  },
  
  deleteTask: (taskId) => async (dispatch) => {
    dispatch({ type: 'DELETE_TASK_START', payload: taskId });
    try {
      await api.deleteTask(taskId);
      dispatch({ type: 'DELETE_TASK_SUCCESS', payload: taskId });
    } catch (error) {
      dispatch({ type: 'DELETE_TASK_ERROR', payload: error });
      throw error;
    }
  },
  
  // Task status actions
  completeTask: (taskId) => async (dispatch) => {
    dispatch({ type: 'COMPLETE_TASK_START', payload: taskId });
    try {
      const task = await api.completeTask(taskId);
      dispatch({ type: 'COMPLETE_TASK_SUCCESS', payload: task });
      return task;
    } catch (error) {
      dispatch({ type: 'COMPLETE_TASK_ERROR', payload: error });
      throw error;
    }
  },
  
  // Assignment actions
  assignTask: (taskId, assigneeId, options) => async (dispatch) => {
    dispatch({ 
      type: 'ASSIGN_TASK_START', 
      payload: { taskId, assigneeId, options } 
    });
    try {
      const task = await api.assignTask(taskId, assigneeId, options);
      dispatch({ type: 'ASSIGN_TASK_SUCCESS', payload: task });
      return task;
    } catch (error) {
      dispatch({ type: 'ASSIGN_TASK_ERROR', payload: error });
      throw error;
    }
  },
  
  // UI actions
  setTaskView: (viewType) => ({
    type: 'SET_TASK_VIEW',
    payload: viewType
  }),
  
  setTaskFilters: (filters) => ({
    type: 'SET_TASK_FILTERS',
    payload: filters
  }),
  
  setTaskSort: (sortOption) => ({
    type: 'SET_TASK_SORT',
    payload: sortOption
  }),
  
  setTaskGroup: (groupOption) => ({
    type: 'SET_TASK_GROUP',
    payload: groupOption
  }),
  
  selectTask: (taskId) => ({
    type: 'SELECT_TASK',
    payload: taskId
  }),
  
  startEditingTask: (taskId) => ({
    type: 'START_EDITING_TASK',
    payload: taskId
  }),
  
  cancelEditingTask: () => ({
    type: 'CANCEL_EDITING_TASK'
  })
};
```

### 3.3 API Integration

```javascript
// Task API integration (pseudocode)
const taskApi = {
  // Get tasks with filters
  getTasks: async (filters) => {
    const queryParams = new URLSearchParams();
    
    // Add filters to query params
    if (filters.status) queryParams.append('filter[status]', filters.status);
    if (filters.assignee) queryParams.append('filter[assignee]', filters.assignee);
    if (filters.timeframe) queryParams.append('filter[timeframe]', filters.timeframe);
    if (filters.category) queryParams.append('filter[category]', filters.category);
    
    // Add sort option
    if (filters.sort) queryParams.append('sort', filters.sort);
    
    // Include related data
    queryParams.append('include', 'assignee,creator,comments');
    
    const response = await api.get(`/tasks?${queryParams.toString()}`);
    return response.data;
  },
  
  // Get a single task
  getTask: async (taskId) => {
    const response = await api.get(`/tasks/${taskId}?include=assignee,creator,comments`);
    return response.data;
  },
  
  // Create a task
  createTask: async (taskData) => {
    const response = await api.post('/tasks', {
      data: taskData
    });
    return response.data;
  },
  
  // Update a task
  updateTask: async (taskId, changes) => {
    const response = await api.put(`/tasks/${taskId}`, {
      data: changes
    });
    return response.data;
  },
  
  // Delete a task
  deleteTask: async (taskId) => {
    await api.delete(`/tasks/${taskId}`);
    return true;
  },
  
  // Complete a task
  completeTask: async (taskId) => {
    const response = await api.post(`/tasks/${taskId}/complete`);
    return response.data;
  },
  
  // Assign a task
  assignTask: async (taskId, assigneeId, options = {}) => {
    const response = await api.post(`/tasks/${taskId}/assign`, {
      data: {
        assignee_id: assigneeId,
        assignment_type: options.assignmentType || 'direct',
        message: options.message || '',
        requires_acceptance: options.requiresAcceptance || false
      }
    });
    return response.data;
  },
  
  // Add a comment to a task
  addTaskComment: async (taskId, comment) => {
    const response = await api.post(`/tasks/${taskId}/comments`, {
      data: {
        content: comment
      }
    });
    return response.data;
  }
};
```

### 3.4 Permission Integration

```javascript
// Task permission hooks (pseudocode)
function useTaskPermissions(taskId) {
  const { checkPermission } = usePermissions();
  const [permissions, setPermissions] = useState({
    canView: false,
    canEdit: false,
    canAssign: false,
    canComplete: false,
    canDelete: false,
    canComment: false
  });
  
  useEffect(() => {
    const checkPermissions = async () => {
      const [
        canView,
        canEdit,
        canAssign,
        canComplete,
        canDelete,
        canComment
      ] = await Promise.all([
        checkPermission('view_task', taskId),
        checkPermission('edit_task', taskId),
        checkPermission('assign_task', taskId),
        checkPermission('complete_task', taskId),
        checkPermission('delete_task', taskId),
        checkPermission('comment_task', taskId)
      ]);
      
      setPermissions({
        canView,
        canEdit,
        canAssign,
        canComplete,
        canDelete,
        canComment
      });
    };
    
    if (taskId) {
      checkPermissions();
    }
  }, [taskId, checkPermission]);
  
  return permissions;
}

// Role-based permission configurations
const taskPermissionConfigs = {
  caregiver: {
    canCreateTasks: true,
    canEditAnyTask: true,
    canAssignTasks: true,
    canDeleteOwnTasks: true,
    canViewAllTasks: true
  },
  organizer: {
    canCreateTasks: true,
    canEditOwnTasks: true,
    canAssignTasks: true,
    canDeleteOwnTasks: true,
    canViewAllTasks: true
  },
  supporter: {
    canCreateTasks: false,
    canEditOwnTasks: false,
    canAssignTasks: false,
    canDeleteOwnTasks: false,
    canViewAssignedTasks: true,
    canCompleteAssignedTasks: true
  },
  care_recipient: {
    canCreateRequests: true,
    canEditOwnRequests: true,
    canViewOwnTasks: true,
    canCompleteOwnTasks: true
  }
};
```

## 4. Role-Specific Workflows

### 4.1 Caregiver Task Workflow

1. **Task Creation**
   - Access through quick add button or full form
   - Enter comprehensive task details
   - Assign to self or other care circle members
   - Set reminders and recurrence

2. **Task Assignment**
   - Assign tasks directly to any role
   - No acceptance required for most assignments
   - Include details and special instructions
   - Track assignment history

3. **Task Management**
   - View all tasks across care circle
   - Filter by assignee, status, category
   - Edit task details as needed
   - Receive notifications about status changes

4. **Task Completion**
   - Mark own tasks as complete
   - Verify completion of critical tasks
   - Add completion notes when necessary
   - View task history and patterns

### 4.2 Care Recipient Task Workflow

1. **Need Request Creation**
   - Create "need requests" through simplified form
   - Set privacy level for each request
   - Indicate preferred helper if desired
   - Add voice notes for details

2. **Task Visibility**
   - View tasks assigned to them
   - See who has been assigned their requests
   - View status of their requests
   - Control privacy settings for their needs

3. **Task Interaction**
   - Mark self-care tasks as complete
   - Request changes to assigned tasks
   - Add notes to existing tasks
   - Provide feedback on completed tasks

4. **Privacy Controls**
   - Set default privacy for all tasks
   - Override privacy for specific tasks
   - Control who can see task details
   - Monitor who has viewed sensitive tasks

### 4.3 Organizer Task Workflow

1. **Task Creation & Assignment**
   - Create team coordination tasks
   - Assign tasks to appropriate team members
   - Balance workload across team
   - Create task templates for common activities

2. **Team Management**
   - View tasks by assignee or category
   - Identify scheduling conflicts
   - Track task status across team
   - Reassign tasks when necessary

3. **Task Monitoring**
   - Track completion status
   - Follow up on overdue tasks
   - Identify patterns requiring attention
   - Generate reports on task completion

4. **Process Improvement**
   - Analyze task completion metrics
   - Optimize task assignments
   - Improve task descriptions
   - Create efficient workflows

### 4.4 Supporter Task Workflow

1. **Task Discovery**
   - View available task opportunities
   - Filter by time, type, and effort
   - Check compatibility with availability
   - Review task details before acceptance

2. **Task Acceptance**
   - Accept tasks that match availability
   - Request more information if needed
   - Confirm commitment
   - Review instructions and requirements

3. **Task Completion**
   - View assigned task details
   - Mark tasks as complete
   - Add completion notes
   - Request verification if needed

4. **Availability Management**
   - Update availability preferences
   - Sync with personal calendar
   - Set task type preferences
   - Manage commitment level

## 5. Testing & Validation

### 5.1 Functional Testing Requirements

| Test Area | Test Cases | Acceptance Criteria |
|-----------|------------|---------------------|
| **Task Creation** | Form validation, Role-specific fields, Template usage | All required fields validated, Role-appropriate forms, Templates correctly applied |
| **Task Assignment** | Assign to different roles, Self-assignment, Assignment notifications | Correct assignee saved, Proper notifications sent, Role permission boundaries respected |
| **Task List Views** | Filtering, Sorting, Grouping, Role-specific views | Filters apply correctly, Sort order maintained, Views adapt to role |
| **Task Detail** | Permission-based actions, Comment functionality, Status updates | Actions available based on role permissions, Comments save correctly, Status updates correctly |
| **Cross-Role Flows** | Task assignment flow, Status update flow, Comment flow | Tasks move correctly between roles, Updates visible to appropriate roles, Comments visible to appropriate roles |

### 5.2 Role Simulation Testing

```javascript
// Example role simulation test
test('Task creation adapts to role', async () => {
  // Test for Caregiver role
  const { getByText, getByLabelText } = render(
    <RoleContextProvider initialRole="caregiver">
      <TaskCreationForm />
    </RoleContextProvider>
  );
  
  // Verify caregiver sees all fields
  expect(getByLabelText(/title/i)).toBeInTheDocument();
  expect(getByLabelText(/description/i)).toBeInTheDocument();
  expect(getByLabelText(/due date/i)).toBeInTheDocument();
  expect(getByLabelText(/priority/i)).toBeInTheDocument();
  expect(getByLabelText(/category/i)).toBeInTheDocument();
  expect(getByLabelText(/assign to/i)).toBeInTheDocument();
  
  // Test for Care Recipient role
  cleanup();
  const { getByText: getRecipientText, getByLabelText: getRecipientLabel, queryByLabelText } = render(
    <RoleContextProvider initialRole="care_recipient">
      <TaskCreationForm />
    </RoleContextProvider>
  );
  
  // Verify care recipient sees simplified form
  expect(getRecipientLabel(/title/i)).toBeInTheDocument();
  expect(getRecipientLabel(/due date/i)).toBeInTheDocument();
  expect(queryByLabelText(/priority/i)).not.toBeInTheDocument();
  expect(getRecipientText(/privacy/i)).toBeInTheDocument();
  
  // Additional role tests...
});
```

### 5.3 Permission Testing

```javascript
// Example permission test
test('Task assignment respects permissions', async () => {
  // Mock task data
  const mockTask = { id: 'task123', title: 'Test Task' };
  
  // Mock permission service
  const mockPermissionService = {
    checkPermission: jest.fn()
  };
  
  // Test as Caregiver with permission
  mockPermissionService.checkPermission.mockResolvedValue(true);
  
  const { getByText } = render(
    <PermissionContext.Provider value={mockPermissionService}>
      <RoleContextProvider initialRole="caregiver">
        <TaskDetail task={mockTask} />
      </RoleContextProvider>
    </PermissionContext.Provider>
  );
  
  // Verify assign button exists for caregiver with permission
  expect(getByText(/assign/i)).toBeInTheDocument();
  
  // Test as Supporter without permission
  cleanup();
  mockPermissionService.checkPermission.mockResolvedValue(false);
  
  const { queryByText } = render(
    <PermissionContext.Provider value={mockPermissionService}>
      <RoleContextProvider initialRole="supporter">
        <TaskDetail task={mockTask} />
      </RoleContextProvider>
    </PermissionContext.Provider>
  );
  
  // Verify assign button does not exist for supporter
  expect(queryByText(/assign/i)).not.toBeInTheDocument();
});
```

### 5.4 Accessibility Testing

| Test Method | Focus Areas | Tools |
|-------------|-------------|-------|
| **Automated Testing** | ARIA roles, Labels, Contrast | Axe, Lighthouse |
| **Keyboard Navigation** | Tab order, Focus management | Manual testing |
| **Screen Reader** | Announcements, Form labels | NVDA, VoiceOver |
| **Touch Targets** | Size, Spacing | Manual testing |
| **Color Independence** | Information conveyed through multiple means | Visual review |

## 6. Emotional Intelligence Considerations

### 6.1 Role-Specific Emotional Design

| Role | Emotional Context | Design Approach | Language Principles |
|------|-------------------|-----------------|---------------------|
| **Caregiver** | May feel overwhelmed by tasks, responsible for outcomes | Emphasize organization, clear priorities, delegation options | Supportive, acknowledging effort, focusing on progress |
| **Care Recipient** | May feel loss of control, discomfort with dependence | Emphasize autonomy, request framing, privacy controls | Empowering, dignity-preserving, focusing on choice |
| **Organizer** | May feel pressure to coordinate effectively | Emphasize clear status, efficient assignment, team view | Clarity-focused, coordination-oriented, focusing on team alignment |
| **Supporter** | May worry about commitment level, ability to help effectively | Emphasize clear expectations, manageable scope, positive reinforcement | Appreciative, specific, focusing on contribution value |

### 6.2 Terminology Guidelines

| Term to Use | Instead of | Rationale |
|-------------|------------|-----------|
| "Need Request" | "Help Request" | Focuses on the need rather than the dependency |
| "Task" or "Activity" | "Chore" or "Duty" | Neutral framing without burden implication |
| "Assign" or "Match" | "Delegate" or "Hand off" | Implies shared responsibility rather than burden shifting |
| "Complete" | "Finish" or "Check off" | Emphasizes accomplishment and impact |
| "Care Activity" | "Work" or "Job" | Reinforces the care aspect of all tasks |

### 6.3 Error and Empty State Handling

| State | Emotional Approach | Message Examples |
|-------|-------------------|------------------|
| **No Tasks** | Positive, opportunity-focused | "Ready to start organizing care activities? Create your first task." |
| **Task Creation Error** | Solution-oriented, non-blaming | "We couldn't save your task. Let's try a simpler approach." |
| **Assignment Declined** | Understanding, alternatives-focused | "Sarah isn't available for this task. Here are other options." |
| **Overdue Tasks** | Supportive, non-judgmental | "Some tasks need attention when you're ready." |
| **Permission Denied** | Informative, redirecting | "This action isn't available. Here's what you can do instead." |

## 7. Implementation Plan

### 7.1 Development Phases

| Phase | Focus | Timeline | Dependencies |
|-------|-------|----------|--------------|
| **Phase 1: Core Components** | Task data model, Basic task list, Task detail | Week 6, Days 1-2 | Data model extensions, Permission framework |
| **Phase 2: Creation & Assignment** | Task creation forms, Assignment workflow | Week 6, Days 3-4 | Core components |
| **Phase 3: List Views & Filters** | Task list views, Filtering, Sorting | Week 6, Day 5 - Week 7, Day 1 | Core components |
| **Phase 4: Role Adaptations** | Role-specific workflows, UI adaptations | Week 7, Days 2-3 | All previous phases |
| **Phase 5: Integration** | Notifications, Calendar integration, API hooks | Week 7, Days 4-5 | All previous phases |

### 7.2 Key Milestones

1. Task data model and API integration complete
2. Basic CRUD operations functional for all roles
3. Task list views with role adaptations implemented
4. Task assignment workflow functional
5. Role-specific task workflows complete
6. Full integration with notification system

### 7.3 Dependencies

| Component | Dependencies | Risk Factors |
|-----------|--------------|--------------|
| **Task Data Model** | Data model extensions | Low - extensions already defined |
| **Task Creation** | Permission framework | Medium - relies on permission checks |
| **Task Assignment** | User/Circle data, Permission framework | Medium - complex workflow |
| **Role Adaptations** | All previous components | High - requires thorough testing |
| **Integration** | Notification system, Calendar components | High - external dependencies |

## 8. Acceptance Criteria

### 8.1 Functional Requirements

- ✅ Task creation with role-appropriate forms
- ✅ Task assignment with proper permissions
- ✅ Task list views with filtering, sorting, and grouping
- ✅ Task detail view with role-appropriate actions
- ✅ Task completion workflow
- ✅ Task comment and update system
- ✅ Cross-role task visibility respecting privacy settings

### 8.2 Performance Requirements

- ✅ Task list loads within 500ms with 100 tasks
- ✅ Task creation completes within 300ms
- ✅ Task assignment completes within 300ms
- ✅ Task filters apply within 100ms
- ✅ Smooth scrolling in task lists

### 8.3 Accessibility Requirements

- ✅ All components pass WCAG 2.1 AA compliance checks
- ✅ Task forms accessible via keyboard
- ✅ Screen readers announce task status changes
- ✅ Focus management maintains context during workflows
- ✅ Color not used as only indicator of status

### 8.4 Cross-Role Requirements

- ✅ Task workflows functional across all roles
- ✅ Permission boundaries respected in all views
- ✅ Privacy settings correctly applied to task visibility
- ✅ Role-appropriate language and UI elements
- ✅ Consistent task representation across roles

## 9. Change Log

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0.0 | 2025-04-14 | Product Lead | Initial specification |
