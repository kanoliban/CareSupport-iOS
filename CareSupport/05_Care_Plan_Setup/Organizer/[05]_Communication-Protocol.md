# `/03_Screen_by_Screen_Specs/CarePlan-Setup/Organizer/[05]_Communication-Protocol.md`

## ✅ **Summary of the Merged MVP 1.0 Spec**

### 💡 Final Title
**How We Stay in Sync**  
(_Claude’s title, “Keeping Everyone Connected,” is excellent, but ChatGPT’s wins with clarity + tone consistency_)

---

## 🎯 **Purpose**
To establish a clear, respectful, and sustainable system of communication for the care circle — helping everyone stay informed, aligned, and emotionally supported without overwhelm. This screen enables the Organizer to define how updates are shared, when, with whom, and through which methods.

---

## 📦 UI Components

### ✅ Progress Indicator
- Step 5 of 6
- Visual bar with label: “Step 5: Communication Protocol”

### ✅ Communication Update Types
Cards for:
- **Daily or Weekly Updates**
- **Emergency Alerts**
- **Care Recipient Check-ins**
- **General Coordination**

Each card includes:
- Frequency selector (`Daily`, `Weekly`, `As Needed`)
- Delivery methods (App, SMS, Email)
- Recipient selector (multi-select from care circle)
- “Avoid times” option per update type (tap to reveal time range picker)

### ✅ Delivery Preferences Grid
A matrix view (Phase 2 can enhance with more granular control):
| Team Member | Preferred Method | Do Not Disturb | Notes              |
|-------------|------------------|----------------|--------------------|
| Maria       | App              | 9PM–7AM        | Text first         |
| James       | SMS              | 10PM–6AM       | OK to call in AM   |

Editable per row. Taps open modals for time pickers.

### ✅ Emergency Communication Logic
- Drag-and-drop priority list: Who to notify first, second, third
- Escalation delay: “Wait X minutes before contacting next person”
- Supports voice or text confirmation on delivery

### ✅ Preview Update Button
Tertiary action: **“Preview Sample Message”**  
Shows a simulated update (e.g., “Daily Update”) with method, time, and visibility — builds trust and clarity.

---

## 📍 **Primary Actions**
- **Primary CTA**: “Save Communication Settings”
- **Secondary CTA**: “Skip for Now”
- **Tertiary**: “Preview Sample Message”

---

## ❤️ Emotional Framing

- **Header Message**: _“Good communication saves time, avoids stress.”_
- **Context Message**: _“Clear expectations help everyone stay connected without feeling overwhelmed.”_
- **Reinforcement**: _“This is how we build trust and keep care running smoothly.”_
- **Tone**: Affirming, respectful, organized, and emotionally attuned

---

## 🔀 Transition Behavior
- **Comes After**: `[04]_Master-Schedule.md`
- **Leads To**: `[06]_CarePlan-Complete.md`
- **If Skipped**:
  - Sets `communication_protocol_complete: false`
  - Dashboard shows reminder
  - Creates default update pattern:
    - Weekly update to Caregiver(s)
    - App notifications only
    - No emergency config

---

## 🧠 Logic & Flow Notes

### Update Types

| Type                 | Purpose                                 | Default Recipients      |
|----------------------|------------------------------------------|--------------------------|
| Daily/Weekly Update | General status + task wrap-up            | Organizer, Caregiver     |
| Emergency Alert     | Urgent update needing immediate response | Primary Caregiver        |
| Check-in            | Wellbeing update from/to recipient       | Organizer + Recipient    |
| Coordination        | Task handoff / adjustments               | Whole team or per-role   |

System may suggest recipients based on role, e.g.:
- "You haven’t selected a recipient for Emergency Alerts"
- "Would you like to include [Supporter Name] on meal updates?"

### Privacy Handling
- Pulls from Care Recipient’s privacy settings
- Warns if conflicting choices (e.g., sharing health info they marked private)
- “Only I can see this” toggle for individual update types (Phase 2)

---

## 🧱 Technical Implementation Notes

### Simplified Data Model (MVP 1.0)
```json
{
  "communication_protocol": {
    "update_defaults": {
      "daily_updates": {
        "frequency": "weekly",
        "time": "18:00",
        "days": ["monday", "thursday"],
        "recipients": ["user123", "user456"],
        "methods": ["app", "email"]
      },
      "emergency_alerts": {
        "priority": ["user123", "user789", "user456"],
        "escalation_delay_minutes": 15
      }
    },
    "member_preferences": {
      "user123": {
        "preferred_method": "app",
        "do_not_disturb": [{"start": "21:00", "end": "07:00"}],
        "notes": "Call only if urgent"
      }
    },
    "protocol_meta": {
      "created_by": "organizer_id",
      "last_updated": "2025-04-13T20:30:00Z",
      "status": "complete"
    }
  }
}
```

---

## 🦾 Accessibility Considerations

- Voice input available for custom messages
- ARIA-labeled controls for time pickers
- Drag-and-drop fallback using keyboard (arrow keys + enter)
- All UI touch targets ≥ 44px
- “Reduce Motion” setting respected on all animations
- App accessibility permissions respected (esp. for notifications)

---

## ✅ Completion Flow

### On Save:
- Save data to `communication_protocol` object
- Run conflict validation (e.g., duplicate escalation contacts)
- Show confirmation toast: _“Communication settings saved”_
- Smooth animation → transition to `[06]_CarePlan-Complete.md`

### On Skip:
- Toast: _“Default communication plan applied. You can refine this later.”_
- Proceed with generic update logic and dashboard flag

---

## 🔚 Final Notes
This screen makes invisible labor visible. The Organizer creates not just a plan, but **a system for clarity, trust, and resilience** — one that can scale, flex, and adapt.

It doesn't overengineer — it simply answers:  
**Who gets what update, when, and how?**
