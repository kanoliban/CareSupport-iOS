# [00] Welcome Overlay

## Screen Purpose
This overlay card appears immediately after onboarding is completed and serves as a warm, human-centered introduction to the CareSupport experience. It reinforces emotional trust, presents an overview of the platform, and orients the caregiver without overwhelming them.

---

## Emotional Design Notes
- **Tone**: Warm, affirming, empowering — you're no longer alone.
- **Transition**: Gentle fade from final onboarding message into this overlay.
- **Voice**: Conversational, kind, with subtle encouragement.

---

## Header Copy
> “You’ve built your support circle for [Care Recipient Name]. Let’s show you how CareSupport helps keep it all connected.”

---

## Body Content
- Brief message: “CareSupport helps you coordinate, track, and communicate — all in one place.”
- Key points presented in a friendly checklist:
  - ✅ Stay on top of meds, tasks, and appointments
  - ✅ Keep everyone on the same page with updates
  - ✅ Track health and share key info with doctors
  - ✅ Use your voice to log notes, stories, and changes
- Closing encouragement: “You’re not doing this alone anymore.”

---

## Visual Elements
- Illustration of connected caregivers (diverse avatars)
- Option for light animation showing tasks updating or a caregiver adding a note
- Subtle progress bar or “1 of 3” hint (if part of a walkthrough)

---

## Call-to-Action Buttons
- **Primary**: “Let’s Go” → Leads to Dashboard
- **Secondary**: “Show Me Around” → Begins Tooltip Tour (optional walkthrough)
- **Dismiss**: [X] icon in corner — permanently dismisses overlay unless re-enabled in settings

---

## Tooltip Tour (if user taps “Show Me Around”)
- Highlights key dashboard features:
  - Quick status card
  - Care recipient avatar
  - “Today’s Focus” priority tasks
  - “Quick Add” button
  - Recent updates timeline

---

## Behavior Logic
- Displayed **only once** after onboarding unless user resets app or returns after 30+ days
- Tooltip tour stored in user preferences
- If onboarding steps were skipped, shows a “Continue Setup” reminder in dashboard

---

## Accessibility
- Fully screen reader compatible
- All buttons labeled
- Animation toggle available in accessibility settings
- High contrast mode respected

---

## Developer Notes
- Appears after onboarding confirmation screen transition
- Stored state: `onboardingCompleted = true` and `welcomeOverlaySeen = false`
- Uses system modal component with dismiss logic and guided tour overlay
