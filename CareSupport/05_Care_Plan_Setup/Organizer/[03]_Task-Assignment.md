
/03_Screen_by_Screen_Specs/CarePlan-Setup/Organizer/[03]_Task-Assignment.md

# Screen Title
Who Does What

# Role
Organizer

## Purpose
To empower the Organizer to assign initial care responsibilities to members of the care circle, establishing a baseline coordination pattern that reduces ambiguity. This screen bridges planning and execution, allowing the Organizer to convert abstract care needs into concrete assignments while respecting each team member's availability and role.

---

## UI Components

### ✅ Progress Indicator
- Step 3 of 6

### ✅ Task Board
- Two main columns:
  - **Unassigned Tasks**
  - **Assigned Tasks**

### ✅ Task Card Elements
Each card contains:
- Task title
- Category icon + label (Medical, Transportation, Meals, etc.)
- Due date / Recurrence
- Assignee dropdown (or avatar)
- Priority badge (optional)
- Expand arrow to show:
  - Notes
  - Reassignment options
  - Voice input

### ✅ Availability Panel
- Sidebar displaying:
  - Team member avatars
  - Role labels
  - Availability snippet
  - Task count badge

### ✅ Add Task Button
- Opens modal with:
  - Task Title
  - Category
  - Due Date or Recurrence
  - Notes (voice-enabled)
  - Assignee selector

---

## Primary Actions

- **Primary CTA**: “Save Task Assignments”
- **Secondary CTA**: “Skip for now”
- **Tertiary Actions**:
  - “+ Add Task”
  - “Auto-suggest Assignments” (Phase 2)

---

## Emotional Framing

- **Header Supportive Message**: _“You’re creating clarity for everyone — including yourself.”_
- **Subtext**: _“When everyone knows what they're responsible for, care happens more reliably.”_
- **Reminder**: “Assignments can always be adjusted later.”

---

## Transition Behavior

- **Comes After**: `[02]_Care-Team-Overview.md`
- **Leads To**: `[04]_Master-Schedule.md`
- **On Skip**: Tasks saved as-is, reminder flag set, transition to next screen

---

## Logic & Flow Notes

### 🔄 Task Source Logic
Tasks pulled from:
- Care Recipient onboarding
- Caregiver Quick Win
- Daily Rhythm
- Common templates based on situation

### 🧠 Assignment Intelligence
- Suggests based on availability + role
- Warns on overload (e.g., 4+ tasks)
- Soft-match by task type → role
- Allows override with justification note

### 📝 Fallback Tasks
Always show at least 2–3 starter tasks:
- “Schedule weekly check-in”
- “Add medication reminder”
- “Coordinate next grocery trip”

---

## Data Structure (Simplified JSON)
```json
{
  "task_assignments": {
    "tasks": [
      {
        "task_id": "task123",
        "title": "Pick up medications",
        "category": "medical",
        "due_date": "2025-04-20",
        "recurrence": null,
        "assignee_id": "user456",
        "assignee_name": "James Smith",
        "status": "assigned"
      }
    ],
    "assignment_meta": {
      "unassigned_count": 3,
      "assignment_complete": true
    }
  }
}
```

---

## Technical Implementation Notes

- Use native iOS components
- Auto-save on exit or mid-task
- Support undo/redo stack
- Role change audit logging
- Save draft assignments offline
- Intelligent fallback if no data

---

## Accessibility Considerations

- Task cards: ARIA roles, readable status
- Voice input on note field
- Touch targets ≥ 44x44px
- Color-independent state indicators
- VoiceOver compatible assignment summary

---

## Completion Flow

### ✅ On Save
- Confirmation animation
- Save all tasks
- Transition to `[04]_Master-Schedule.md`

### 💤 On Skip
- Reminder set
- Save state as draft
- Toast: “You can assign tasks anytime”

---

## Final Notes
This screen transforms care from abstraction into action. By giving the Organizer a clear, emotionally aware interface for assigning tasks, we help them create a support structure that’s not only efficient but human. MVP 1.0 starts with dropdown-based assignment and pre-generated tasks, with room to expand into full task boards, intelligent suggestions, and real-time syncing in Phase 2.
