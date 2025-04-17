/03_Screen_by_Screen_Specs/CarePlan-Setup/CareRecipient/[04]_Help-Preferences.md

# Screen Title
When You Need Support

# Role
Care Recipient

# Purpose
To capture how the care recipient prefers to ask for and receive help, establishing boundaries and preferences that honor their dignity while ensuring effective support. This screen empowers care recipients to express their needs in a way that maintains their sense of agency and autonomy within the care relationship.

# UI Components
- **Progress indicator**: Step 4 of 6

## Help Request Section
- **Preferred request style (multi-select with icons)**:
  - Direct request: "I'll ask directly when I need something"
  - Through the app: "I'll use CareSupport to send requests"
  - Scheduled check-ins: "I prefer regular check-ins"
  - Indirect cues: "Sometimes I find it hard to ask directly"
- **Urgency preferences (radio select or dropdown)**:
  - "As soon as possible"
  - "Same day is fine"
  - "I prefer to plan ahead"
  - Other (custom text)
- **Priority Contacts**:
  - Simple numbered list of care circle members (1st, 2nd, etc.)
  - Option to assign different people to different need types

## Type of Assistance Section
- **Dynamic cards** generated from onboarding needs or manual entry
  - For each type (e.g., Medication, Meals, Mobility):
    - "Who do you prefer helps with this?"
    - "How do you prefer they help?"
    - "What can you manage on your own?"
    - **Voice input and optional notes for all text fields**
    - **Privacy toggle**: "Share with care circle" / "Keep private"

## Emergency Section
- **Primary emergency contact** (dropdown from care circle)
- **Critical info for responders** (text field + voice input)
- **Anything else responders should know** (text + audio)

## In My Own Words Section
- Text box + large voice recorder
- Prompt: “Is there anything else you want to share about how you like to be helped?”
- **Privacy toggle**

# Primary Actions
- **Primary CTA**: "Save My Help Preferences"
- **Secondary CTA**: "Skip for now"

# Voice-to-Text Support
- Prominent microphone icon on every text field
- Sample guidance: 
  - “Say how you'd like help with meals…”
  - “Tell us who you trust most for emergencies…”

# Emotional Framing
- **Header**: "When You Need Support"
- **Subtext**: 
  - “Asking for help can be hard. You deserve to be supported in ways that feel right to you.”
  - “You're in control. We'll follow your lead.”
  - “The right kind of help respects your independence.”

# Transition Behavior
- **Comes After**: "[03]_Communication-Style.md"
- **Leads To**: "[05]_Journal-Introduction.md"
- **Skip Behavior**: Sets flag and proceeds with default structure
- **Post-Save Animation**: Gentle pulse + confirmation toast + Summary card
- **Optional Summary Preview**:
  - Display key selections for confirmation before saving

# Logic & Flow Notes

## Data Structure
```json
{
  "help_request_preferences": {
    "request_methods": ["direct", "app"],
    "urgency": "same_day",
    "custom_notes": "Advance notice preferred",
    "priority_contacts": [
      {"user_id": "abc123", "name": "Sarah", "relationship": "daughter"},
      {"user_id": "def456", "name": "James", "relationship": "friend"}
    ],
    "shared": true
  },
  "assistance_preferences": [
    {
      "need_type": "mobility",
      "preferred_helpers": ["abc123"],
      "helper_approach": "Offer your arm instead of grabbing",
      "self_management": "I can manage short distances alone",
      "shared": false
    }
  ],
  "emergency_preferences": {
    "primary_contact": "abc123",
    "responder_info": "Blood thinners, pacemaker",
    "notes": "Medical card in wallet",
    "shared": true
  },
  "personal_notes": {
    "text": "Sometimes I just need encouragement, not full assistance.",
    "voice_url": "audio_file_url",
    "shared": true
  }
}
```

# Technical Implementation Notes
- Modular input components for each field
- Auto-save drafts on interaction
- Pre-fill values if pulled from onboarding
- Data caching enabled
- Voice-to-text transcriptions stored securely
- All preferences stored per user role + timestamp

# Accessibility
- Voice input as first-class input
- Icons with labels on all cards
- 44x44px tap targets
- Focus indicators
- Screen reader labels
- Keyboard nav
- High contrast + no color-only indicators
- "Reduce Motion" respected

# Completion Flow
- **On Save**:
  - Confirmation toast: “Your help preferences are saved”
  - Animated success checkmark
  - Proceed to next screen after 1.5s
- **On Skip**:
  - Apply general defaults (from onboarding or typical care)
  - Gentle “You can set this up later” toast
  - Transition to Journal

# Final Notes
This screen honors the deeply personal and often difficult reality of needing help. By providing flexible tools to express how support is received, and empowering the care recipient to set boundaries and preferences without friction, CareSupport ensures help is offered with dignity, clarity, and respect. This reinforces our core product principle: Care begins with understanding.