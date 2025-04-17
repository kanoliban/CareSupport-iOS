
# [04]_Daily-Rhythm.md

## Screen Title
**A Typical Day in Care**

## Role
**Caregiver**

## Purpose
To help caregivers establish a foundational daily structure for the care recipient’s routine. This reduces decision fatigue and provides a dependable rhythm for caregiving, while laying the groundwork for a coordinated and adaptable care plan.

---

## UI Components

### 📊 Time Period Sections (static, toggleable)
- **Morning (6–11 AM)**
- **Midday (11 AM–3 PM)**
- **Afternoon (3–6 PM)**
- **Evening (6–10 PM)**
- **Night (10 PM–6 AM)**

Each section includes a collapsible set of pre-populated or user-added activities.

### ➕ Activity Cards
Each time slot contains activity cards, such as:
- Meals
- Medications
- Check-ins
- Rest/Breaks
- Personal Care
- Custom Activities (added by user)

Card elements include:
- Icon + Title
- Time
- Repeating toggle
- Optional Notes

### 📝 Activity Editor Modal
Accessible by tapping on any activity:
- **Activity Name**
- **Time Selector** (hour/minute)
- **Recurring Toggle** (daily by default)
- **Notes Field** (voice-enabled)

### Other Interface Elements
- Step indicator: **Step 4 of 6**
- CTA: **"Save Daily Rhythm"**
- Secondary CTA: **"Skip for now"**
- Tertiary CTA: **"Add Another Activity"**

---

## Voice-to-Text Support
- **Enabled for**: Activity Name, Notes field
- **Trigger**: Microphone icon
- **Prompt**: “What usually happens at this time?”

---

## Emotional Framing

### Header
"A daily rhythm brings peace of mind."

### Subtext
"Even simple routines help make each day more predictable—for you and for [Care Recipient Name]."

### Encouragement
"Focus on what matters most. You can refine this later."

### Tone
Supportive, pragmatic, non-judgmental.

---

## Transition Behavior

### Comes After
`[03]_Quick-Win.md`

### Leads To
`[05]_Circle-Involvement.md`

### Skip Logic
- System sets skip flag
- Dashboard will surface soft reminder
- Default rhythm is generated visually

---

## Logic & Flow Notes

### Pre-Population Logic

| **Primary Challenge** | **Pre-Populated Activities** |
|-----------------------|------------------------------|
| Medication            | Morning/Evening pills (8 AM / 8 PM) |
| Task Coordination     | Inserted from Quick Win (e.g., meal task) |
| Appointments          | Scheduled from Quick Win + prep/rest slots |
| Communication         | Check-in + Update time |
| Something Else        | Baseline rhythm: meals + rest periods |

---

### Basic Activity Templates

| **Activity** | **Default Time** | **Recurring** |
|--------------|------------------|----------------|
| Breakfast    | 8:00 AM          | Daily          |
| Lunch        | 12:00 PM         | Daily          |
| Dinner       | 6:00 PM          | Daily          |
| Morning Pills| 8:00 AM          | Daily          |
| Evening Pills| 8:00 PM          | Daily          |
| Check-In     | 9:00 AM & 7:00 PM| Daily          |
| Rest Period  | 2:00 PM & 9:00 PM| Daily          |

---

### Editing Features
- Tap to edit any activity
- Add/remove new activities
- Optional notes on each card
- Time input via native picker

---

## Completion Flow

### On Save
- Subtle success animation (pulse or tick)
- Toast: "Daily rhythm saved"
- Auto-redirect to `[05]_Circle-Involvement.md` after 1.5s

### On Skip
- Toast: "You can set up your daily rhythm later"
- Continue immediately to next step
- Placeholder rhythm created for dashboard use

---

## Technical Implementation Notes

### Data Schema

```json
{
  "activity_type": "medication",
  "title": "Morning Pills",
  "time": "08:00",
  "recurring": true,
  "days": ["monday", "tuesday", "wednesday", "thursday", "friday", "saturday", "sunday"],
  "notes": "Take with food"
}
```

### Other Notes
- Local state caching for interrupted sessions
- iOS native pickers preferred for time
- Activities sync to backend and dashboard

---

## Accessibility Considerations

| Feature | Implementation |
|---------|----------------|
| **Labels** | All sections + activities have ARIA labels |
| **Focus States** | Visibly outlined & programmatically reachable |
| **Voice UI** | Consistent mic icon; toggles accessible |
| **Keyboard Nav** | All fields reachable in logical sequence |
| **Contrast** | All elements follow WCAG 2.1 AA |
| **Touch Targets** | Minimum 44x44px for all interactive items |

---

## Final Notes

This screen serves as a light but powerful way to translate a caregiver’s concern into routine structure. By building from prior actions (Quick Win) and using clear visual categories, this design reinforces a sense of momentum without adding pressure. Whether it’s a detailed plan or just 2–3 anchor activities, this step helps create continuity between intention and action in the daily care journey.
