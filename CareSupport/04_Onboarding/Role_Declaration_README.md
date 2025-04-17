# 01_Role-Declaration.md

## 🧠 Screen Name
**Role Declaration**

## 🧭 Screen Position
**First screen** of the onboarding flow. This screen branches the user into one of four role-specific onboarding paths: Caregiver, Care Recipient, Organizer, or Supporter.

---

## 🖼️ UI Layout

- **Header**:  
  “Let’s get started.”

- **Prompt/Title**:  
  “What best describes your role in this care journey?”

- **Selectable Role Cards** (4):
  - Caregiver
  - Care Recipient
  - Organizer
  - Supporter

Each role card should:
- Be large, touch-optimized (min 48x48px)
- Include a one-line description
- Include an icon or illustration (placeholder: use role-related icons or abstract avatars)

- **CTA (disabled until role selected)**:  
  Button text: `Continue`

---

## 🔁 Logic and Flow

| Action | Outcome |
|--------|---------|
| User taps role | Role is visually selected (checkmark + highlight) |
| User taps “Continue” | System stores selected role in session + routes to the first screen of that role’s flow |
| If user drops mid-onboarding | Resume path based on saved role selection |

> 🔒 Stored in local state before login is complete

---

## 💡 Emotional Intelligence Cues

- Affirmation that all roles are valuable:
  > “Every role is meaningful. Whether you're offering a hand or leading care, we’re here to support you.”

- Subtle, human-language microcopy:
  > “No pressure. Pick what feels closest — you can update later.”

---

## 🔐 Data & Privacy

- Role selection is stored **in local app state**  
- No API call made yet  
- Marked as **non-sensitive** data

---

## ♿ Accessibility Notes

- Ensure high contrast on selected cards (meets WCAG 2.1 AA minimum contrast ratio of 4.5:1)
- Fully keyboard and screen-reader navigable
- Each card labeled with `aria-label` (e.g., `aria-label="Caregiver: I’m coordinating care for someone else"`)

---

## 🧪 QA Acceptance Criteria

- [ ] All four roles selectable and visually distinguishable  
- [ ] Cannot proceed without making a selection  
- [ ] Role stored in local session data  
- [ ] “Continue” leads to correct next screen per role  
- [ ] Mobile responsive  
- [ ] Fully navigable via screen reader and keyboard  
- [ ] Microcopy text appears accurately  
- [ ] Visual feedback on role selection meets tap/hover states  

---

## 🔜 Next Screen Per Role

| Role | Next Screen |
|------|-------------|
| Caregiver | Define-Relationship |
| Care Recipient | Confirm-Invite |
| Organizer | Who-Are-You-Supporting |
| Supporter | How-Can-You-Help |