# `/03_Screen_by_Screen_Specs/CarePlan-Setup/Supporter/[04]_Communication-Complete.md`

## Screen Title
Staying Connected

## Role
Supporter

## Purpose
To establish communication preferences, set boundaries, and complete the Care Plan Setup. This final screen empowers Supporters to control how and when they're contacted about care opportunities, reinforces the value of their contribution, and transitions them to the dashboard with clear expectations about their role going forward.

## UI Components
- **Progress indicator**: Step 4 of 4 (completed state)
- **Communication preferences section**:
  - **Contact method selector**: Radio buttons for preferred method
    - SMS / Text messages
    - Email
    - In-App notifications only
  - **Do-not-contact time range**: Time picker for quiet hours
  - **Help request preference**: How they prefer to be asked for help
    - Direct ask
    - Scheduled request
    - Optional nudges
- **Setup completion section**:
  - Summary of preferences set
  - Confirmation of role in care circle
  - Next steps preview
- **Primary CTA button**
- **Secondary "Review my settings" link**

## Primary Actions
- **Primary CTA**: "Finish Setup and Join the Circle"
- **Secondary Link**: "Review My Settings"

## Voice-to-Text Support
- Not applicable for this screen

## Emotional Framing
- **Header**: "Staying Connected"
- **Appreciation Message**: "Your support makes a meaningful difference."
- **Connection Assurance**: "We'll check in occasionally based on what you've shared."
- **Boundary Reinforcement**: "You'll only be contacted in ways that work for you."
- **Tone**: Appreciative, respectful of boundaries, forward-looking

## Transition Behavior
- **Comes After**: "[03]_Availability.md"
- **Leads To**: Supporter Dashboard
- **Data Flow**: Completes `supporter_preferences` object, marks setup as complete

## Logic & Flow Notes

### Communication Preferences Logic
- Default contact method based on how they were invited (SMS if by phone, Email if by email)
- Default help request preference: "Scheduled request"
- Do-not-contact time range: Optional

### Summary Content
- Dynamically generated based on previous selections:
  - Selected support types
  - Availability patterns
  - Communication preferences
- Formatted for scannable reading

### Dashboard Preparation
- Initializes dashboard based on support preferences
- Queues relevant support opportunities matching preferences
- Generates welcome message

## Technical Implementation Notes

### Data Structure
```json
{
  "supporter_preferences": {
    "communication": {
      "preferred_method": "sms",
      "do_not_contact": {
        "start": "22:00",
        "end": "07:00",
        "enabled": true
      },
      "help_request_style": "scheduled_request",
      "updated_at": "2025-04-14T17:00:00Z"
    },
    "setup_complete": true,
    "setup_completed_at": "2025-04-14T17:00:00Z"
  }
}
```

### Implementation Considerations
- Use native iOS selection components
- Store communication preferences for notification system
- Create connection to notification permissions if needed
- Generate complete preferences object for dashboard use
- Support partial state recovery if process was interrupted

## Accessibility Considerations
- Method selectors have proper ARIA roles and states
- Time picker is screen reader accessible with keyboard alternative
- All interactive elements have minimum 44x44px touch targets
- Selection states indicated by both color and shape/icon
- Summary content properly structured for screen readers
- Support keyboard navigation through all selectors
- Focus management follows logical order

## Completion Flow
- **On Finish**:
  - Save all preferences and mark setup complete
  - Subtle celebration animation (gentle, dignified)
  - Success message: "You're now part of [Care Recipient]'s care circle"
  - Transition to Supporter Dashboard

- **On Review**:
  - Display comprehensive settings review
  - Allow edits to any previous selections
  - Return to this screen after review complete

## Final Notes
This final screen completes the Supporter's setup journey with clear communication boundaries, summary of their preferences, and transition to active participation. The emphasis remains on flexibility and choice, reinforcing that their contribution is valued but will occur on their terms. By ending with communication preferences, we acknowledge the importance of appropriate boundaries in sustainable support roles and set expectations for ongoing engagement that respects the Supporter's life and other commitments.

---

## Supporter Dashboard Experience

After completing the Care Plan Setup, Supporters are transitioned to a simplified dashboard focused on their specific role:

### Dashboard Key Components:
- **"Ways You Can Help This Week"**: Opportunities matching their preferences
- **Upcoming Scheduled Support**: Any confirmed commitments
- **Care Circle Updates**: Shared updates appropriate to their role
- **Availability Toggle**: Quick way to mark themselves unavailable temporarily
- **Settings Access**: Easy way to adjust preferences set during Care Plan Setup

### Dashboard Philosophy:
- Low-pressure, opportunity-based engagement
- Clarity about what's needed and expected
- Respect for boundaries established during setup
- Appreciation for any contribution made
- Simple path to either increase or decrease involvement

This dashboard experience maintains the emotional intelligence and boundary-respect established during the Care Plan Setup, ensuring Supporters remain engaged at a level that works for their life circumstances.

