# `/03_Screen_by_Screen_Specs/CarePlan-Setup/Supporter/[03]_Availability.md`

## Screen Title
When Are You Usually Available?

## Role
Supporter

## Purpose
To capture the Supporter's general availability patterns in a low-pressure, flexible way. This screen acknowledges that Supporters have their own lives and commitments while helping match their availability to care needs, creating expectations that respect their time boundaries.

## UI Components
- **Progress indicator**: Step 3 of 4
- **Day selector**: Multi-select weekday buttons (Monday through Sunday)
- **Time window toggles**:
  - Morning (6AM-12PM)
  - Afternoon (12PM-5PM)
  - Evening (5PM-10PM)
- **Frequency indicator**:
  - Radio buttons: Weekly, Monthly, Occasionally
- **Availability notes**: Free text field with voice input option
  - Placeholder: "E.g., Mornings are better, but flexible some weekends"
- **Calendar visualization**: Simple visual representation of selected availability
- **Primary CTA button**
- **Secondary "Skip for now" link** (de-emphasized)

## Primary Actions
- **Primary CTA**: "Save My Availability"
- **Secondary Link**: "Skip for now"

## Voice-to-Text Support
- Available for availability notes field
- Microphone icon with tap-to-speak functionality
- Voice prompt: "Tell us about your general availability"

## Emotional Framing
- **Header**: "When Are You Usually Available?"
- **Supportive Context**: "Everyone has different schedules. Let us know when you might have time to help."
- **Flexibility Emphasis**: "This is just a general guide—not a commitment to these specific times."
- **Tone**: Casual, flexible, emphasizing patterns rather than commitments

## Transition Behavior
- **Comes After**: "[02]_Help-Ways.md"
- **Leads To**: "[04]_Communication-Complete.md"
- **Skip Behavior**: If skipped, creates default "occasional" availability with no specific days/times
- **Data Flow**: Updates `supporter_preferences.availability` with selected patterns

## Logic & Flow Notes

### Availability Model
- Captures general patterns rather than specific calendar blocks
- Combines days, time windows, and frequency
- Supplemented by free-text notes for nuance
- Visualized in simplified calendar view

### Selection Behavior
- Multiple days can be selected
- Multiple time windows can be toggled
- Only one frequency option can be selected
- No minimum selection required to proceed
- Calendar visualization updates dynamically with selections

### Default Suggestions
- No pre-selected days or times
- Default frequency: "Occasionally"
- If no selections made, system records "flexible, occasional availability"

## Technical Implementation Notes

### Data Structure
```json
{
  "supporter_preferences": {
    "availability": {
      "days": ["monday", "wednesday", "saturday"],
      "time_windows": ["morning", "evening"],
      "frequency": "weekly",
      "notes": "Available most Wednesday mornings and some weekends",
      "updated_at": "2025-04-14T16:45:00Z"
    }
  }
}
```

### Implementation Considerations
- Use native iOS selection components
- Implement real-time visual feedback when updating calendar visualization
- Store partial selections if user navigates away
- Support offline selection with later sync
- Simple rather than complete calendar implementation for MVP
- Phase 2: More detailed availability calendar integration

## Accessibility Considerations
- Day selectors have proper ARIA roles and states
- Time window toggles clearly labeled for screen readers
- Calendar visualization has text alternative description
- All interactive elements have minimum 44x44px touch targets
- Selection states indicated by both color and shape/icon
- Voice input prominently available for notes
- Support keyboard navigation through all selectors
- Focus management follows logical order

## Completion Flow
- **On Save**:
  - Visual confirmation of saved availability
  - Subtle animation acknowledging selection
  - Automatic progression to next screen

- **On Skip**:
  - Toast notification: "We've set default occasional availability"
  - System creates default availability with "occasional" frequency
  - Proceeds to next screen

## Final Notes
This screen captures availability in a way that feels lightweight and flexible, appropriate to the Supporter role. By focusing on general patterns rather than specific commitments, it reduces pressure while still providing useful information for coordination. The simple visual representation helps Supporters understand how their availability will be interpreted, while the notes field allows for the nuance that real-life schedules often require.
