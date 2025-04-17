# [03] Care Situation

**Screen Title**: Care Situation  
**Location**: `/03_Screen_by_Screen_Specs/Caregiver-Flow/[03]_Care-Situation.md`  
**Date**: 2025-04-11  
**Status**: ✅ Ready for Handoff

---

## 🧠 Purpose
To capture context about the care recipient’s condition and caregiving scenario. This input drives downstream personalization, dynamic suggestions, and intelligent defaults in the app experience.

---

## 🎯 Screen Goals
- Collect concise caregiver description of their situation.
- Offer helpful autosuggestions for common conditions.
- Balance freeform input and structured tags for both empathy and data usability.

---

## 🧩 Elements

| Component       | Type        | Notes                                                                 |
|----------------|-------------|-----------------------------------------------------------------------|
| Header         | Text        | "Tell us about your care situation"                                  |
| Subtext        | Text        | "This helps us tailor the experience to your current needs."         |
| Text Input     | Textarea    | 3–4 lines visible, expandable                                         |
| Autosuggest    | Dropdown    | Triggers on keystroke; Suggestions include: Dementia, Stroke, Cancer |
| Button         | Primary     | Label: “Continue” – leads to Step 5 (Select Primary Challenge)       |

---

## 🤖 Logic & Behavior

- **Autosuggest** appears after 2 characters.
- User can **select a suggestion** or **continue typing** freely.
- Field must not be empty to proceed.
- Captured input is stored in onboarding context state as `care_situation.description`.

---

## 💬 Microcopy

- Placeholder: “E.g. My mom was recently diagnosed with dementia and needs help with meals and meds.”
- Error state (if empty): “Please share a little about your care situation before continuing.”

---

## 🧠 Emotional Intelligence

- Tone: Calm, helpful, emotionally attuned
- Purposefully positioned early to signal empathy and individualized support
- Helps user feel *seen* rather than processed

---

## ✅ Acceptance Criteria

- [ ] Input accepts 300+ characters without clipping
- [ ] Autosuggest populates based on condition list
- [ ] Continue button disabled until text input is detected
- [ ] Selection or typing updates onboarding state
- [ ] Screen adapts to keyboard appearance on mobile
