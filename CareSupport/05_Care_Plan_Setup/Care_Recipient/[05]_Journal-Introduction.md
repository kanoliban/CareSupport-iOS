# `/03_Screen_by_Screen_Specs/CarePlan-Setup/CareRecipient/[05]_Journal-Introduction.md`

## Screen Title
Your Private Thoughts

## Role
Care Recipient

## Purpose
To introduce the care recipient to their personal journal space, providing a safe place for self-expression, emotional processing, and reflection. This screen establishes the journal as a tool for maintaining autonomy, tracking wellbeing, and optionally sharing thoughts with trusted circle members—all while reinforcing the care recipient's voice and agency in their care journey.

## UI Components
- **Progress indicator**: Step 5 of 6
- **Journal Demonstration Area**:
  - Visual representation of journal interface
  - Sample entry with placeholder text
  - Privacy indicator (lock icon)
- **First Journal Entry Creation**:
  - Simple prompt: “What would you like to remember or reflect on today?”
  - Text area with prominent voice input option
  - Optional: Mood tracker (visual scale from 1–5) *(MVP 1.0)*
  - Date/time stamp (auto-generated)
- **Privacy Toggle (default)**:
  - Global default setting: "Keep all entries private"
  - Per-entry privacy options: "Just for me" / "Share with specific people" / "Share with care circle" *(Phase 2)*

## Primary Actions
- **Primary CTA**: "Save My First Entry"
- **Secondary CTA**: "Skip for now"
- **Tertiary Link**: "Just show me how it works"

## Voice-to-Text Support
- Prominent microphone icon in journal entry area
- Real-time transcription preview with edit
- Visual waveform during voice recording
- Voice recording playback with re-record option

## Emotional Framing
- **Header**: "Your Private Thoughts"
- **Supporting message**: "Your journal is a space that belongs to you. Share as much or as little as you choose."
- **Autonomy reinforcement**: "Sometimes writing or speaking your thoughts can help process feelings and keep track of what matters to you."
- **Privacy assurance**: "You control who sees what—each entry can be completely private or shared with people you trust."
- **Tone**: Supportive, calm, validating the importance of self-expression without pressure

## Transition Behavior
- **Comes After**: `[04]_Help-Preferences.md`
- **Leads To**: `[06]_CarePlan-Complete.md`
- **Data Flow**: First journal entry (if created) is saved to personal journal
- **Skip Behavior**: Initializes journal space with a placeholder entry for continuity on dashboard

## Logic & Flow Notes
- If “Just show me how it works” is tapped:
  - Walkthrough demo version of journal
  - Shows recording, typing, privacy, and save features
  - Option to continue to next screen or write a real entry
- If “Skip for now”:
  - System flags dashboard for first-time journal setup reminder
  - Placeholder entry created for continuity

## Journal Entry Data Structure (MVP 1.0)
```json
{
  "entry_id": "abc123",
  "timestamp": "2025-04-12T14:30:00Z",
  "content": {
    "text": "I'm feeling a bit tired today but looking forward to Sarah's visit tomorrow.",
    "voice_recording_url": "url_to_recording"
  },
  "mood_rating": 3,
  "media": [],
  "privacy": {
    "level": "private",
    "shared_with": []
  },
  "tags": ["daily_check_in"]
}
```

## Privacy Control (MVP 1.0)
- Default privacy setting = private
- Binary toggle for MVP: private vs. shared
- Future versions will support per-entry selective sharing (Phase 2)

## Technical Implementation Notes
- Native iOS voice recording and transcription tools
- End-to-end encryption for all journal entries
- Auto-save and offline caching enabled
- Save journal entry state even if user exits mid-writing
- Lightweight success animation upon save

## Accessibility Considerations
- Voice input as primary method reduces friction
- Adjustable text size for reading comfort
- Full screen reader compatibility
- Clear touch targets (44x44px)
- Haptic feedback during recording
- Keyboard navigable interface
- Supports “Reduce Motion” system setting

## Completion Flow
- **On Save**:
  - Gentle celebration animation (e.g. soft pulse or glow)
  - Toast: "Your first journal entry is saved"
  - Auto-transition to next screen after 2 seconds
- **On Skip**:
  - Toast: "You can start your journal anytime"
  - System sets reminder card on dashboard
  - Transition to `[06]_CarePlan-Complete.md`

---

## Final Notes
The journal is not just a feature—it’s a quiet act of respect. It gives care recipients a place to express, reflect, and preserve moments without needing to explain themselves. With thoughtful defaults, voice-first interaction, and privacy-first design, this screen honors both the human complexity of care and the dignity of self-expression.

