
/03_Screen_by_Screen_Specs/CarePlan-Setup/Organizer/[02]_Care-Team-Overview.md

# Screen Title
Your Care Team

# Role
Organizer

## **Purpose**  
To give the Organizer a complete, editable overview of their care circle. This screen allows the Organizer to confirm, adjust, and expand their team, assign appropriate roles, handle pending invites, and identify missing support — all while reinforcing their leadership and role in creating clarity and coordination within the care network.

---

## **UI Components**

### ✅ Progress Indicator  
- Step 2 of 6

### ✅ Visual Snapshot (Optional Section at Top)  
- Horizontal icon tiles showing:
  - Care Recipient
  - Caregiver(s)
  - Supporter(s)
  - Organizer(s)
- Helps Organizer visually scan the overall team balance

### ✅ Pinned Care Recipient Card  
- Non-editable, always displayed at top
- Includes:
  - Name, avatar or initials
  - Role: "Care Recipient"
  - Status: Confirmed
  - Optional: Edit contact info (Phase 2)

### ✅ Scrollable Vertical List of Member Cards  
Each card includes:
- Avatar or initials
- Name
- Role dropdown (editable)
- Status badge:
  - **Confirmed** (green)
  - **Pending** (yellow)
  - **Invite Expired** (red, after 7 days)
- Expand arrow (▾)

**Expanded view adds:**
- Contact details
- Availability notes (text field with voice input)
- Permissions view (Phase 2)
- Resend invitation button (enabled after 24h)
- Remove button (disabled for Organizer unless another exists)

### ✅ Add to Care Circle Button  
When tapped, launches inline form:
- Name
- Email or phone
- Role selector (with popover descriptions)
- Optional invite message
- Smart role suggestion if gap exists
- “Send Invitation” CTA

### ✅ Gap Detection Banners  
Smart suggestions based on care needs captured earlier:
- Example: “It looks like no one is supporting transportation”
- Limit: Max 2 suggestions visible
- CTA: “Add someone for this”

---

## **Primary Actions**

- **Primary CTA**: “Confirm Care Team”
- **Secondary CTA**: “Skip for Now”
- **Tertiary**: “+ Add to Care Circle”

---

## **Emotional Framing**

- **Header Supportive Message**: _“You're the one holding this together — now let's make sure no one has to do it alone.”_
- **Subtext**: _“Each person contributes something unique to [Care Recipient]'s care.”_
- **Gap Tone**: Friendly and helpful, never shaming
- **Voice**: Affirming, steady, and partnership-focused

---

## **Transition Behavior**

- **Comes After**: `[01]_CarePlan-Welcome.md`
- **Leads To**: `[03]_Task-Assignment.md`
- **Skip Behavior**:
  - Dashboard flag set to remind later
  - Toast: _“You can update your care team at any time”_

---

## **Logic & Flow Notes**

### 🔄 **Role Descriptions (Popover Content)**

| Role        | Description                                                                 |
|-------------|------------------------------------------------------------------------------|
| Caregiver   | Provides direct, day-to-day physical, emotional, or logistical care         |
| Supporter   | Offers occasional help (meals, rides, check-ins)                            |
| Organizer   | Coordinates everyone, sends reminders, keeps things on track                |
| Care Recipient | The individual receiving care — their voice is central to everything     |

### 🧠 **Role Editing Constraints**
- Can’t remove Care Recipient
- Can’t demote self from Organizer unless another Organizer exists
- Role changes notify affected users
- Pending members’ roles can be changed before acceptance

### ⚠️ **Gap Detection Logic**
Examples:
- If “mobility” selected during onboarding → suggest adding a Supporter for rides
- If “meals” mentioned → suggest Supporter for food
- If only one caregiver assigned → suggest backup
- Limit to two suggestions at a time

### ✉️ **Invitation Handling**
- Status = Pending (yellow badge)
- Invite auto-expires after 7 days
- Resend button appears after 24h
- Message customization encouraged
- New members auto-added to care_circle with pending_invite_id

---

## **Data Structure (Simplified)**
```json
{
  "care_circle": {
    "members": [
      {
        "user_id": "123",
        "name": "Maria Lopez",
        "role": "caregiver",
        "status": "confirmed",
        "contact": {
          "email": "maria@example.com"
        },
        "availability_notes": "Available Mon–Thu after 6 PM"
      },
      {
        "user_id": null,
        "pending_invite_id": "inv_456",
        "name": "James K.",
        "role": "supporter",
        "status": "pending",
        "contact": {
          "email": "james@example.com"
        },
        "invite_message": "Can you help with Friday errands?"
      }
    ],
    "care_recipient": {
      "user_id": "789",
      "name": "Elizabeth K.",
      "status": "confirmed"
    }
  }
}
```

---

## **Technical Implementation Notes**

- Use expandable iOS list view
- Support offline save of pending invites
- Enable real-time UI update if invite is accepted during session
- Store all role changes in audit log
- Enable future batch invite capability
- All changes are auto-saved as drafts if the user exits mid-process

---

## **Accessibility Considerations**

- All list items keyboard navigable
- Role dropdowns have ARIA roles and labels
- Text input supports screen reader hints
- Minimum 44x44px touch targets
- Color isn’t the only status indicator (use icons + text)
- “Reduce Motion” setting respected on animations

---

## **Completion Flow**

### ✅ On “Confirm Care Team”
- Save updated care_circle object
- Confirm toast: _“Your team is set up!”_
- Transition: Animate to next step `[03]_Task-Assignment.md`

### 💤 On “Skip for Now”
- Save current state, set reminder
- Toast: _“You can return to finalize your team anytime”_
