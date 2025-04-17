
# [06] Invite Care Recipient

## Screen Name
Invite Care Recipient

## Purpose
To allow the caregiver to invite the care recipient into the CareSupport platform and establish the foundational link for shared care coordination.

## Trigger
Triggered immediately after the caregiver completes their quick-win action based on their primary challenge.

## Role
- **Visible to**: Caregiver
- **Not visible to**: Care Recipient, Organizer, Supporter

## UI Components
- Header: "Invite Your Care Recipient"
- Subtext: “You’re almost done setting up. Let’s invite the person you’re caring for so they can join the circle when ready.”
- Input fields:
  - Phone number (with SMS preview format)
  - Email (optional)
- Primary button: “Send Invitation”
- Secondary option: “Skip for now” (small text link, styled gently)
- Confirmation banner: “Invitation sent!” (or fallback error messaging)

## Logic

### Validation:
- At least one contact method is required (phone or email).
- Must be formatted and validated before enabling submission.

### Behavior:
- If “Send Invitation” is tapped:
  - Store contact in Care Recipient profile
  - Generate a secure, time-limited invite link
  - Send SMS or email based on user entry
  - Trigger audit log event
  - Move user to next screen after confirmation toast
- If “Skip for now”:
  - Move to next screen
  - Tag care recipient as “Uninvited”
  - Create dashboard prompt/reminder to invite later

## Emotional Intelligence Notes
- Reinforces autonomy: caregiver is not forced to share unless ready
- Framing avoids language of control; focuses on inviting, not registering someone else
- Visual hierarchy favors forward momentum while still offering deferment

## Accessibility Notes
- All inputs accessible by keyboard/tab
- Input field labels are persistent and outside the field
- Form validation errors announced by screen reader
- “Skip for now” link is fully accessible and clearly described

## Data Stored
- Care recipient contact method(s)
- Invitation status (sent, skipped, pending)
- Timestamp of invitation
- Invitation method used (SMS or email)

## Success Criteria
- At least one contact method entered and validated
- Link successfully generated and sent
- User is redirected without confusion
- Skipping option saves state for future follow-up
