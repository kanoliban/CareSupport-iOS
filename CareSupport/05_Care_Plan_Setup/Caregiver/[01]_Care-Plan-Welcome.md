# [01] Care Plan Welcome Screen

---

## 🧭 Purpose

This is the first screen caregivers see immediately after onboarding. It provides emotional continuity, sets the tone for next steps, and initiates the Care Plan Setup flow. This screen must communicate that what follows is valuable, doable, and directly helpful.

---

## 📝 Copy

> _“You’ve built your support circle. Now let’s turn it into action.”_  
> _What you're doing matters. A thoughtful care plan will help everyone know how to support [Care Recipient Name]._  
> _This will only take a few minutes and will save you hours later._

---

## 🧩 Screen Elements

- **Header Text**: Large and welcoming
- **Subheader**: Brief guidance + empathy
- **Care Recipient Name**: Dynamically inserted from onboarding data
- **Progress Indicator**: Shows user is in “Care Plan Setup”
- **Primary CTA**: “Create [Care Recipient]’s Care Plan”
- **Secondary Option**: “I’ll come back to this later”

---

## 🔁 Transition Behavior

- **Comes After**: Final emotional screen of Caregiver onboarding (`[10]_FinalMessage.md`)
- **Leads To**: `[02]_Starting-Point.md` on CTA tap
- **Alternate Path**: If user taps “I’ll come back to this later,” they are taken to dashboard, and a reminder card is added to complete Care Plan setup later

---

## 🧠 Logic & Flow Notes

- Personalized with care recipient’s name and caregiver’s relationship
- References skipped actions (if applicable): “We’ll also have a chance to add any details you skipped earlier.”
- Reinforces sense of momentum from onboarding → planning
- Setup flag is stored so system knows whether to remind user later

---

## ♿ Accessibility Considerations

- Minimum contrast ratio of 4.5:1 for all text
- All elements support screen reader navigation with correct hierarchy
- Tap targets minimum 44x44px
- CTA button is accessible by keyboard and voice control
- Transition animations respect device-level “Reduce Motion” settings
