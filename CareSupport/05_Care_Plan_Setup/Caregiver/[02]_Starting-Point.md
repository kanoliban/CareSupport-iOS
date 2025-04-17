# [02] Starting Point — Caregiver Flow

## 🧾 Screen Title
**Starting With What Matters Most**

## 👤 Role
Caregiver

## 🎯 Purpose
To personalize the Care Plan Setup experience based on the primary challenge identified during onboarding, creating a sense of immediate relevance and value by addressing the caregiver's most pressing concern first.

---

## ✅ Primary Actions

- **Primary CTA**: “Yes, start here”
- **Secondary CTA**: “I want to choose a different focus”

---

## 🖍 Inputs / Fields

If secondary button is tapped, user is shown a list of alternative focus areas:
- Medication Management
- Appointment Tracking
- Task Coordination
- Family Communication
- Custom Setup (Free text entry)

---

## 🎤 Voice-to-Text Support

- Available for all challenge descriptions, including predefined and custom

---

## ❤️ Emotional Framing

- **Acknowledgment**: “You mentioned [primary challenge] is your biggest concern right now.”
- **Validation**: “Many caregivers struggle with this. Let's start by making it easier.”
- **Empowerment**: “We’ll build your plan around what matters most to you.”
- **Tone**: Understanding, responsive to needs, solution-focused

---

## 🔁 Transition Behavior

- **Comes After**: `[01]_CarePlan-Welcome.md`
- **Leads To**: `[03]_Quick-Win.md` with context of selected challenge
- **Data Flow**: `primary_challenge` selection is passed forward to determine template

---

## 🔀 Logic & Flow Notes

### Challenge Mapping

| Challenge Selected       | Focus Card Displayed          | Setup Flow Triggered              |
|--------------------------|-------------------------------|-----------------------------------|
| Medication Management    | Medication Tracker            | Add Medication Flow               |
| Appointment Scheduling   | Calendar Setup                | Create Appointment Flow           |
| Family Coordination      | Care Taskboard                | Task Assignment Flow              |
| Communication With Family| Updates Center                | Communication Settings Flow       |
| Something Else           | Custom Setup Path             | Open-Ended Task Start             |

- If onboarding `primary_challenge` was skipped:
  - Show fallback prompt: “Let’s choose a place to begin building your care plan.”
  - Present all challenge cards for selection.

---

## 🧠 Dynamic Content

- Screen text dynamically inserts the `primary_challenge` from onboarding data.
- Visual card previews update based on selection:
  - Pill organizer → Medication
  - Calendar → Appointment
  - Care circle graphic → Coordination
  - Message icon → Communication

---

## 🗂 Component State

- Confirmed challenge proceeds to next screen
- New selection updates `primary_challenge` and passes it forward
- If user navigates away and returns, selection state is preserved

---

## 🧩 Visual Layout Specs

### Challenge Card

- **Icon**: Represents challenge visually
- **Title**: 18pt Semi-Bold
- **Description**: One-liner
- **Preview Graphic**: Related to feature
- **Selection Style**: Highlighted border, checkmark

### Card Dimensions

- Height: 120dp
- Width: Full width minus 32dp margin
- Corner Radius: 12dp
- Spacing Between Cards: 16dp

---

## ♿ Accessibility

- ARIA roles: `role="radiogroup"` for group, `role="radio"` for each card
- Selected state: `aria-checked="true"`
- Tap targets: Minimum 44x44px
- Text contrast ratio: 4.5:1 or higher
- Focus indicators and screen reader announcements on selection change

---

## 🔧 Technical Implementation

- Retrieve `primary_challenge` from onboarding store
- Use template strings for dynamic content
- Truncate long strings after 40 characters
- Announce card selection and state changes via screen reader
- Send updated challenge via navigation param to `[03]_Quick-Win.md`
