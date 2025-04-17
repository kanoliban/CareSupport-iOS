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

# `/03_Screen_by_Screen_Specs/CarePlan-Setup/Supporter/[01]_Welcome-Circle.md`

## Screen Title
You're Part of the Team

## Role
Supporter

## Purpose
To welcome the Supporter to the care circle, affirming their value while clearly establishing the low-pressure nature of their role. This screen sets expectations that their contribution matters but can be flexible and occasional, respecting their boundaries while helping them feel part of the care team.

## UI Components
- **Progress indicator**: Step 1 of 4
- **Welcome header** with Supporter's name
- **Visual representation**: Simple illustration of a care circle with the Supporter included
- **Message card**: Warm welcome with clear role definition
- **Expectation clarification cards**:
  - "Help when you can"
  - "Opt out when you can't"
  - "Choose how you want to contribute"
- **Primary CTA button**
- **Secondary "Skip for now" link** (de-emphasized)

## Primary Actions
- **Primary CTA**: "Set My Preferences"
- **Secondary Link**: "Skip for now"

## Emotional Framing
- **Header**: "You're Part of the Team"
- **Appreciation Message**: "Small things make a big difference."
- **Role Clarification**: "You've been invited to support [Care Recipient Name]. That might mean bringing meals, helping with errands, or simply being available if something comes up."
- **Pressure Reduction**: "You're not expected to do it all—just what feels right for you."
- **Tone**: Warm, appreciative, reassuring about boundaries

## Transition Behavior
- **Comes After**: Final screen of Supporter onboarding
- **Leads To**: "[02]_Help-Ways.md"
- **Alternative Path**: If "Skip for now" is selected, flags system and transitions to simplified dashboard with reminder to complete setup
- **Data Flow**: Initializes `supporter_preferences` object

## Logic & Flow Notes
- **Personalization**: Greeting includes Supporter's name from onboarding
- **Context Adaptation**:
  - If invited by caregiver or organizer, references their connection: "[Inviter Name] has invited you to help support [Care Recipient]."
  - If self-initiated, emphasizes value of stepping forward: "Thank you for stepping up to support [Care Recipient]."
- **Visual Continuity**: Maintains visual language from Supporter onboarding path
- **Time Estimate**: "This will only take about 2 minutes."

## Technical Implementation Notes
- Use consistent UI components from onboarding
- Initialize supporter preferences object
- Track setup progress for resume functionality
- Store skip state if user opts to return later

## Accessibility Considerations
- All text meets WCAG 2.1 AA standards for contrast (minimum 4.5:1)
- Illustrations have text alternatives
- CTA button has minimum 44x44px touch target
- Progress indicator has text alternative for screen readers
- Keyboard navigation fully supported
- All interactive elements have clear focus states
- Support "Reduce Motion" setting for animations

## Completion Flow
- **On Continue**: Smooth transition animation to next screen
- **On Skip**: 
  - Gentle acknowledgment toast: "You can set your preferences anytime"
  - Dashboard flag to remind about incomplete setup
  - Simplified dashboard view with core functionality

## Final Notes
This welcome screen sets the tone for the Supporter's experience by acknowledging their contribution while respecting their boundaries. The emphasis on flexibility and choice reassures them that their support role is valued but won't overwhelm them. This approach encourages participation by removing pressure and establishing clear expectations from the start.
