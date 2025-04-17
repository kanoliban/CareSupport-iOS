
# Screen Specification: [05]_Circle-Involvement.md

## Screen Title
Who's Helping With What

## Role
Caregiver

## Purpose
To help caregivers assign and clarify responsibilities among their care circle members, fostering collaboration, reducing personal burden, and making support roles tangible. This screen continues the Care Plan Setup and begins activating the care network.

---

## 🧩 UI Components

### Header
- Title: “Who's Helping With What”
- Subheader: "You don't have to do this alone. Let’s match your team to the tasks."

### Member Carousel
- Horizontally scrollable avatars of care circle members
- Label beneath each: Name + Role
- Optional Status Indicator (Phase 2): "Has tasks", "No tasks yet"

### Task Assignment Cards
- Presented by category or time block (e.g., “Today’s Tasks”, “Meals”, “Appointments”)
- For each card:
  - Task name
  - Suggested assignee (if available)
  - Dropdown to assign task
  - Option to leave note for assignee (voice-enabled)
  - Visual confirmation once assigned

### Add New Task Button
- Opens modal with:
  - Task name
  - Assign to (dropdown)
  - Due date/time
  - Notes (optional)
  - Repetition toggle (Phase 2)

### Share Confirmation
- Once assignments are made:
  - Summary: “3 tasks assigned across your care circle.”
  - CTA: “Send Plan to Team”
  - Optional toggle: “Require acknowledgment” (Phase 2)

---

## ✅ Primary Actions

- **Primary CTA**: "Send Plan to Team"
- **Secondary Action**: "I'll Assign Later"
- **Tertiary CTA**: "Add New Task"

---

## 🗣️ Emotional Framing

- **Header Reinforcement**: "Sharing the load makes care sustainable."
- **Encouragement**: "You’ve taken the first step. Now, let’s invite others to walk with you."
- **Tone**: Empowering, collaborative, grounded in gratitude

---

## 📍 Transition Behavior

- **Comes After**: `[04]_Daily-Rhythm.md`
- **Leads To**: `[06]_CarePlan-Complete.md` or Dashboard if setup complete
- **If Skipped**: System sets reminder and flags care circle as incomplete in dashboard

---

## 🔁 Data Flow

- Pulls:
  - Care circle members from onboarding
  - Tasks from Quick Win and Daily Rhythm
- Outputs:
  - Assignment data (who is responsible for what task)
  - Notes tied to each assignment
  - Task metadata (urgency, recurrence)

---

## 🔄 Logic & Flow Notes

### If No Care Circle Added
- Prompt: “You haven’t added anyone to your care circle yet.”
- CTA: “Invite Someone” → launches invite flow
- Option: “Assign tasks to yourself for now”

### Smart Suggestions (Phase 2)
- Suggest assignments based on prior engagement
- Auto-assign tasks left unassigned for >3 days

---

## 💬 Accessibility Considerations

- Minimum 44x44px touch targets
- Full keyboard navigation enabled
- ARIA labels for member avatars and dropdowns
- Voice notes accessible via long-press or microphone icon
- Screen reader announcements on assignment
- High-contrast mode compliant

---

## 🧠 Technical Implementation Notes

- Save task/assignment pairings in structured format:
```json
{
  "task_id": "abc123",
  "task_name": "Meal Delivery",
  "assignee": "John Doe",
  "due_date": "2025-04-13T18:00:00Z",
  "notes": "Leave at the porch",
  "acknowledgment_required": false
}
```

- Cache all changes locally until submission
- Allow undo of latest assignment (Phase 2)

---

## 🎉 Completion Flow

- Upon “Send Plan to Team”:
  - Micro-animation (task cards fly to avatars)
  - Confirmation message: “Care Plan shared with your circle”
  - CTA: “Continue Setup” → `[06]_CarePlan-Complete.md`

- Upon “I’ll Assign Later”:
  - Gentle message: “You can assign tasks at any time”
  - CTA: “Finish Care Plan” → `[06]_CarePlan-Complete.md`

---

## 🔚 Final Notes

This screen represents the moment where caregivers invite their team to actively participate. It balances simplicity with emotional impact by showing how care becomes sustainable through teamwork. Clear visibility into who is helping with what, combined with gentle nudges and thoughtful defaults, transforms support from theory into practice—while avoiding guilt or micromanagement.
