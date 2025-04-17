
# [04] Select Primary Challenge

## 🧭 Purpose
To understand the caregiver’s most pressing concern and personalize the onboarding flow. This input determines the subsequent “Quick Win” screen and sets the tone for contextual app guidance moving forward.

---

## 🖼️ Screen Overview

- **Screen Title**: “What are you struggling with most right now?”
- **Screen Type**: Full-screen with radio button list
- **Input Type**: Single selection required
- **UI Elements**:
  - Header text
  - Subtext reinforcing empathy
  - List of selectable challenges
  - “Next” button (disabled until selection made)
  - “Something else” option opens short text input field

---

## 🔡 Copy (Example)

- **Header**: What’s the hardest part of caregiving right now?
- **Subtext**: We’ll use this to help tailor your experience.
- **Options**:
  - Keeping track of medications
  - Managing appointments
  - Getting help from others
  - Communicating with family
  - Something else (opens freeform response box)

---

## 🧠 Logic

- User must select one option to proceed
- If “Something else” is selected, user must type at least 5 characters
- Selection is saved and passed to next screen logic
- Used to determine personalized Quick Win task on following screen

---

## 🎯 Outcome

- Variable: `primary_challenge`
- Possible values: `meds`, `appointments`, `help`, `family`, `custom`
- This value controls which screen appears next in Quick Win logic

---

## 📝 Accessibility

- All selections must be keyboard-navigable
- Freeform input has visible label
- Text-to-speech enabled for list options
- Error message if “Next” is tapped with no selection

---

## 💾 Data

- Saved as part of onboarding session
- Stored securely in encrypted state
- Available to emotional intelligence layer for context

---

## 💬 Emotional Intelligence Cues

- Empathetic copy reinforces understanding of caregiving complexity
- Tone is supportive, non-clinical
- Reinforces user agency by allowing “custom” input
