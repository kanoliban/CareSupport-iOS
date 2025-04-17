# Communication Hub Specification

**Version:** 1.0.0  
**Last Updated:** 2025-04-14  
**Status:** Draft  
**Document Location:** `/06_Core_Platform/Communication_Hub/Communication_Hub_Specification.md`

## 1. Overview

### 1.1 Purpose

The Communication Hub serves as the central messaging and update system for the CareSupport platform, facilitating information exchange and connection across all care circle roles. This module enables transparent, privacy-aware communication that adapts to each role's needs while maintaining appropriate boundaries. The Communication Hub transforms dispersed communication into a unified, role-appropriate experience that reduces coordination overhead, increases awareness, and strengthens relationships while respecting privacy preferences.

### 1.2 Related Documents

- `/06_Core_Platform/Task_Management/Task_Management_Specification.md` - Task system integration
- `/06_Core_Platform/Care_Coordination/Care_Coordination_Specification.md` - Care coordination integration
- `/06_Core_Platform/Post_Setup_Transition/Welcome_Home_Specification.md` - Post-setup experience
- `/02_Research/Relationship_Model.md` - Role relationships and interaction patterns
- `/03_Information_Architecture/Navigation_System.md` - Navigation framework
- `/07_Technical/Data_Models/data-model-extensions.md` - Core data structures
- `/07_Technical/API_Requirements/Core_API_Specs.md` - API endpoints
- `/07_Technical/Implementation_Guides/Permission_Framework.md` - Permission implementation
- `/07_Technical/Implementation_Guides/State_Management_System.md` - State management approach

### 1.3 Module Goals

- Provide a unified communication platform that adapts to each role's communication needs
- Enable transparent information sharing while respecting privacy boundaries
- Reduce coordination overhead through centralized updates and notifications
- Strengthen care circle connections through appropriate communication channels
- Support both structured (updates, reports) and conversational communication
- Integrate with task, schedule, and health tracking for contextual communication
- Preserve care recipient dignity and autonomy through privacy controls

## 2. Core Components

### 2.1 Update Feed Components

#### 2.1.1 Purpose
Provides a centralized, chronological view of important care-related information, status changes, and updates, filtered appropriately for each role.

#### 2.1.2 Technical Specifications

| Attribute | Specification |
|-----------|---------------|
| **Update Types** | Status, Task, Event, Health, General, Need Expression |
| **Content Formats** | Text, Images, Voice Messages, Structured Data |
| **Filtering Options** | By type, author, date, relevance, privacy level |
| **Sorting Options** | Chronological, Priority, Relevance |
| **Privacy Controls** | Per-update visibility, Role-based access |

#### 2.1.3 Component Structure

```json
{
  "update_feed": {
    "update_types": {
      "status_update": {
        "icon": "status-icon",
        "color": "#4CAF50",
        "template": "Status update for {care_recipient_name}",
        "data_fields": ["status", "observer", "timestamp"]
      },
      "task_update": {
        "icon": "task-icon",
        "color": "#2196F3",
        "template": "Task {status}: {task_name}",
        "data_fields": ["task_id", "status", "actor", "timestamp"]
      },
      "event_update": {
        "icon": "event-icon",
        "color": "#9C27B0",
        "template": "Event {status}: {event_name}",
        "data_fields": ["event_id", "status", "actor", "timestamp"]
      },
      "health_update": {
        "icon": "health-icon",
        "color": "#F44336",
        "template": "Health update: {metric_name} {value}",
        "data_fields": ["metric", "value", "recorder", "timestamp"]
      },
      "general_update": {
        "icon": "general-icon",
        "color": "#FF9800",
        "template": "Update from {author_name}",
        "data_fields": ["content", "author", "timestamp"]
      },
      "need_expression": {
        "icon": "need-icon",
        "color": "#607D8B",
        "template": "Need expressed: {need_description}",
        "data_fields": ["need_type", "description", "timestamp"]
      }
    },
    "filter_options": {
      "type": ["all", "status", "task", "event", "health", "general", "need"],
      "author": ["all", "me", "[specific_user]"],
      "timeframe": ["all", "today", "yesterday", "this_week", "custom"],
      "privacy": ["all", "public", "shared_with_me"]
    },
    "sort_options": ["newest", "oldest", "priority", "relevance"]
  }
}
```

#### 2.1.4 Role Adaptations

| Role | Update Feed Focus | Privacy Capabilities | Action Capabilities | Special Features |
|------|------------------|----------------------|---------------------|-----------------|
| **Caregiver** | Comprehensive feed | Full visibility (per Care Recipient settings) | Create all update types, React, Comment | Priority highlighting, Critical alerts, History access |
| **Care Recipient** | Filtered personal feed | Full privacy control | Create updates, Control sharing | Privacy indicators, Sharing preferences, Need expression |
| **Organizer** | Coordination-focused feed | Team coordination visibility | Create coordination updates | Team mentions, Task references, Schedule integration |
| **Supporter** | Task-relevant updates | Limited to shared content | Create task-specific updates | Simple task-focused view, Relevant updates only |

#### 2.1.5 Implementation Example

```jsx
// Update feed component with role adaptations
function UpdateFeed({ userRole }) {
  // Get role-specific configuration
  const config = useUpdateFeedConfig(userRole);
  
  // State for current filters and sorting
  const [filters, setFilters] = useState({
    type: config.defaultFilters.type,
    author: config.defaultFilters.author,
    timeframe: config.defaultFilters.timeframe,
    privacy: config.defaultFilters.privacy
  });
  const [sortOption, setSortOption] = useState(config.defaultSortOption);
  
  // Load updates with permission filtering
  const { updates, loading, error } = useUpdates(filters, sortOption);
  
  // Handle update creation
  const handleCreateUpdate = (updateType) => {
    if (config.canCreateUpdateTypes.includes(updateType)) {
      openUpdateCreationModal(updateType);
    }
  };
  
  // Handle filter changes
  const handleFilterChange = (filterType, value) => {
    setFilters({
      ...filters,
      [filterType]: value
    });
  };
  
  // Handle sort changes
  const handleSortChange = (option) => {
    setSortOption(option);
  };
  
  if (loading) return <LoadingUpdates />;
  if (error) return <ErrorDisplay error={error} />;
  
  return (
    <div className="update-feed">
      <header className="update-feed-header">
        <h2>{config.title}</h2>
        
        <div className="update-feed-controls">
          {/* Filter controls - adapted to role */}
          <FilterControls
            filters={filters}
            onChange={handleFilterChange}
            options={config.filterOptions}
          />
          
          {/* Sort selector */}
          <SortSelector
            value={sortOption}
            onChange={handleSortChange}
            options={config.sortOptions}
          />
          
          {/* Create update button - only for permitted types */}
          {config.canCreateUpdateTypes.length > 0 && (
            <CreateUpdateButton 
              onCreateUpdate={handleCreateUpdate}
              availableTypes={config.canCreateUpdateTypes}
            />
          )}
          
          {/* Role-specific controls */}
          {userRole === 'care_recipient' && (
            <PrivacyControl 
              onPrivacyChange={handlePrivacyChange} 
            />
          )}
        </div>
      </header>
      
      {/* Update list */}
      <div className="update-list">
        {updates.length === 0 ? (
          <EmptyState
            message={config.emptyStateMessage}
            action={config.canCreateUpdateTypes.length > 0 ? {
              label: "Create Update",
              onClick: () => handleCreateUpdate(config.canCreateUpdateTypes[0])
            } : null}
          />
        ) : (
          updates.map(update => (
            <UpdateCard
              key={update.id}
              update={update}
              userRole={userRole}
              onReact={config.canReact ? handleReact : undefined}
              onComment={config.canComment ? handleComment : undefined}
              onShare={config.canShare ? handleShare : undefined}
              privacyLevel={update.privacy_level}
              showAuthorDetails={config.showAuthorDetails}
              detailLevel={config.detailLevel}
            />
          ))
        )}
      </div>
      
      {/* Load more button - only if there are more updates */}
      {updates.hasMore && (
        <Button onClick={handleLoadMore}>
          {config.loadMoreButtonText}
        </Button>
      )}
    </div>
  );
}

// Update card component with role adaptations
function UpdateCard({ update, userRole, onReact, onComment, onShare, privacyLevel, showAuthorDetails, detailLevel }) {
  // Get update type configuration
  const updateTypeConfig = useUpdateTypeConfig(update.type);
  
  // State for expanded view
  const [expanded, setExpanded] = useState(false);
  
  // Handle toggle expanded
  const handleToggleExpanded = () => {
    setExpanded(!expanded);
  };
  
  return (
    <Card 
      className={`update-card update-type-${update.type}`}
      style={{ borderLeftColor: updateTypeConfig.color }}
    >
      <div className="update-header">
        {/* Update type icon */}
        <div 
          className="update-icon"
          style={{ backgroundColor: updateTypeConfig.color }}
        >
          <Icon name={updateTypeConfig.icon} />
        </div>
        
        {/* Update title and metadata */}
        <div className="update-metadata">
          <div className="update-title">
            {formatUpdateTitle(update, updateTypeConfig.template)}
          </div>
          
          <div className="update-info">
            {showAuthorDetails && (
              <span className="update-author">
                <UserChip user={update.author} size="small" />
              </span>
            )}
            
            <span className="update-time">
              {formatRelativeTime(update.timestamp)}
            </span>
            
            {/* Privacy indicator */}
            <PrivacyBadge level={privacyLevel} />
          </div>
        </div>
      </div>
      
      {/* Update content - detail level varies by role */}
      <div className="update-content">
        {detailLevel === 'full' && (
          <div className="update-body-full">
            {renderUpdateContent(update, 'full')}
          </div>
        )}
        
        {detailLevel === 'standard' && (
          <div className="update-body-standard">
            {renderUpdateContent(update, 'standard')}
          </div>
        )}
        
        {detailLevel === 'limited' && (
          <div className="update-body-limited">
            {renderUpdateContent(update, 'limited')}
          </div>
        )}
        
        {/* Media attachments - if present and allowed for role */}
        {update.media && update.media.length > 0 && 
         hasMediaPermission(userRole, update) && (
          <div className="update-media">
            <MediaGallery media={update.media} />
          </div>
        )}
      </div>
      
      {/* Update actions - based on permissions */}
      <div className="update-actions">
        {onReact && (
          <ReactionButtons 
            reactions={update.reactions}
            onReact={(reaction) => onReact(update.id, reaction)}
          />
        )}
        
        {expanded && onComment && (
          <CommentSection 
            comments={update.comments}
            onAddComment={(comment) => onComment(update.id, comment)}
          />
        )}
        
        <div className="update-action-buttons">
          {onComment && (
            <Button 
              onClick={handleToggleExpanded}
              icon={expanded ? "comment-hide" : "comment-show"}
            >
              {expanded ? "Hide Comments" : "Comments"}
              {!expanded && update.comments.length > 0 && (
                <Badge>{update.comments.length}</Badge>
              )}
            </Button>
          )}
          
          {onShare && (
            <Button 
              onClick={() => onShare(update.id)}
              icon="share"
            >
              Share
            </Button>
          )}
        </div>
      </div>
    </Card>
  );
}
```

### 2.2 Messaging Components

#### 2.2.1 Purpose
Enables direct communication between care circle members, providing role-appropriate messaging capabilities while respecting privacy boundaries and contextual relationships.

#### 2.2.2 Technical Specifications

| Attribute | Specification |
|-----------|---------------|
| **Message Types** | Text, Image, Voice, Structured (Task/Event Reference) |
| **Conversation Types** | One-to-one, Group, Care Team, Broadcast |
| **Message Features** | Read receipts, Delivery status, Context links |
| **Privacy Controls** | Role-appropriate conversations, Content filtering |
| **Integration Points** | Task references, Event references, Health references |

#### 2.2.3 Component Structure

```json
{
  "messaging": {
    "conversation_types": {
      "direct": {
        "participants": 2,
        "features": ["read_receipts", "typing_indicators", "attachments"]
      },
      "group": {
        "min_participants": 3,
        "max_participants": 20,
        "features": ["read_receipts", "typing_indicators", "attachments", "group_management"]
      },
      "care_team": {
        "participants": "all_caregivers_and_organizers",
        "features": ["read_receipts", "typing_indicators", "attachments", "pinned_messages"]
      },
      "broadcast": {
        "sender_roles": ["caregiver", "organizer"],
        "recipient_roles": ["all"],
        "features": ["delivery_confirmation", "scheduled_send"]
      }
    },
    "message_types": {
      "text": {
        "max_length": 2000,
        "formatting": ["bold", "italic", "list"]
      },
      "image": {
        "max_size": "10MB",
        "allowed_formats": ["jpg", "png", "gif"]
      },
      "voice": {
        "max_duration": "3 minutes",
        "format": "m4a"
      },
      "task_reference": {
        "fields": ["task_id", "title", "status", "due_date"]
      },
      "event_reference": {
        "fields": ["event_id", "title", "start_time", "location"]
      },
      "health_reference": {
        "fields": ["metric", "value", "timestamp"],
        "roles_with_access": ["caregiver", "care_recipient"]
      }
    }
  }
}
```

#### 2.2.4 Role Adaptations

| Role | Conversation Access | Message Capabilities | Privacy Controls | Special Features |
|------|---------------------|---------------------|------------------|-----------------|
| **Caregiver** | All conversation types | All message types | Full visibility (per Care Recipient settings) | Care coordination context, Health references, Task coordination |
| **Care Recipient** | Direct, Group, Care Team (if included) | All message types | Full privacy control | Privacy indicators, Caregiver priority, Need expression |
| **Organizer** | Direct, Group, Care Team, Broadcast | Text, Image, Voice, Task/Event References | Team coordination visibility | Team broadcasts, Schedule coordination |
| **Supporter** | Direct, Group (if included) | Text, Image, Voice | Limited to contextual conversations | Task-specific chat, Availability updates |

#### 2.2.5 Implementation Example

```jsx
// Messaging component with role adaptations
function MessagingCenter({ userRole }) {
  // Get role-specific configuration
  const config = useMessagingConfig(userRole);
  
  // State for selected conversation and new message
  const [selectedConversationId, setSelectedConversationId] = useState(null);
  const [newMessage, setNewMessage] = useState('');
  
  // Load conversations with permission filtering
  const { conversations, loading, error } = useConversations();
  
  // Get selected conversation
  const selectedConversation = conversations.find(
    conv => conv.id === selectedConversationId
  );
  
  // Load messages for selected conversation
  const { messages, loadingMessages, loadMoreMessages, hasMoreMessages } = 
    useMessages(selectedConversationId);
  
  // Handle conversation selection
  const handleSelectConversation = (conversationId) => {
    setSelectedConversationId(conversationId);
  };
  
  // Handle creating new conversation
  const handleCreateConversation = () => {
    if (config.canCreateConversations) {
      openCreateConversationModal();
    }
  };
  
  // Handle sending message
  const handleSendMessage = () => {
    if (!newMessage.trim()) return;
    
    sendMessage(selectedConversationId, {
      type: 'text',
      content: newMessage,
      timestamp: new Date()
    });
    
    setNewMessage('');
  };
  
  if (loading) return <LoadingConversations />;
  if (error) return <ErrorDisplay error={error} />;
  
  return (
    <div className="messaging-center">
      <div className="conversation-sidebar">
        <header className="conversation-sidebar-header">
          <h2>{config.conversationListTitle}</h2>
          
          {/* Create conversation button - only for roles with permission */}
          {config.canCreateConversations && (
            <Button 
              onClick={handleCreateConversation}
              icon="create-conversation"
            >
              {config.createConversationButtonText}
            </Button>
          )}
        </header>
        
        {/* Conversation list */}
        <div className="conversation-list">
          {conversations.length === 0 ? (
            <EmptyState
              message={config.emptyConversationsMessage}
              action={config.canCreateConversations ? {
                label: config.createConversationButtonText,
                onClick: handleCreateConversation
              } : null}
            />
          ) : (
            conversations.map(conversation => (
              <ConversationListItem
                key={conversation.id}
                conversation={conversation}
                isSelected={conversation.id === selectedConversationId}
                onClick={() => handleSelectConversation(conversation.id)}
                userRole={userRole}
              />
            ))
          )}
        </div>
      </div>
      
      <div className="message-area">
        {!selectedConversation ? (
          <div className="no-conversation-selected">
            <p>{config.noConversationSelectedMessage}</p>
          </div>
        ) : (
          <>
            {/* Conversation header */}
            <header className="conversation-header">
              <div className="conversation-info">
                <h3>{getConversationName(selectedConversation)}</h3>
                <div className="conversation-participants">
                  {renderParticipants(selectedConversation.participants)}
                </div>
              </div>
              
              {/* Conversation actions */}
              <div className="conversation-actions">
                {config.canManageConversations && (
                  <Button 
                    onClick={() => handleManageConversation(selectedConversation.id)}
                    icon="manage-conversation"
                  >
                    Manage
                  </Button>
                )}
              </div>
            </header>
            
            {/* Message list */}
            <div className="message-list">
              {loadingMessages ? (
                <LoadingMessages />
              ) : messages.length === 0 ? (
                <EmptyState
                  message={config.emptyMessagesMessage}
                />
              ) : (
                <>
                  {hasMoreMessages && (
                    <Button 
                      onClick={loadMoreMessages}
                      className="load-more-messages"
                    >
                      Load Earlier Messages
                    </Button>
                  )}
                  
                  {messages.map(message => (
                    <MessageBubble
                      key={message.id}
                      message={message}
                      isCurrentUser={message.sender.id === currentUserId}
                      userRole={userRole}
                      showSenderDetails={selectedConversation.type !== 'direct'}
                    />
                  ))}
                </>
              )}
            </div>
            
            {/* Message composer - adapted to role permissions */}
            <div className="message-composer">
              <div className="composer-input">
                <textarea
                  value={newMessage}
                  onChange={(e) => setNewMessage(e.target.value)}
                  placeholder={config.messageInputPlaceholder}
                  disabled={!config.canSendMessages}
                ></textarea>
              </div>
              
              <div className="composer-actions">
                {/* Attachment buttons - only for allowed types */}
                {config.allowedAttachmentTypes.includes('image') && (
                  <Button 
                    onClick={handleAttachImage}
                    icon="image"
                    aria-label="Attach Image"
                  />
                )}
                
                {config.allowedAttachmentTypes.includes('voice') && (
                  <Button 
                    onClick={handleRecordVoice}
                    icon="microphone"
                    aria-label="Record Voice Message"
                  />
                )}
                
                {/* Reference buttons - only for certain roles */}
                {userRole === 'caregiver' && (
                  <>
                    <Button 
                      onClick={handleAddTaskReference}
                      icon="task"
                      aria-label="Add Task Reference"
                    />
                    
                    <Button 
                      onClick={handleAddEventReference}
                      icon="event"
                      aria-label="Add Event Reference"
                    />
                  </>
                )}
                
                {/* Send button */}
                <Button 
                  onClick={handleSendMessage}
                  icon="send"
                  disabled={!newMessage.trim() || !config.canSendMessages}
                  primary
                >
                  Send
                </Button>
              </div>
            </div>
          </>
        )}
      </div>
    </div>
  );
}

// Conversation list item component
function ConversationListItem({ conversation, isSelected, onClick, userRole }) {
  // Get latest message preview
  const latestMessage = conversation.latest_message;
  
  // Format participants for display
  const participantsText = formatParticipants(conversation.participants, userRole);
  
  return (
    <div 
      className={`conversation-list-item ${isSelected ? 'selected' : ''}`}
      onClick={onClick}
    >
      {/* Conversation avatar */}
      {conversation.type === 'direct' ? (
        <UserAvatar user={getOtherParticipant(conversation.participants)} />
      ) : (
        <GroupAvatar participants={conversation.participants} type={conversation.type} />
      )}
      
      <div className="conversation-details">
        {/* Conversation name */}
        <div className="conversation-name">
          {getConversationName(conversation)}
        </div>
        
        {/* Participants summary - for group chats */}
        {conversation.type !== 'direct' && (
          <div className="conversation-participants-summary">
            {participantsText}
          </div>
        )}
        
        {/* Latest message preview */}
        {latestMessage && (
          <div className="conversation-preview">
            {conversation.type !== 'direct' && (
              <span className="preview-sender">
                {latestMessage.sender.first_name}:
              </span>
            )}
            <span className="preview-content">
              {formatMessagePreview(latestMessage, userRole)}
            </span>
            <span className="preview-time">
              {formatShortTime(latestMessage.timestamp)}
            </span>
          </div>
        )}
      </div>
      
      {/* Unread indicator and count */}
      {conversation.unread_count > 0 && (
        <Badge className="unread-badge">
          {conversation.unread_count}
        </Badge>
      )}
    </div>
  );
}

// Message bubble component
function MessageBubble({ message, isCurrentUser, userRole, showSenderDetails }) {
  // Determine if message content is viewable based on role permissions
  const canViewContent = canViewMessageContent(message, userRole);
  
  return (
    <div className={`message-bubble ${isCurrentUser ? 'current-user' : 'other-user'}`}>
      {/* Sender info - only shown in group chats */}
      {showSenderDetails && !isCurrentUser && (
        <div className="message-sender">
          <UserChip user={message.sender} size="small" />
        </div>
      )}
      
      {/* Message content */}
      <div className="message-content">
        {!canViewContent ? (
          <div className="private-content">
            <PrivacyIndicator />
            <span>This message has privacy restrictions</span>
          </div>
        ) : message.type === 'text' ? (
          <div className="text-message">
            {message.content}
          </div>
        ) : message.type === 'image' ? (
          <div className="image-message">
            <img src={message.content.url} alt="Shared image" />
          </div>
        ) : message.type === 'voice' ? (
          <div className="voice-message">
            <AudioPlayer src={message.content.url} duration={message.content.duration} />
          </div>
        ) : message.type === 'task_reference' ? (
          <div className="task-reference">
            <TaskReferenceCard task={message.content.task} />
          </div>
        ) : message.type === 'event_reference' ? (
          <div className="event-reference">
            <EventReferenceCard event={message.content.event} />
          </div>
        ) : message.type === 'health_reference' ? (
          <div className="health-reference">
            <HealthReferenceCard healthData={message.content.health_data} />
          </div>
        ) : (
          <div className="unknown-message-type">
            Unsupported message type
          </div>
        )}
        
        {/* Message metadata */}
        <div className="message-metadata">
          <span className="message-time">
            {formatTime(message.timestamp)}
          </span>
          
          {message.delivery_status && isCurrentUser && (
            <DeliveryStatus status={message.delivery_status} />
          )}
        </div>
      </div>
    </div>
  );
}
```

### 2.3 Need Expression Components

#### 2.3.1 Purpose
Provides an empowering way for Care Recipients to express needs and request assistance while preserving dignity and autonomy, with appropriate response mechanisms for other roles.

#### 2.3.2 Technical Specifications

| Attribute | Specification |
|-----------|---------------|
| **Need Types** | Physical Assistance, Social, Emotional, Practical, Medical |
| **Expression Methods** | Structured Requests, Free Text, Voice Recording, Quick Buttons |
| **Urgency Levels** | When Convenient, Today, As Soon As Possible, Urgent |
| **Visibility Controls** | Public to Circle, Specific Individuals Only, Privacy Override |
| **Response Options** | Acknowledge, Volunteer, Assign, Schedule, Decline |

#### 2.3.3 Component Structure

```json
{
  "need_expression": {
    "need_types": {
      "physical_assistance": {
        "icon": "helping-hand",
        "color": "#4CAF50",
        "examples": ["Help with mobility", "Assistance with personal care", "Help with household tasks"]
      },
      "social": {
        "icon": "people",
        "color": "#2196F3",
        "examples": ["Company for a meal", "Conversation", "Outing companion"]
      },
      "emotional": {
        "icon": "heart",
        "color": "#E91E63",
        "examples": ["Someone to talk to", "Reassurance", "Emotional support"]
      },
      "practical": {
        "icon": "tools",
        "color": "#FF9800",
        "examples": ["Transportation", "Shopping", "Administrative help"]
      },
      "medical": {
        "icon": "medical",
        "color": "#F44336",
        "examples": ["Medication assistance", "Medical appointment help", "Health monitoring"]
      }
    },
    "urgency_levels": {
      "when_convenient": {
        "icon": "calendar",
        "color": "#8BC34A",
        "timeframe": "Flexible timing"
      },
      "today": {
        "icon": "today",
        "color": "#FFC107",
        "timeframe": "Sometime today"
      },
      "as_soon_as_possible": {
        "icon": "clock",
        "color": "#FF9800",
        "timeframe": "Within a few hours"
      },
      "urgent": {
        "icon": "alert",
        "color": "#F44336",
        "timeframe": "Immediate attention"
      }
    },
    "response_types": {
      "acknowledge": {
        "icon": "checkmark",
        "message": "I've seen this request"
      },
      "volunteer": {
        "icon": "hand-raised",
        "message": "I can help with this"
      },
      "assign": {
        "icon": "assign",
        "message": "I've assigned someone to help",
        "roles": ["caregiver", "organizer"]
      },
      "schedule": {
        "icon": "calendar-add",
        "message": "I've scheduled time for this",
        "roles": ["caregiver", "organizer", "supporter"]
      },
      "decline": {
        "icon": "decline",
        "message": "I'm not available to help",
        "requires_reason": true
      }
    }
  }
}
```

#### 2.3.4 Role Adaptations

| Role | Expression Capabilities | Visibility | Response Capabilities | Special Features |
|------|------------------------|------------|----------------------|-----------------|
| **Caregiver** | Submit on behalf (with consent) | Full visibility | All response types | Convert to task, Priority management, Delegation |
| **Care Recipient** | All need types, All expression methods | Full control | Acknowledge, Close request | Privacy controls, Preferred helper selection, Dignified interface |
| **Organizer** | None (coordination only) | Per Care Recipient settings | Acknowledge, Volunteer, Assign, Schedule, Decline | Team assignment, Scheduling integration |
| **Supporter** | None | Only requests shared with them | Acknowledge, Volunteer, Decline | Simple volunteer interface, Availability integration |

#### 2.3.5 Implementation Example

```jsx
// Need expression component for Care Recipient
function NeedExpressionForm({ onSubmit }) {
  // State for need request
  const [needType, setNeedType] = useState('');
  const [needDescription, setNeedDescription] = useState('');
  const [urgencyLevel, setUrgencyLevel] = useState('when_convenient');
  const [preferredHelpers, setPreferredHelpers] = useState([]);
  const [privacyLevel, setPrivacyLevel] = useState('circle');
  
  // Get need types and urgency levels
  const { needTypes, urgencyLevels } = useNeedExpressionConfig();
  
  // Get care circle members for preferred helpers
  const { careCircleMembers } = useCareCircle();
  
  // Handle form submission
  const handleSubmit = (e) => {
    e.preventDefault();
    
    onSubmit({
      need_type: needType,
      description: needDescription,
      urgency_level: urgencyLevel,
      preferred_helpers: preferredHelpers,
      privacy_level: privacyLevel,
      timestamp: new Date()
    });
    
    // Clear form
    setNeedType('');
    setNeedDescription('');
    setUrgencyLevel('when_convenient');
    setPreferredHelpers([]);
  };
  
  // Handle voice input for description
  const handleVoiceInput = async () => {
    try {
      const text = await startVoiceRecognition();
      setNeedDescription(prevText => prevText ? `${prevText} ${text}` : text);
    } catch (error) {
      // Handle voice recognition errors
    }
  };
  
  return (
    <form className="need-expression-form" onSubmit={handleSubmit}>
      <h2>Express a Need</h2>
      
      {/* Need type selection */}
      <div className="need-type-selection">
        <label>What type of help do you need?</label>
        <div className="need-type-options">
          {Object.entries(needTypes).map(([type, config]) => (
            <Button
              key={type}
              className={`need-type-button ${needType === type ? 'selected' : ''}`}
              onClick={() => setNeedType(type)}
              style={{
                borderColor: needType === type ? config.color : undefined,
                backgroundColor: needType === type ? `${config.color}20` : undefined
              }}
            >
              <Icon name={config.icon} color={config.color} />
              <span>{formatNeedTypeName(type)}</span>
            </Button>
          ))}
        </div>
      </div>
      
      {/* Need description */}
      <div className="need-description">
        <label htmlFor="need-description">Describe what you need:</label>
        <div className="input-with-voice">
          <textarea
            id="need-description"
            value={needDescription}
            onChange={(e) => setNeedDescription(e.target.value)}
            placeholder="Please describe what you need assistance with..."
            required
          ></textarea>
          <VoiceInputButton onClick={handleVoiceInput} />
        </div>
        
        {/* Example suggestions - based on selected need type */}
        {needType && (
          <div className="need-examples">
            <p>Examples:</p>
            <ul>
              {needTypes[needType].examples.map((example, index) => (
                <li key={index}>
                  <Button
                    className="example-button"
                    onClick={() => setNeedDescription(example)}
                  >
                    {example}
                  </Button>
                </li>
              ))}
            </ul>
          </div>
        )}
      </div>
      
      {/* Urgency level selection */}
      <div className="urgency-selection">
        <label>How urgent is this need?</label>
        <div className="urgency-options">
          {Object.entries(urgencyLevels).map(([level, config]) => (
            <Button
              key={level}
              className={`urgency-button ${urgencyLevel === level ? 'selected' : ''}`}
              onClick={() => setUrgencyLevel(level)}
              style={{
                borderColor: urgencyLevel === level ? config.color : undefined,
                backgroundColor: urgencyLevel === level ? `${config.color}20` : undefined
              }}
            >
              <Icon name={config.icon} color={config.color} />
              <span>{formatUrgencyLevelName(level)}</span>
              <small>{config.timeframe}</small>
            </Button>
          ))}
        </div>
      </div>
      
      {/* Preferred helpers selection */}
      <div className="preferred-helpers">
        <label>Who would you prefer to help with this? (Optional)</label>
        <div className="helper-selection">
          <MemberSelector
            members={careCircleMembers}
            selected={preferredHelpers}
            onChange={setPreferredHelpers}
            placeholder="Select preferred helpers (optional)"
            multiSelect={true}
          />
        </div>
      </div>
      
      {/* Privacy controls */}
      <div className="privacy-controls">
        <label>Who can see this request?</label>
        <PrivacyLevelSelector
          value={privacyLevel}
          onChange={setPrivacyLevel}
        />
      </div>
      
      {/* Submit button */}
      <div className="form-actions">
        <Button type="submit" primary>
          Send Request
        </Button>
      </div>
    </form>
  );
}

// Need request component for responding to needs
function NeedRequestCard({ request, userRole, onRespond }) {
  // Get configuration based on role
  const config = useNeedRequestConfig(userRole);
  
  // Get response types from config
  const { responseTypes } = useNeedExpressionConfig();
  
  // Check if user can view this request based on privacy and role
  const canViewRequest = checkNeedRequestPermission(request, userRole);
  
  // State for response reason (if declining)
  const [declineReason, setDeclineReason] = useState('');
  const [isResponding, setIsResponding] = useState(false);
  const [selectedResponse, setSelectedResponse] = useState(null);
  
  // Handle response selection
  const handleSelectResponse = (responseType) => {
    setSelectedResponse(responseType);
    
    if (responseType === 'decline') {
      setIsResponding(true);
    } else if (responseType === 'assign' && userRole === 'organizer') {
      setIsResponding(true);
    } else {
      handleSubmitResponse(responseType);
    }
  };
  
  // Handle response submission
  const handleSubmitResponse = (responseType, details = {}) => {
    onRespond(request.id, {
      response_type: responseType,
      timestamp: new Date(),
      ...details
    });
    
    setIsResponding(false);
    setSelectedResponse(null);
    setDeclineReason('');
  };
  
  if (!canViewRequest) {
    return (
      <Card className="need-request-card private">
        <PrivacyIndicator />
        <p>This request has privacy restrictions</p>
      </Card>
    );
  }
  
  // Get need type and urgency level configuration
  const needTypeConfig = getNeedTypeConfig(request.need_type);
  const urgencyLevelConfig = getUrgencyLevelConfig(request.urgency_level);
  
  return (
    <Card 
      className={`need-request-card urgency-${request.urgency_level}`}
      style={{ borderLeftColor: urgencyLevelConfig.color }}
    >
      {/* Request header */}
      <div className="request-header">
        <div className="request-icons">
          <div 
            className="need-type-icon"
            style={{ backgroundColor: needTypeConfig.color }}
          >
            <Icon name={needTypeConfig.icon} />
          </div>
          
          <div 
            className="urgency-icon"
            style={{ backgroundColor: urgencyLevelConfig.color }}
          >
            <Icon name={urgencyLevelConfig.icon} />
          </div>
        </div>
        
        <div className="request-meta">
          <div className="request-title">
            {formatNeedTypeName(request.need_type)} Assistance Needed
          </div>
          
          <div className="request-details">
            <span className="request-urgency">
              {formatUrgencyLevelName(request.urgency_level)}
            </span>
            
            <span className="request-time">
              Requested {formatRelativeTime(request.timestamp)}
            </span>
            
            {request.preferred_helpers.length > 0 && (
              <span className="preferred-helper-indicator">
                <Icon name="preferred" size="small" />
                Preferred helpers selected
              </span>
            )}
          </div>
        </div>
      </div>
      
      {/* Request content */}
      <div className="request-content">
        <p className="request-description">
          {request.description}
        </p>
        
        {request.preferred_helpers.length > 0 && userRole !== 'care_recipient' && (
          <div className="preferred-helpers-list">
            <label>Preferred Helpers:</label>
            <div className="helpers">
              {request.preferred_helpers.map(helper => (
                <UserChip 
                  key={helper.id} 
                  user={helper}
                  highlighted={helper.id === currentUserId}
                />
              ))}
            </div>
          </div>
        )}
      </div>
      
      {/* Response section */}
      {!isResponding ? (
        <div className="response-options">
          {/* Only show response options if not already responded and has permission */}
          {!request.user_response && config.canRespond && (
            <div className="response-buttons">
              {config.availableResponseTypes.map(responseType => (
                <Button
                  key={responseType}
                  onClick={() => handleSelectResponse(responseType)}
                  className={`response-button response-${responseType}`}
                  style={{
                    backgroundColor: responseType === 'volunteer' ? '#4CAF50' : undefined,
                    color: responseType === 'volunteer' ? 'white' : undefined
                  }}
                >
                  <Icon name={responseTypes[responseType].icon} />
                  {responseTypes[responseType].message}
                </Button>
              ))}
            </div>
          )}
          
          {/* Show user's response if already responded */}
          {request.user_response && (
            <div className="user-response">
              <div className="response-label">
                Your response:
              </div>
              <div className={`response-value response-${request.user_response.response_type}`}>
                <Icon name={responseTypes[request.user_response.response_type].icon} />
                {responseTypes[request.user_response.response_type].message}
                {request.user_response.response_type === 'decline' && request.user_response.reason && (
                  <div className="decline-reason">
                    Reason: {request.user_response.reason}
                  </div>
                )}
                <div className="response-time">
                  {formatRelativeTime(request.user_response.timestamp)}
                </div>
              </div>
            </div>
          )}
          
          {/* Show all responses (for organizer/caregiver) */}
          {(userRole === 'caregiver' || userRole === 'organizer' || userRole === 'care_recipient') && 
           request.responses && request.responses.length > 0 && (
            <div className="all-responses">
              <Button 
                onClick={() => openResponsesModal(request)}
                icon="response-list"
              >
                View All Responses ({request.responses.length})
              </Button>
            </div>
          )}
        </div>
      ) : (
        <div className="response-form">
          {selectedResponse === 'decline' && (
            <div className="decline-form">
              <label htmlFor="decline-reason">
                Please provide a reason for declining:
              </label>
              <textarea
                id="decline-reason"
                value={declineReason}
                onChange={(e) => setDeclineReason(e.target.value)}
                placeholder="Let them know why you're unable to help..."
                required
              ></textarea>
              
              <div className="form-actions">
                <Button 
                  onClick={() => handleSubmitResponse('decline', { reason: declineReason })}
                  disabled={!declineReason.trim()}
                  primary
                >
                  Submit Response
                </Button>
                <Button onClick={() => setIsResponding(false)}>
                  Cancel
                </Button>
              </div>
            </div>
          )}
          
          {selectedResponse === 'assign' && userRole === 'organizer' && (
            <div className="assign-form">
              <label>
                Assign this request to:
              </label>
              <AssignmentForm
                request={request}
                onAssign={(assigneeId) => handleSubmitResponse('assign', { assignee_id: assigneeId })}
                onCancel={() => setIsResponding(false)}
              />
            </div>
          )}
        </div>
      )}
    </Card>
  );
}
```

### 2.4 Notification Components

#### 2.4.1 Purpose
Provides an intelligent, role-appropriate notification system that keeps users informed of relevant updates, tasks, and communications while respecting boundaries and preventing notification fatigue.

#### 2.4.2 Technical Specifications

| Attribute | Specification |
|-----------|---------------|
| **Notification Types** | Task, Update, Message, Need, Event, Health, System |
| **Delivery Methods** | In-app, Push, Email, SMS |
| **Priority Levels** | Critical, Important, Standard, Low |
| **Grouping Options** | By type, By source, By priority, Chronological |
| **Customization Options** | Per type, Per source, Quiet hours, Delivery method preferences |

#### 2.4.3 Component Structure

```json
{
  "notifications": {
    "notification_types": {
      "task": {
        "icon": "task",
        "color": "#2196F3",
        "default_priority": "standard",
        "default_delivery": ["in-app", "push"]
      },
      "update": {
        "icon": "update",
        "color": "#FF9800",
        "default_priority": "standard",
        "default_delivery": ["in-app"]
      },
      "message": {
        "icon": "message",
        "color": "#4CAF50",
        "default_priority": "standard",
        "default_delivery": ["in-app", "push"]
      },
      "need": {
        "icon": "need",
        "color": "#9C27B0",
        "default_priority": "important",
        "default_delivery": ["in-app", "push", "email"]
      },
      "event": {
        "icon": "event",
        "color": "#3F51B5",
        "default_priority": "standard",
        "default_delivery": ["in-app", "push"]
      },
      "health": {
        "icon": "health",
        "color": "#F44336",
        "default_priority": "important",
        "default_delivery": ["in-app", "push", "email"]
      },
      "system": {
        "icon": "system",
        "color": "#607D8B",
        "default_priority": "low",
        "default_delivery": ["in-app"]
      }
    },
    "priority_levels": {
      "critical": {
        "icon": "critical",
        "color": "#F44336",
        "allow_mute": false,
        "override_quiet_hours": true
      },
      "important": {
        "icon": "important",
        "color": "#FF9800",
        "allow_mute": true,
        "override_quiet_hours": false
      },
      "standard": {
        "icon": "standard",
        "color": "#2196F3",
        "allow_mute": true,
        "override_quiet_hours": false
      },
      "low": {
        "icon": "low",
        "color": "#9E9E9E",
        "allow_mute": true,
        "override_quiet_hours": false
      }
    },
    "delivery_methods": ["in-app", "push", "email", "sms"]
  }
}
```

#### 2.4.4 Role Adaptations

| Role | Notification Focus | Preferences | Delivery Methods | Special Features |
|------|-------------------|------------|-------------------|-----------------|
| **Caregiver** | All types, Comprehensive coverage | Full customization | All methods | Critical alerts, Prioritization, Delegation |
| **Care Recipient** | Personal, Privacy-controlled | Full control | User preference | Dignified notifications, Non-intrusive design, Simple preferences |
| **Organizer** | Team coordination, Task-focused | Team notifications | In-app, Push, Email | Team digests, Assignment notifications, Conflict alerts |
| **Supporter** | Task-related only | Limited customization | In-app, Push | Task-focused, Simplified design, Commitment notifications |

#### 2.4.5 Implementation Example

```jsx
// Notification center component with role adaptations
function NotificationCenter({ userRole }) {
  // Get role-specific configuration
  const config = useNotificationConfig(userRole);
  
  // State for notifications and filters
  const [notifications, setNotifications] = useState([]);
  const [unreadCount, setUnreadCount] = useState(0);
  const [isOpen, setIsOpen] = useState(false);
  const [filters, setFilters] = useState({
    type: 'all',
    priority: 'all',
    read: 'all'
  });
  
  // Load notifications with permission filtering
  useEffect(() => {
    const fetchNotifications = async () => {
      try {
        const data = await getNotifications(filters);
        setNotifications(data.notifications);
        setUnreadCount(data.unread_count);
      } catch (error) {
        console.error('Error fetching notifications:', error);
      }
    };
    
    fetchNotifications();
    
    // Set up real-time notification updates
    const unsubscribe = subscribeToNotifications((newNotification) => {
      setNotifications(prev => [newNotification, ...prev]);
      setUnreadCount(prev => prev + 1);
    });
    
    return () => unsubscribe();
  }, [filters]);
  
  // Handle opening notification center
  const handleToggleNotificationCenter = () => {
    setIsOpen(!isOpen);
    
    if (!isOpen) {
      // Mark as viewed when opening
      markNotificationsAsViewed();
    }
  };
  
  // Handle marking notification as read
  const handleMarkAsRead = async (notificationId) => {
    await markNotificationAsRead(notificationId);
    
    setNotifications(prev => 
      prev.map(notification => 
        notification.id === notificationId 
          ? { ...notification, read: true } 
          : notification
      )
    );
    
    setUnreadCount(prev => Math.max(0, prev - 1));
  };
  
  // Handle marking all as read
  const handleMarkAllAsRead = async () => {
    await markAllNotificationsAsRead();
    
    setNotifications(prev => 
      prev.map(notification => ({ ...notification, read: true }))
    );
    
    setUnreadCount(0);
  };
  
  // Handle notification click
  const handleNotificationClick = (notification) => {
    // Mark as read
    handleMarkAsRead(notification.id);
    
    // Navigate to relevant content
    navigateToNotificationTarget(notification);
  };
  
  // Get filtered notifications
  const filteredNotifications = useMemo(() => {
    return notifications.filter(notification => {
      // Filter by type
      if (filters.type !== 'all' && notification.type !== filters.type) {
        return false;
      }
      
      // Filter by priority
      if (filters.priority !== 'all' && notification.priority !== filters.priority) {
        return false;
      }
      
      // Filter by read status
      if (filters.read === 'unread' && notification.read) {
        return false;
      }
      
      if (filters.read === 'read' && !notification.read) {
        return false;
      }
      
      return true;
    });
  }, [notifications, filters]);
  
  return (
    <div className="notification-center-container">
      {/* Notification bell button */}
      <Button 
        onClick={handleToggleNotificationCenter}
        className="notification-bell"
        aria-label={`Notifications (${unreadCount} unread)`}
      >
        <Icon name="notification" />
        {unreadCount > 0 && (
          <Badge>{unreadCount}</Badge>
        )}
      </Button>
      
      {/* Notification panel - shown when open */}
      {isOpen && (
        <div className="notification-panel">
          <header className="notification-header">
            <h2>{config.title}</h2>
            
            <div className="notification-controls">
              {/* Filter controls - simplified for some roles */}
              {config.showFilters && (
                <FilterDropdown
                  filters={filters}
                  onChange={setFilters}
                  options={config.filterOptions}
                />
              )}
              
              {/* Mark all as read button */}
              {unreadCount > 0 && (
                <Button 
                  onClick={handleMarkAllAsRead}
                  className="mark-all-read"
                >
                  Mark all as read
                </Button>
              )}
              
              {/* Settings button - only for roles with customization */}
              {config.canCustomizeNotifications && (
                <Button 
                  onClick={() => navigateToNotificationSettings()}
                  icon="settings"
                  aria-label="Notification Settings"
                />
              )}
              
              {/* Close button */}
              <Button 
                onClick={() => setIsOpen(false)}
                icon="close"
                aria-label="Close"
              />
            </div>
          </header>
          
          {/* Notification list */}
          <div className="notification-list">
            {filteredNotifications.length === 0 ? (
              <div className="empty-notifications">
                <Icon name="notifications-empty" size="large" />
                <p>{filters.type === 'all' ? config.emptyStateMessage : `No ${filters.type} notifications`}</p>
              </div>
            ) : (
              filteredNotifications.map(notification => (
                <NotificationItem
                  key={notification.id}
                  notification={notification}
                  onClick={() => handleNotificationClick(notification)}
                  onMarkAsRead={() => handleMarkAsRead(notification.id)}
                  userRole={userRole}
                />
              ))
            )}
          </div>
          
          {/* Footer with view all link */}
          <footer className="notification-footer">
            <Button 
              onClick={() => navigateToAllNotifications()}
              className="view-all"
            >
              View All Notifications
            </Button>
          </footer>
        </div>
      )}
    </div>
  );
}

// Individual notification item component
function NotificationItem({ notification, onClick, onMarkAsRead, userRole }) {
  // Get notification type configuration
  const { notificationTypes, priorityLevels } = useNotificationConfig();
  const typeConfig = notificationTypes[notification.type];
  const priorityConfig = priorityLevels[notification.priority];
  
  // Format notification time
  const formattedTime = formatRelativeTime(notification.timestamp);
  
  // Handle marking as read without navigating
  const handleMarkAsRead = (e) => {
    e.stopPropagation();
    onMarkAsRead();
  };
  
  return (
    <div 
      className={`notification-item ${notification.read ? 'read' : 'unread'} priority-${notification.priority}`}
      onClick={onClick}
    >
      {/* Notification icon */}
      <div 
        className="notification-icon"
        style={{ backgroundColor: typeConfig.color }}
      >
        <Icon name={typeConfig.icon} />
      </div>
      
      {/* Notification content */}
      <div className="notification-content">
        <div className="notification-title">
          {notification.title}
          
          {/* Priority indicator for important/critical */}
          {(notification.priority === 'important' || notification.priority === 'critical') && (
            <span 
              className={`priority-indicator priority-${notification.priority}`}
              style={{ backgroundColor: priorityConfig.color }}
            >
              <Icon name={priorityConfig.icon} size="small" />
            </span>
          )}
        </div>
        
        <div className="notification-message">
          {notification.message}
        </div>
        
        <div className="notification-meta">
          <span className="notification-time">
            {formattedTime}
          </span>
          
          {/* Type indicator */}
          <span className="notification-type">
            {formatNotificationType(notification.type)}
          </span>
        </div>
      </div>
      
      {/* Unread indicator and actions */}
      <div className="notification-actions">
        {!notification.read && (
          <>
            <div className="unread-indicator"></div>
            <Button 
              onClick={handleMarkAsRead}
              className="mark-read"
              icon="check"
              aria-label="Mark as read"
            />
          </>
        )}
      </div>
    </div>
  );
}
```

## 3. Technical Implementation

### 3.1 Data Model

The Communication Hub builds on the data model extensions defined in `/07_Technical/Data_Models/data-model-extensions.md`, with these key structures:

```json
{
  "update": {
    "id": "update123",
    "type": "observation",
    "content": "Mom seemed more tired than usual today, but ate a full lunch",
    "media": [
      {
        "type": "image",
        "url": "https://storage.example.com/media/img456.jpg",
        "thumbnail_url": "https://storage.example.com/media/thumb_img456.jpg",
        "created_at": "2025-04-14T10:30:00Z"
      }
    ],
    "timestamp": "2025-04-14T10:30:00Z",
    "author": {
      "user_id": "user123",
      "role": "caregiver"
    },
    "care_recipient": {
      "user_id": "recipient456"
    },
    "care_circle_id": "circle123",
    "related_items": [
      {
        "type": "task",
        "id": "task456",
        "title": "Monitor energy levels"
      }
    ],
    "privacy_level": "caregivers_only",
    "visibility_overrides": [],
    "reactions": [
      {
        "user_id": "user789",
        "type": "acknowledge",
        "timestamp": "2025-04-14T10:45:00Z"
      }
    ],
    "comments": [
      {
        "id": "comment123",
        "content": "Thanks for the update. I'll check in on her this evening.",
        "author_id": "user789",
        "timestamp": "2025-04-14T10:50:00Z",
        "edited": false
      }
    ]
  },
  
  "conversation": {
    "id": "conv123",
    "type": "group",
    "title": "Mom's Care Team",
    "participants": [
      {
        "user_id": "user123",
        "role": "caregiver"
      },
      {
        "user_id": "user456",
        "role": "organizer"
      },
      {
        "user_id": "user789",
        "role": "supporter"
      }
    ],
    "created_at": "2025-03-15T14:00:00Z",
    "created_by": {
      "user_id": "user123",
      "role": "caregiver"
    },
    "care_circle_id": "circle123",
    "latest_message": {
      "id": "msg456",
      "sender": {
        "user_id": "user123",
        "role": "caregiver"
      },
      "type": "text",
      "content": "Don't forget Mom's appointment tomorrow at 2pm",
      "timestamp": "2025-04-14T15:30:00Z",
      "delivery_status": "delivered"
    },
    "unread_count": 2,
    "last_read_message_id": {
      "user123": "msg456",
      "user456": "msg455",
      "user789": "msg454"
    }
  },
  
  "message": {
    "id": "msg456",
    "conversation_id": "conv123",
    "sender": {
      "user_id": "user123",
      "role": "caregiver"
    },
    "type": "text",
    "content": "Don't forget Mom's appointment tomorrow at 2pm",
    "timestamp": "2025-04-14T15:30:00Z",
    "read_by": [
      {
        "user_id": "user123",
        "timestamp": "2025-04-14T15:30:00Z"
      }
    ],
    "delivery_status": "delivered",
    "privacy_level": "care_team",
    "visibility_overrides": []
  },
  
  "need_request": {
    "id": "need123",
    "need_type": "practical",
    "description": "Need a ride to my doctor's appointment tomorrow at 2pm",
    "urgency_level": "tomorrow",
    "timestamp": "2025-04-14T10:00:00Z",
    "care_recipient": {
      "user_id": "recipient456"
    },
    "care_circle_id": "circle123",
    "preferred_helpers": [
      {
        "user_id": "user789",
        "role": "supporter"
      }
    ],
    "privacy_level": "circle",
    "visibility_overrides": [],
    "status": "open",
    "responses": [
      {
        "user_id": "user789",
        "response_type": "volunteer",
        "timestamp": "2025-04-14T10:30:00Z"
      }
    ],
    "resolution": null
  },
  
  "notification": {
    "id": "notif123",
    "user_id": "user123",
    "type": "task",
    "title": "New task assigned to you",
    "message": "John assigned you to pick up medications tomorrow",
    "related_item": {
      "type": "task",
      "id": "task456"
    },
    "timestamp": "2025-04-14T09:30:00Z",
    "priority": "standard",
    "read": false,
    "viewed": false,
    "delivered_via": ["in-app", "push"]
  }
}
```

### 3.2 State Management

```javascript
// Communication hub state management (pseudocode)
const communicationState = {
  // Update feed state
  updates: {
    byId: {},
    allIds: [],
    filters: {
      type: 'all',
      author: 'all',
      timeframe: 'all',
      privacy: 'all'
    },
    sortOption: 'newest',
    loading: false,
    error: null
  },
  
  // Messaging state
  conversations: {
    byId: {},
    allIds: [],
    loading: false,
    error: null
  },
  
  messages: {
    byConversationId: {},
    loading: false,
    error: null
  },
  
  messaging: {
    selectedConversationId: null,
    draftMessages: {},
    pendingMessages: []
  },
  
  