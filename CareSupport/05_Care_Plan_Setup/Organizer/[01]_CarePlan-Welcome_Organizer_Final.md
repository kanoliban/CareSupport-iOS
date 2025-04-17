
/03_Screen_by_Screen_Specs/CarePlan-Setup/Organizer/[01]_CarePlan-Welcome.md

# Screen Title
You're Holding It Together — Let's Help You Do It

# Role
Organizer

# Purpose
To welcome the Organizer to their Care Plan Setup, affirming their essential role in care coordination while smoothly transitioning them from onboarding into the next step of building the Care Plan. The screen emphasizes emotional support, clarity of purpose, and the promise of practical tools that will reduce chaos and improve care.

# UI Components
- Progress indicator: Step 1 of 6
- Personalized welcome header: "Hi [Organizer Name]"
- Emotional message card
- Illustration showing coordination (e.g., calendar, arrows between people)
- Preview cards (tiles with icons and short descriptions):
  - Care Team Management
  - Task Assignment
  - Calendar Coordination
  - Communication Hub
- Optional voice reflection input: "Why are you taking on this role?" (not shared)
- Primary CTA button
- Secondary "I'll do this later" link (de-emphasized)
- Time estimate note: "This will take about 5–7 minutes and will make everything clearer moving forward."

# Primary Actions
- CTA: "Set Up Your Coordination Center"
- Secondary: "I'll come back to this later"

# Emotional Framing
- Header: "You’re Making Care Happen"
- Continuity Message: "You’re holding it together — even when things feel scattered."
- Support Message: "Let’s help you organize it all. You’re not alone in this."
- Reinforcement: "We’ve built tools to help simplify the hardest parts."
- Tone: Empowering, dignified, warm without glorifying stress

# Transition Behavior
- Comes After: Final onboarding screen for Organizer
- Leads To: [02]_Care-Team-Overview.md
- If Skipped:
  - Flag set to show a dashboard reminder to complete setup
  - Gentle toast: “You can return to your Coordination Center setup anytime”

# Data Continuity
- Organizer name from onboarding
- If invited by a caregiver: “[Caregiver Name] has invited you to help coordinate care for [Care Recipient Name].”
- Prefetch:
  - Care recipient name
  - Existing care circle
  - Skipped onboarding flags for downstream logic

# Logic & Flow Notes
- If Organizer is also a Caregiver, reference dual-role context (MVP 2)
- Time estimate and progress show setup is guided, not overwhelming
- Optional voice reflection is private; saved in profile with `shared: false`

# Technical Implementation Notes
- Reuse standard welcome screen component
- Store setup initiation time
- Voice note saved as transcription/audio if used
- Initialize coordination object with `coordination_setup_started: true`
- Cache voice input before upload

# Accessibility Considerations
- All elements screen reader accessible
- Icons have text labels
- Voice input toggle reachable by keyboard
- Large touch areas (44x44px minimum)
- High contrast for all text
- Reduce motion settings respected

# Completion Flow
- On Continue:
  - Subtle pulse animation
  - Load coordination state
  - Navigate to next screen after 1s

- On Skip:
  - Set `organizer_skipped_setup: true`
  - Dashboard reminder scheduled
  - Immediate redirect to dashboard

# Final Notes
This screen sets a tone of acknowledgment and support, making the Organizer feel seen for the invisible labor they perform. It communicates: you’re essential — but you don’t have to do it alone. The preview of tools and voice reflection offer both structure and dignity, setting the stage for an empowered but sustainable approach to care coordination.
