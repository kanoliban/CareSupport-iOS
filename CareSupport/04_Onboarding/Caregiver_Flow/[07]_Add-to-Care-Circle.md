# Screen: Add to Care Circle

**Filename:**  
`[07]_Add-to-Care-Circle.md`  
**Location:**  
`/03_Screen_by_Screen_Specs/Caregiver-Flow/`

---

## 🧠 Purpose  
To invite additional supporters (Organizers, Supporters) into the caregiving space, define their roles, and set initial boundaries. This helps share responsibility early and reduces caregiver isolation.

---

## 🖼️ UI Elements
- **Header:** “You're not alone in this.”
- **Prompt:** “Would you like to invite others to help support [Care Recipient’s Name]?”
- **List of Suggested Roles (selectable):**
  - Organizer (e.g., schedules, planning)
  - Supporter (e.g., meals, errands, visits)
  - Other (free text)
- **Input fields:**
  - Name
  - Email or phone
- **Option:** “Skip for now” (recorded as a dashboard reminder)

---

## 🤖 Logic

- If the user invites others:
  - Generate invite link(s) tied to specific role
  - Store pending status until recipient accepts
  - Invitees will be guided through their own tailored onboarding flow
- If skipped:
  - Reminder saved as open onboarding task in dashboard
- Limit to 3 invites in MVP to avoid overwhelming first-time users

---

## 🔐 Permissions & Roles

| Role        | Default Access Level |
|-------------|----------------------|
| Organizer   | View + Edit tasks and schedule |
| Supporter   | View only (can be upgraded later) |
| Other       | Custom (prompt for selection) |

User can later edit access via dashboard.

---

## 🧭 Navigation
- **Primary CTA:** “Send Invites”
- **Secondary CTA:** “Skip for Now”

---

## 💬 Emotional Message
> "Sometimes care takes a village. Let’s help you build yours."

---

## 📝 Notes for Engineering
- Each invite triggers role-specific onboarding path
- If invited person already exists in the system, auto-merge profiles
- Capture invite delivery status (sent, delivered, accepted)
