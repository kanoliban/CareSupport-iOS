/03_Screen_by_Screen_Specs/CarePlan-Setup/Organizer/[04]_Master-Schedule.md

# Screen Title
The Master Schedule

# Role
Organizer

## Purpose
To establish the central calendar for care coordination, allowing the Organizer to visualize and manage time-based care events. This screen transforms tasks and responsibilities into a structured schedule, reducing ambiguity, preventing overlaps, and improving continuity of care.

---

## UI Components

### ✅ Progress Indicator
- Step 4 of 6

### ✅ Calendar Visualization (MVP)
- Weekly view (default), with horizontal days and vertical 30-minute time slots
- Time grid (6 AM to 10 PM, expandable)
- Color-coded event cards by type: medication, appointments, meals, tasks
- Snap-to-grid drag & drop rescheduling (tablet/desktop only)
- Tap to view/edit event on mobile

### ⏳ Phase 2 Enhancements
- Daily view
- Flexible event window types
- Template scheduling
- iCal/Google Calendar sync

### ✅ Event Cards
Each event includes:
- Title
- Time
- Assignee (avatar + dropdown)
- Type icon
- Conflict indicator (if applicable)
- Tap to expand → show recurrence, location, notes

### ✅ Availability Overlay
- Toggleable layer showing team member availability (from Care Team)
- Grey blocks indicate unavailable
- Light green blocks indicate preferred times

### ✅ Care Recipient Preference Layer
- Soft background tint for aligned times (green)
- Tooltip on hover/tap: “Matches [Name]’s ideal rhythm”

### ✅ Event Management Panel
- Filter events by:
  - Person
  - Type
  - Day
- Quick actions:
  - “+ Add Event”
  - “Generate from Tasks”
- Tap to edit event in full screen view

---

## Primary Actions

- **Primary CTA**: “Confirm Master Schedule”
- **Secondary CTA**: “Skip for Now”
- **Tertiary CTA**: “+ Add Event”

---

## Emotional Framing

- Header: _“Building the rhythm everyone can follow”_
- Subtext: _“A reliable schedule keeps things flowing—and reduces the mental load on everyone.”_
- Reinforcement: _“This can evolve. You’re setting a starting point, not a rulebook.”_

---

## Transition Behavior

- **Comes After**: `[03]_Task-Assignment.md`
- **Leads To**: `[05]_Communication-Protocol.md`
- **Skip Behavior**:
  - Basic schedule auto-generated from assigned tasks
  - Dashboard flag set
  - Toast: “Basic schedule created. You can refine it anytime.”

---

## Logic & Flow Notes

### 📋 Schedule Generation Logic (MVP)
- Imports:
  - Assigned tasks with times
  - Ideal Day preferences
  - Medication Quick Win entries
- Converts to calendar events
- Auto-identifies hard/soft conflicts
- Suggests adjustments

### 📆 Event Types
- One-time (e.g. doctor appointment)
- Recurring (e.g. meds, meals)
- Flexible (Phase 2)

### ⚠️ Conflict Types
- **Hard**: Overlapping events for same person → red highlight, must resolve
- **Soft**: Tight transitions or low availability → yellow, may continue

### 🧠 Conflict Resolution Options
- Reassign
- Reschedule
- “Handle later” tag
- Phase 2: Split responsibility

---

## 📊 Data Structure
```json
{
  "care_schedule": {
    "events": [
      {
        "event_id": "evt123",
        "title": "Morning Medication",
        "event_type": "medication",
        "start_time": "08:00",
        "end_time": "08:15",
        "recurrence": {
          "pattern": "daily",
          "days": ["monday", "tuesday", "wednesday", "thursday", "friday", "saturday", "sunday"]
        },
        "assignee_id": "user456",
        "source": "assigned_task",
        "status": "confirmed"
      }
    ],
    "conflicts": [
      {
        "conflict_id": "c1",
        "event_ids": ["evt123", "evt999"],
        "type": "assignee_overlap",
        "resolved": false
      }
    ],
    "schedule_meta": {
      "generated_from_tasks": true,
      "has_unresolved_conflicts": true,
      "last_updated": "2025-04-14T18:30:00Z"
    }
  }
}
```

---

## Technical Implementation Notes

- Use iOS calendar UI components (customized)
- Default: 30-minute time slots (configurable in future)
- All events saved as structured objects with timestamps
- Events sync to dashboard
- Save on edit; autosave in background

---

## Accessibility Considerations

- Time blocks: 44x44px minimum
- Color-coding paired with icons and text
- Drag & drop alternatives for keyboard users
- Full screen reader support:
  - Event title, type, time, assignee, conflicts
- Calendar supports “Reduce Motion”
- Tab order is logical by event time

---

## Completion Flow

### ✅ On “Confirm Master Schedule”
- Validate for unresolved hard conflicts
- Save events
- Success animation: gentle calendar pulse
- Toast: “Master Schedule created”
- Transition to Communication Protocol screen

### 💤 On “Skip for Now”
- Generate baseline schedule
- Set reminder flag
- Transition forward

---

## Final Notes
This is the operational heart of care coordination. By translating tasks and rhythms into shared time, the Organizer gives shape to the team’s intentions. This screen is designed to be empowering but not overwhelming—supporting confident decisions while leaving room for future flexibility.