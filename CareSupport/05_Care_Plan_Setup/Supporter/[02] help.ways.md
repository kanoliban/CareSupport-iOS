# Supporter Care Plan Setup - Full Specifications

This document contains the complete production-ready specifications for the Supporter Care Plan Setup flow in the CareSupport iOS app. The Supporter flow is designed to be lightweight, emotionally intelligent, and respectful of boundaries, recognizing that Supporters are often extended family, neighbors, or friends who are willing to help but not looking to be overwhelmed.

## Overall Design Philosophy

The Supporter Care Plan Setup is:
- **4 steps total** (shorter than other roles)
- Emphasizing **opt-in**, **autonomy**, and **clarity**
- Capturing **task preferences**, **availability**, and **communication comfort**
- Avoiding overwhelming or assuming high involvement
- Ending with a clear preview of how they'll contribute

---

# `/03_Screen_by_Screen_Specs/CarePlan-Setup/Supporter/[02]_Help-Ways.md`

## Screen Title
Ways You're Open to Support

## Role
Supporter

## Purpose
To enable Supporters to specify the types of assistance they're comfortable providing, creating clarity about boundaries and preferences. This screen empowers Supporters to opt into specific support activities that match their abilities, availability, and comfort level, establishing expectations that respect their autonomy.

## UI Components
- **Progress indicator**: Step 2 of 4
- **Task preference grid**: Card-based multi-select interface
  - Each card includes:
    - Icon representing task type
    - Task category label
    - Brief description
    - Multi-select checkbox
  - Task categories include:
#### Predefined Cards:
- 🛒 **Meals & Groceries**  
  _Shopping, food prep, or meal drop-offs_
  
- 🚗 **Transportation**  
  _Driving to appointments, errands_

- ☎️ **Social Check-Ins**  
  _Calls, texts, or brief visits_

- 📦 **Errands**  
  _Mail, supplies, pickup/drop-off_

- 🧑‍🤝‍🧑 **Companionship / Visits**  
  _Just being there, talking, hanging out_

- 🧾 **Administrative Help**  
  _Forms, bills, calls, or paperwork_

- ➕ **Other**  
  - Custom text field appears when selected  
  - With voice input option

- **Selected tasks summary**: Dynamic count of selected preferences
- **Microcopy reminder**: "You can always adjust these later"
- **Primary CTA button**
- **Secondary "Skip for now" link** (de-emphasized)

## Primary Actions
- **Primary CTA**: "Save Support Preferences"
- **Secondary Link**: "Skip for now"

## Voice-to-Text Support
- Available for "Other" category text field
- Microphone icon with tap-to-speak functionality
- Voice prompt: "Tell us other ways you might be able to help"

## Emotional Framing
- **Header**: "Ways You're Open to Support"
- **Supportive Context**: "Everyone has different strengths. Let us know where you feel most comfortable helping."
- **Pressure Removal**: "This isn't a commitment—just a way to match you with the right opportunities."
- **Tone**: Encouraging without expectation, focusing on strengths rather than obligations

## Transition Behavior
- **Comes After**: "[01]_Welcome-Circle.md"
- **Leads To**: "[03]_Availability.md"
- **Skip Behavior**: If skipped, creates empty preferences set with default "Social check-ins" enabled
- **Data Flow**: Updates `supporter_preferences.task_types` with selected options

## Logic & Flow Notes

### Task Type Definitions
Each task category includes:
- Icon representation
- Brief description of what it entails
- Typical time commitment
- Examples of specific tasks

### Selection Behavior
- Multi-select allows any number of preferences
- At least one selection recommended before proceeding
- If no selections made but "Save" tapped, gentle suggestion appears
- "Other" selection reveals text field for custom support types

### Pre-Selection Logic
- If invited by Organizer with specific support needs mentioned, those categories subtly highlighted
- Based on care recipient needs, relevant categories may be emphasized

## Technical Implementation Notes

### Data Structure
```json
{
  "supporter_preferences": {
    "task_types": [
      {
        "type": "meals_groceries",
        "selected": true,
        "custom_notes": ""
      },
      {
        "type": "transportation",
        "selected": true,
        "custom_notes": ""
      },
      {
        "type": "social_checkins",
        "selected": false,
        "custom_notes": ""
      },
      {
        "type": "errands",
        "selected": false,
        "custom_notes": ""
      },
      {
        "type": "companionship",
        "selected": true,
        "custom_notes": ""
      },
      {
        "type": "administrative",
        "selected": false,
        "custom_notes": ""
      },
      {
        "type": "other",
        "selected": false,
        "custom_notes": "Helping with gardening occasionally"
      }
    ],
    "updated_at": "2025-04-14T16:30:00Z"
  }
}
```

### Implementation Considerations
- Use native iOS card selection components
- Implement subtle visual feedback on selection/deselection
- Store partial selections if user navigates away
- Support offline selection with later sync
- Enable analytics to track most common support preferences

## Accessibility Considerations
- Task cards have clear ARIA roles and labels
- Selection state is both visually apparent and announced to screen readers
- Multi-select behavior clearly communicated
- All interactive elements have minimum 44x44px touch targets
- High contrast for selected/unselected states
- Voice input available for custom task description
- Support keyboard navigation through grid
- Focus management follows logical order

## Completion Flow
- **On Save**:
  - Visual confirmation of saved preferences
  - Subtle animation acknowledging selection
  - Automatic progression to next screen

- **On Skip**:
  - Toast notification: "We've set some default preferences you can update later"
  - System creates default preference set with "Social check-ins" enabled
  - Proceeds to next screen

## Final Notes
This screen empowers Supporters to define their contribution on their terms, reducing anxiety about overcommitment while still encouraging participation. The multi-select approach with clear descriptions helps Supporters identify where they can be most effective, creating better matches between needs and offers. The emphasis on being able to adjust preferences later reinforces flexibility and reduces pressure around initial selections.
