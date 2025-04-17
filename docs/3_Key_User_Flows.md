<!--
Date: April 17, 2025
Purpose: Key User Flows document for CareSupport iOS app
Contains: Detailed user flows for onboarding, daily experiences, and cross-role interactions
Created by: Cascade AI
-->

# CareSupport iOS: Key User Flows

**Document Version:** 1.0.0  
**Last Updated:** 2025-04-17  

## 1. Onboarding & Setup

### 1.1 Universal Onboarding Start

**Flow Description:** All users begin with a common entry point that introduces the app concept before diverging into role-specific flows.

**Sequence:**
1. Welcome/intro screens explaining CareSupport concept
2. Authentication (login or signup)
3. Role selection screen
4. Transition to role-specific onboarding

![Universal Onboarding Flow]

### 1.2 Caregiver Onboarding Flow

**Flow Description:** Primary flow for caregivers joining the platform, establishing care relationships and initial needs.

**Sequence:**
1. Define relationship to care recipient
   - Select relationship type (parent, child, spouse, friend, etc.)
   - Input relationship duration

2. Identify care recipient
   - Input care recipient name, age, basic information
   - Option to add photo

3. Describe care situation
   - Select primary condition categories
   - Rate current challenges in key areas
   - Note special considerations

4. Select primary challenges
   - Prioritize top 3 challenges from presented options
   - Add custom challenges if needed
   - Rate urgency of each challenge

5. Quick-win action identification
   - Select immediate actions that would provide relief
   - Schedule first action with reminders

6. Invite care recipient (optional)
   - Send email/SMS invitation
   - Set temporary access permissions
   - Option to postpone this step

7. Add to care circle
   - Identify additional helpers
   - Assign initial roles (organizer, supporter)
   - Send invitations

8. Privacy and permissions overview
   - Set default sharing levels
   - Configure sensitive information controls
   - Review permission recommendations

9. Emotional framing
   - Acknowledge caregiver efforts
   - Set expectations for the platform
   - Offer initial resources

10. Home view introduction
    - Guided tour of main dashboard
    - First task/action prompt
    - Care plan setup introduction

### 1.3 Care Recipient Onboarding Flow

**Flow Description:** Tailored experience for individuals receiving care, focusing on autonomy and preferences.

**Sequence:**
1. Confirm invitation
   - Welcome and context setting
   - Relationship confirmation

2. Privacy preferences
   - Information sharing controls
   - Health data visibility settings
   - Communication preferences

3. Express needs and preferences
   - Daily routine preferences
   - Assistance preferences (how, when, who)
   - Custom communication preferences

4. Today's plan preview
   - Review upcoming activities/assistance
   - Modify or adjust as needed
   - Express comfort level with plan

5. Consent confirmation
   - Clear explanation of information usage
   - Review of caregiver access
   - Explicit consent capture

6. Emotional reinforcement
   - Messaging about maintaining independence
   - Adjustment tips and expectations
   - Resource recommendations

7. Home view introduction
   - Guided tour of adapted dashboard
   - Need expression interface demonstration
   - Privacy control access points

### 1.4 Organizer & Supporter Flows

**Abbreviated Flow Description:** Similar patterns with role-specific adjustments, focused on coordination and occasional help respectively.

## 2. Daily User Experience

### 2.1 Caregiver Daily Flow

**Flow Description:** Primary daily interaction patterns for caregivers managing care activities.

**Key Interactions:**

1. **Morning Check-in**
   - Review today's care plan
   - Check medication schedule
   - Prioritize day's tasks
   - Review overnight updates

2. **Task Management**
   - Create and assign new tasks
   - Track task completion status
   - Receive task completion notifications
   - Add notes to completed tasks

3. **Health Tracking**
   - Log medication administration
   - Record vital signs and symptoms
   - Track mood and well-being indicators
   - Document behavioral observations

4. **Communication**
   - Send updates to care circle
   - Respond to inquiries
   - Review and acknowledge support offers
   - Schedule coordination with other roles

5. **Evening Wrap-up**
   - Complete end-of-day checklist
   - Set up tomorrow's priorities
   - Log any concerns or observations
   - Review next day's schedule

### 2.2 Care Recipient Daily Flow

**Flow Description:** Daily experience for the person receiving care, focused on autonomy and need expression.

**Key Interactions:**

1. **Morning Preview**
   - Review today's scheduled assistance
   - Check who will be helping today
   - Adjust preferences if needed
   - Express immediate needs

2. **Need Expression**
   - Request specific assistance
   - Provide context for helpers
   - Set priority level for needs
   - Indicate timing preferences

3. **Daily Status**
   - Log personal well-being
   - Record medication adherence
   - Track symptoms or concerns
   - Note mood and energy levels

4. **Communication Hub**
   - Send messages to specific helpers
   - Post updates to entire circle
   - Review incoming messages
   - Respond to questions or offers

5. **Privacy Management**
   - Review information sharing
   - Adjust visibility settings
   - Control access to new information
   - Manage notification preferences

### 2.3 Organizer Daily Flow

**Flow Description:** Coordination-focused experience for those managing logistics and schedules.

**Key Interactions:**

1. **Schedule Management**
   - Review today's care coverage
   - Identify and resolve schedule gaps
   - Coordinate helper availability
   - Adjust assignments as needed

2. **Task Coordination**
   - Assign and track tasks across team
   - Prioritize unassigned tasks
   - Send reminders for upcoming tasks
   - Resolve task conflicts or issues

3. **Resource Management**
   - Track supply inventory
   - Coordinate transportation needs
   - Manage shared resources
   - Document resource usage

4. **Team Communication**
   - Send coordinator updates
   - Facilitate team problem-solving
   - Answer questions about assignments
   - Relay important information

5. **Reporting**
   - Review task completion rates
   - Track care coverage metrics
   - Identify potential improvements
   - Document care delivery patterns

### 2.4 Supporter Daily Flow

**Flow Description:** Streamlined experience for occasional helpers focused on specific tasks.

**Key Interactions:**

1. **Task Review**
   - Check assigned tasks
   - View task details and notes
   - Accept or request changes
   - See contextual information

2. **Schedule Management**
   - Update availability calendar
   - Receive upcoming task reminders
   - Confirm scheduled commitments
   - Request schedule adjustments

3. **Task Completion**
   - Check in when starting task
   - Follow task instructions
   - Document completion
   - Add relevant notes or observations

4. **Communication**
   - Ask questions about tasks
   - Notify of any issues or delays
   - Provide feedback after completion
   - Coordinate with other supporters

## 3. Cross-Role Interactions

### 3.1 Task Assignment & Completion

**Flow Description:** How tasks flow between different roles from creation to completion.

**Interaction Sequence:**

1. **Task Creation** (Caregiver or Organizer)
   - Create task with details and requirements
   - Set priority, timing, and instructions
   - Add relevant context or resources
   - Mark role requirements

2. **Task Assignment** (Caregiver or Organizer)
   - Select appropriate team member
   - Set due date and reminders
   - Include any special instructions
   - Send assignment notification

3. **Task Acceptance** (Any assigned role)
   - Receive and review assignment
   - Accept task or request modification
   - Ask clarifying questions if needed
   - Add to personal schedule

4. **Task Execution** (Any assigned role)
   - Check in when beginning task
   - Follow documented process
   - Record relevant information
   - Document any issues encountered

5. **Task Completion** (Any assigned role)
   - Mark task as complete
   - Add completion notes
   - Upload any relevant photos/documents
   - Report any follow-up needed

6. **Task Verification** (Caregiver or Organizer)
   - Review completion details
   - Provide feedback if needed
   - Create any follow-up tasks
   - Close task or request modifications

### 3.2 Care Plan Adjustments

**Flow Description:** How care plans are collaboratively modified across roles.

**Interaction Sequence:**

1. **Needs Identification** (Typically Care Recipient)
   - Express changing needs or preferences
   - Request specific adjustments
   - Provide context for request
   - Indicate priority of changes

2. **Plan Review** (Caregiver)
   - Review current plan vs. new needs
   - Assess impact of requested changes
   - Consult with healthcare guidance if needed
   - Create draft adjustment proposal

3. **Coordination** (Organizer)
   - Review logistical implications
   - Check resource availability
   - Consult affected supporters
   - Propose implementation approach

4. **Collaborative Refinement** (All relevant roles)
   - Share draft changes for feedback
   - Discuss concerns or alternatives
   - Make collaborative adjustments
   - Reach consensus on approach

5. **Implementation** (All roles)
   - Update official care plan
   - Adjust task schedules and assignments
   - Update resource allocations
   - Set evaluation timeframe

6. **Communication** (Typically Organizer)
   - Notify all circle members of changes
   - Provide role-specific guidance
   - Answer questions about new plan
   - Schedule follow-up check-in

### 3.3 Health Event Management

**Flow Description:** How health-related events are handled across roles.

**Interaction Sequence:**

1. **Event Detection** (Any role)
   - Observe health change or concern
   - Document symptoms or observations
   - Rate urgency and severity
   - Initiate appropriate alert level

2. **Initial Assessment** (Caregiver)
   - Review reported information
   - Gather additional details if needed
   - Consult healthcare guidelines
   - Determine response level

3. **Response Coordination** (Caregiver + Organizer)
   - Create action plan
   - Assign urgent tasks
   - Adjust schedules as needed
   - Coordinate resources (transport, etc.)

4. **Information Sharing** (Controlled by permissions)
   - Update care circle with appropriate details
   - Provide role-specific instructions
   - Maintain privacy boundaries
   - Document in health record

5. **Follow-up Management** (Multiple roles)
   - Track resolution progress
   - Document outcome and learnings
   - Update care plan if needed
   - Create preventive measures

### 3.4 Circle Membership Changes

**Flow Description:** How new members are added to or removed from the care circle.

**Interaction Sequence:**

1. **Need Identification** (Typically Caregiver)
   - Identify gap in support
   - Define role and responsibilities needed
   - Document time commitment
   - Set permission requirements

2. **Member Selection** (Caregiver, with Care Recipient input)
   - Identify potential new member
   - Review relationship and capabilities
   - Confirm willingness to participate
   - Set expectations for role

3. **Invitation Process** (Caregiver or Organizer)
   - Send formal invitation
   - Include role description
   - Set initial permission levels
   - Provide context about care situation

4. **Onboarding** (New member + system)
   - Complete abbreviated role onboarding
   - Review responsibilities and boundaries
   - Set availability and preferences
   - Connect with key circle members

5. **Integration** (Organizer)
   - Assign initial tasks
   - Introduce to other members
   - Schedule orientation if needed
   - Set up regular check-ins

These user flows represent the core interactions within the CareSupport platform. Each flow should be implemented with attention to emotional intelligence, clear feedback, and appropriate privacy controls.
