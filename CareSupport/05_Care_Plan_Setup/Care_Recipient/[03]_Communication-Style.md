# /03_Screen_by_Screen_Specs/CarePlan-Setup/CareRecipient/[03]_Communication-Style.md

## Screen Title
How Would You Like to Connect?

## Role
Care Recipient

## Purpose
To establish communication preferences that respect the care recipient's autonomy, boundaries, and emotional comfort. This screen captures how, when, and about what the recipient prefers to communicate with their care circle, ensuring interactions happen on their terms while facilitating effective care coordination.

## UI Components
- **Progress indicator**: Step 3 of 6
- **Communication Method Section**:
  - Method cards with icons + text (multi-select):
    - Phone Call (phone icon)
    - Text Message (message bubble icon)
    - App Notifications (bell icon)
    - In-Person (people icon)
    - Video Call (camera icon)
    - Other (plus icon with text field)
  - Preference indicator for each (star icon for "Preferred")
  - Suggested methods from caregiver visually distinct (light background with "Suggested" tag)

- **Best Times to Reach Section**:
  - Time chips (multi-select):
    - Morning (7–10 AM)
    - Midday (11–2 PM)
    - Afternoon (2–5 PM)
    - Evening (6–9 PM)
  - "Other" chip opens time range selector
  - "Do Not Disturb" time range selector

- **Conversation Preferences Section**:
  - "Topics I enjoy talking about" (multi-select + text field):
    - Family updates, Memories/past experiences, Hobbies/interests, Current events, Day-to-day updates, Other
  - "Topics I prefer to avoid" (optional text field)
  - "How I prefer to receive health information" (dropdown)
  - "How I like to be addressed" (text field)

- **"In My Own Words" Section**:
  - Text area
  - Prominent voice recording button
  - Voice recording playback with edit option

- **Privacy Controls**:
  - Toggle on each section: "Share with my care circle" / "Keep private"
  - Lock icon for private items

## Primary Actions
- **Primary CTA**: "Save My Communication Style"
- **Secondary CTA**: "Skip for now"

## Voice-to-Text Support
- Microphone icon on all text fields
- Voice prompt suggestions and examples

## Emotional Framing
- **Header**: "How Would You Like to Connect?"
- **Guidance**: "Let your care circle know how to reach out and what feels good to talk about."
- **Reinforcement**: "Clear preferences help everyone connect with you in ways that feel right."
- **Tone**: Empowering, boundary-respecting, warm

## Transition Behavior
- **Comes After**: [02]_Ideal-Day.md
- **Leads To**: [04]_Help-Preferences.md
- **Data Flow**: Preferences saved to profile and shared per privacy settings
- **Skip Behavior**: Defaults applied, reminder flag created

## Logic & Flow Notes
### Communication Preferences Data Structure
```json
{
  "communication_methods": {
    "phone_calls": {"preferred": true, "shared": true},
    "text_messages": {"preferred": false, "shared": true},
    "video_calls": {"preferred": false, "shared": true},
    "in_person": {"preferred": true, "shared": true},
    "app_notifications": {"preferred": false, "shared": true},
    "other": {"label": "Email is fine too", "preferred": false, "shared": true}
  },
  "timing_preferences": {
    "preferred_times": ["morning", "afternoon"],
    "custom_times": [{"start": "13:00", "end": "15:00", "label": "After lunch"}],
    "do_not_disturb": [{"start": "21:00", "end": "07:00"}],
    "shared": true
  },
  "conversation_preferences": {
    "enjoy_topics": ["family", "memories", "hobbies"],
    "custom_topics": "I love hearing about my grandchildren",
    "avoid_topics": "Please don't discuss my medical test results over the phone",
    "preferred_address": "I prefer to be called Elizabeth, not Betty",
    "health_info_preference": "direct_but_gentle",
    "shared": false
  },
  "personal_notes": {
    "text": "I appreciate when you give me time to respond. Sometimes I need a moment to gather my thoughts.",
    "voice_recording_url": "url_to_recording",
    "shared": true
  }
}
```

## Accessibility Considerations
- All cards and chips include icons + text
- High contrast, large fonts (16pt+)
- Touch targets 44x44px minimum
- Screen reader support, proper labels/headings
- VoiceOver navigation fully supported
- Color-independent information
- Error prevention for time conflicts

## Completion Flow
- **On Save**:
  - Gentle animation
  - Toast: "Your communication preferences are saved"
  - Transition to next screen

- **On Skip**:
  - Toast: "Basic communication settings applied"
  - Default setup used
  - Flag for reminder

## Final Notes
This screen balances practical communication needs with emotional respect for boundaries. The simplified card and chip UI keeps interaction clear and direct. The "In My Own Words" section gives care recipients a powerful place to set tone and expectations. Privacy controls support agency, while voice-first support promotes comfort and inclusivity.