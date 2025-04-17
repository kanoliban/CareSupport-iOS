# Post-Setup Transition: Welcome Home Experience

**Version:** 1.0.0  
**Last Updated:** 2025-04-14  
**Status:** Draft  
**Document Location:** `/06_Core_Platform/Post_Setup_Transition/Welcome_Home_Specification.md`

## 1. Overview

This specification defines the critical transition experience that occurs immediately after a user completes onboarding and role-specific setup. The "Welcome Home" experience bridges the gap between setup and active platform usage, providing orientation, immediate value, and setting expectations for ongoing engagement.

### 1.1 Related Documents

- `/05_Care_Plan_Setup/Caregiver/[06]_CarePlan-Complete.md`
- `/05_Care_Plan_Setup/Care_Recipient/[06]_CarePlan-Complete.md`
- `/05_Care_Plan_Setup/Organizer/[06]_CarePlan-Complete.md`
- `/05_Care_Plan_Setup/Supporter/[04]_Communication-Complete.md`
- `/05_Care_Plan_Setup/[00]_Welcome-Overlay.md`

### 1.2 Feature Goals

- Create a seamless transition from setup to active platform usage
- Orient users to the unified platform interface
- Provide immediate value that reinforces their investment in setup
- Set expectations for ongoing engagement
- Establish role-appropriate guidance without overwhelming
- Drive initial engagement with core features relevant to the user's role

## 2. User Stories

| As a... | I want to... | So that... | Priority |
|---------|-------------|------------|----------|
| Caregiver | See a welcome that acknowledges my completed setup | I feel my efforts were worthwhile | 🔴 MUST |
| Caregiver | Understand what I can do next | I can start using the platform productively | 🔴 MUST |
| Care Recipient | See a simple, clear welcome | I feel comfortable using the platform | 🔴 MUST |
| Organizer | See an overview of the care coordination features | I can start managing care effectively | 🔴 MUST |
| Supporter | See a lightweight, low-pressure welcome | I understand how I can help without feeling overwhelmed | 🔴 MUST |
| Any Role | Have the option for guided orientation | I can learn the interface at my own pace | 🟠 SHOULD |
| Any Role | See my profile and care circle status | I understand who's connected and what's next | 🔴 MUST |
| Any Role | Easily find incomplete setup items | I can finish setting up when ready | 🟠 SHOULD |

## 3. Functional Requirements

### 3.1 Core Functionality

| ID | Requirement | Priority | Notes |
|----|-------------|----------|-------|
| WH-01 | System shall display a role-specific welcome overlay immediately after setup completion | 🔴 MUST | Draws from emotional messaging in setup |
| WH-02 | Welcome overlay shall contain 3-4 key actions relevant to user's role | 🔴 MUST | Based on role, preferences, and setup data |
| WH-03 | System shall provide "Show me around" option for guided tooltips | 🟠 SHOULD | Optional guided tour |
| WH-04 | System shall pre-populate interface with data from onboarding and setup | 🔴 MUST | Tasks, preferences, care circle, etc. |
| WH-05 | System shall visually indicate incomplete areas from setup | 🟠 SHOULD | Non-disruptive indicators |
| WH-06 | System shall include "quick win" opportunities based on setup data | 🔴 MUST | E.g., Completing a skipped invite |
| WH-07 | System shall maintain setup emotional framing in transition messaging | 🔴 MUST | Consistent tone and values |
| WH-08 | Welcome experience shall be dismissible | 🔴 MUST | Users can skip to main interface |
| WH-09 | System shall default to role-appropriate dashboard view after welcome | 🔴 MUST | Same structure, different content |
| WH-10 | System shall log completion of welcome experience | 🟠 SHOULD | For analytics and future reference |

### 3.2 Role-Specific Adaptations

| Role | Welcome Content | First-Time Interface | Guidance Focus |
|------|----------------|----------------------|----------------|
| Caregiver | "You've built your support circle. Now let's turn it into action." Draws from emotional framing in [09]_Emotional-Framing.md and [10]_FinalMessage.md | Daily tasks/medications, Care recipient status, Circle coordination | Emphasizes: Quick task creation, Medication tracking, Status updates |
| Care Recipient | "You're not just receiving care—you're shaping it." Draws from [06]_Emotional-Reinforcement.md and [07]_FinalMessage.md | Today's Plan, Express Needs, Privacy controls | Emphasizes: Privacy settings, Viewing schedule, Communication preferences |
| Organizer | "You're holding it together. Let's help you organize it all." Draws from [05]_Emotional-Framing_Organizer.md and [05]_FinalMessage.md | Team overview, Task assignment, Master schedule | Emphasizes: Team coordination, Schedule management, Task allocation |
| Supporter | "Small things make a big difference." Draws from [05]_Emotional-Framing_Supporter.md and [05]_FinalMessage.md | Opportunity view, Availability toggle, Care updates | Emphasizes: Setting availability, Accepting opportunities, Low-pressure engagement |

## 4. UI Components

### 4.1 Welcome Overlay

**Component:** Modal overlay with semi-transparent background
**Content:**
- Role-specific header message
- Brief orienting message (1-2 sentences)
- Visual indication of completed setup (e.g., checkmarks or progress indicator)
- 3-4 key actions represented as buttons or cards
- "Show me around" option
- Dismiss option

### 4.2 First-Time User Interface

**Component:** Modified dashboard with first-time elements
**Content:**
- Standard dashboard layout with role-specific content
- Prominent "getting started" guidance panel
- Highlighted primary actions based on role
- Visual indicators for incomplete setup items
- Simplified navigation with progressive disclosure
- First-time tooltip triggers

### 4.3 Guided Tour System

**Component:** Sequential tooltips highlighting key features
**Content:**
- Sequential tooltips for key interface elements
- Role-appropriate feature introductions
- "Skip" and "Next" options for each step
- Progress indicator (e.g., "2 of 5")
- Completion confirmation

### 4.4 Quick Win Opportunities

**Component:** Actionable cards based on setup data
**Content:**
- Suggested actions from incomplete setup items
- Role-appropriate quick tasks
- Success celebration for completed actions
- Clear completion indicators

## 5. Data Requirements

### 5.1 Data Objects

```json
{
  "welcome_home_state": {
    "user_id": "user123",
    "role": "caregiver",
    "setup_completion_date": "2025-04-14T14:30:00Z",
    "welcome_seen": false,
    "guided_tour_completed": false,
    "tour_progress": 0,
    "quick_wins_completed": [],
    "incomplete_setup_items": [
      {
        "type": "circle_invitation",
        "id": "invite123",
        "context": "Skipped during setup"
      }
    ],
    "personalized_actions": [
      {
        "id": "action123",
        "type": "medication_reminder",
        "context": "From quick win in setup",
        "completed": false
      }
    ]
  }
}
```

### A5.2 API Requirements

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/api/welcome-home/state` | GET | Retrieve welcome home state for user |
| `/api/welcome-home/complete` | POST | Mark welcome home experience as completed |
| `/api/welcome-home/tour-progress` | POST | Update guided tour progress |
| `/api/welcome-home/quick-win/:id` | POST | Complete a quick win action |
| `/api/incomplete-setup` | GET | Retrieve incomplete setup items |

## 6. Edge Cases & Error States

| Scenario | System Behavior | User Feedback |
|----------|----------------|---------------|
| User exits app before completing welcome | Remember progress, resume on next login | "Welcome back! Let's pick up where you left off." |
| User has multiple incomplete setup items | Prioritize based on importance, show one at a time | Show most critical item first with option to see all |
| User completes all setup items during welcome | Update UI to reflect completion, offer new actions | "Everything's set! Here's what you can do next." |
| User has multiple roles | Present welcome for primary role, indicate other roles | Primary role welcome with option to switch roles |
| System can't load personalized content | Fall back to role-based defaults | Generic welcome with error handled gracefully |
| User explicitly skips welcome | Mark as seen, proceed to dashboard | No further welcome prompts unless requested |

## 7. Accessibility Considerations

- All welcome text meets WCAG 2.1 AA contrast requirements (4.5:1 minimum)
- Welcome overlay is fully keyboard navigable with logical tab order
- Screen readers announce welcome content with proper heading structure
- Guided tour tooltips have ARIA labels and are keyboard accessible
- Animations respect "Reduce Motion" settings
- Quick win opportunities have clear success states visible without color dependency
- All interactive elements have minimum 44x44px touch targets
- Content prioritizes clarity over density for readability

## 8. Implementation Notes

### 8.1 Technical Approach

- Welcome overlay and guided tour use shared component library
- Personalization driven by data from onboarding and setup phases
- Role adaptations implemented through component props, not separate components
- Analytics track engagement with welcome components
- Tour state persisted across sessions
- Progressive enhancement for slower connections

### 8.2 Phasing

Phase 1 (MVP):
- Basic welcome overlay with role-specific content
- Dashboard with first-time elements
- Incomplete setup indicators
- Quick win opportunities

Phase 2:
- Enhanced guided tour
- Deeper personalization
- Improved analytics
- Additional quick win types

### 8.3 Integration Dependencies

- Requires completed onboarding and setup data models
- Depends on unified dashboard framework
- Needs role-based permission system
- Requires access to setup completion state

## 9. Success Metrics

- 95% of users complete welcome experience
- 80% engage with at least one suggested action
- 70% opt for guided tour if offered
- 50% complete a quick win action during first session
- <5% exit app during welcome without completing
- Caregiver retention: 85% return within 24 hours

## Change Log

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0.0 | 2025-04-14 | Product Lead | Initial document |
