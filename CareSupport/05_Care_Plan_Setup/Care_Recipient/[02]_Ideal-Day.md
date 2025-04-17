# /03_Screen_by_Screen_Specs/CarePlan-Setup/CareRecipient/[02]_Ideal-Day.md

## Screen Title
How Would Your Best Day Flow?

## Role
Care Recipient

## Purpose
To empower care recipients to share their ideal daily rhythm in a way that respects their autonomy while creating practical guidance for their care team. This screen captures personal preferences for daily activities, rest periods, and care interactions, anchoring the care plan in the recipient's actual desires rather than assumptions.

## UI Components
- Timeline visualization divided into time periods:
  - Morning (6–11 AM)
  - Midday (11 AM–3 PM)
  - Afternoon (3–6 PM)
  - Evening (6–10 PM)
  - Night (10 PM–6 AM)
- Preference cards for each time period:
  - Wake/sleep time preferences (time pickers)
  - Meal preferences (time + brief notes)
  - Activity preferences (multi-select: rest, social, exercise, entertainment)
  - Personal care preferences (assistance level, privacy needs)
- Privacy toggle on each entry: "Only I can see this" / "Share with my care circle"
- Add preference button (+) for each time period
- Progress indicator: Step 2 of 6
- Voice input button prominently displayed on all text fields

## Primary Actions
- Primary CTA: "Save My Daily Preferences"
- Secondary CTA: "Skip for now"

## Voice-to-Text Support
- Prominent microphone icon on all text entry fields
- Voice prompt suggestions: "Tell us about your morning routine"
- Voice-first approach: Microphone positioned before text fields
- Example voice prompts shown when fields are empty:
  - "Say something like: 'I usually wake up around 8 and have tea.'"
  - "Try: 'I prefer assistance with bathing in the morning.'"

## Emotional Framing
- Header: "How Would Your Best Day Flow?"
- Guidance Message: "No one knows your rhythm better than you. Let us know how you like your days to feel — from meals to rest, to the little things that bring comfort."
- Reinforcement: "These details help your care circle support you in ways that truly feel right."
- Tone: Respectful, curious, and empowering without being patronizing

## Transition Behavior
- Comes After: "[01]_CarePlan-Welcome.md"
- Leads To: "[03]_Communication-Style.md"
- Data Flow: Preferences saved to care plan and shared according to privacy settings
- Skip Behavior: Creates minimal default rhythm with generic structure (meals, rest) and flags for dashboard reminder

## Logic & Flow Notes
### Activity Data Structure
Each activity is captured as:
```json
{
  "activity_type": "meal",
  "label": "Lunch",
  "time_period": "midday",
  "time": "12:00",
  "notes": "Prefer something light",
  "recurring": true,
  "shared_with_circle": true
}
```

### Pre-Population Logic
- If care recipient expressed specific needs during onboarding, suggest relevant activities
- If invited by caregiver who completed their flow, show caregiver-suggested activities as optional items
- Default suggestions (easily editable or removable):
  - Meals: Breakfast (8 AM), Lunch (12 PM), Dinner (6 PM)
  - Rest: Afternoon rest (2 PM), Bedtime (9 PM)
  - If medications mentioned in onboarding, suggest medication times

### Privacy Controls
- Default: All entries shared with care circle
- Each entry has individual privacy toggle
- Privacy settings are visually indicated (lock icon)
- Private entries still appear in recipient's view but are hidden from care circle

### Simplification Logic
- If cognitive challenges were indicated during onboarding, show simplified version with fewer options
- If physical challenges were noted, prioritize voice input even more prominently

## Technical Implementation Notes
- Use native iOS time pickers for reliability
- Implement simple iOS card pattern for activities
- Store timeline data in structured format linked to user profile
- Cache changes locally before syncing to server
- Ensure all voice input is processed on-device when possible for privacy
- Use standard iOS animations for smooth transitions

## Accessibility Considerations
- Timeline has text alternative for screen readers
- Large, clear text (minimum 16pt) suitable for older adults
- Voice input prominently featured for those with typing difficulties
- All interactive elements have minimum 44x44px touch targets
- High contrast ratios exceeding WCAG 2.1 AA standards (4.5:1)
- All time pickers keyboard-accessible and properly labeled
- Color is not the only method of conveying information
- Support VoiceOver with proper heading structure
- "Reduce Motion" setting respected for animations

## Completion Flow
### On Save:
- Simple success animation (subtle checkmark or gentle pulse)
- Toast confirmation: "Your preferences are saved"
- Automatic transition to next screen after 1.5 seconds

### On Skip:
- Confirmation toast: "You can set your daily rhythm later from your dashboard"
- System flag for dashboard reminder
- Default rhythm created with editable placeholders
- Immediate transition to next screen

## Final Notes
This screen balances practical utility with emotional respect. By capturing the care recipient's preferred daily rhythm, we enable more dignified, personalized care while reducing uncertainty for the care team. The emphasis on voice input and privacy controls reinforces the care recipient's agency, while the simple timeline structure makes the information actionable without being overwhelming. This is not about rigid scheduling, but about honoring preferences and creating a foundation for care that feels right to the recipient.