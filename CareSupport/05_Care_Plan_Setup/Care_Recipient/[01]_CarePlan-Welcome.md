
# /03_Screen_by_Screen_Specs/CarePlan-Setup/CareRecipient/[01]_CarePlan-Welcome.md

## Screen Title
Your Care, Your Way

## Role
Care Recipient

## Purpose
To warmly transition the care recipient from onboarding to an active role in shaping their care experience. This screen establishes that their preferences, needs, and dignity are central to the care plan, reinforcing their agency while introducing the purpose of the setup process.

## UI Components
- **Welcoming header** with care recipient's name
- **Progress indicator** showing "Step 1 of 6"
- **Warm illustration** showing person in control of their environment
- **Message card** with emotional framing
- **Voice option introduction** with microphone icon animation
- **Primary CTA button**
- **Secondary "Skip for now" option** (de-emphasized)

## Primary Actions
- **Primary CTA**: "Tell Us Your Preferences"
- **Secondary Link**: "I'll do this later" (visually de-emphasized)

## Voice-to-Text Support
- **Introduction text**: "Throughout setup, you can tap the microphone to speak instead of type"
- **Demonstration microphone icon** with subtle pulse animation
- **Voice input preview**: Brief example of how voice input works

## Emotional Framing
- **Header**: "Your Care, Your Way"
- **Continuity Message**: "You're not just receiving care—you're shaping it."
- **Agency Reinforcement**: "Your preferences and choices matter. They will guide how support is provided."
- **Privacy Assurance**: "You control who sees what information about your care."
- **Tone**: Empowering, dignified, respectful of autonomy, warm but not patronizing

## Transition Behavior
- **Comes After**: Final emotional screen of Care Recipient onboarding
- **Leads To**: "[02]_Ideal-Day.md" when primary CTA is tapped
- **Alternative Path**: If "I'll do this later" is selected, flags system and transitions to simplified dashboard with reminder to complete setup
- **Data Continuity**: References privacy preferences and expressed needs from onboarding

## Logic & Flow Notes
- **Personalization**: Greeting includes care recipient's name from onboarding
- **Relationship Context**: If invited by a caregiver, shows their name: "Sarah has invited you to share how you prefer to receive care"
- **Onboarding Continuity**: References any privacy preferences already established during onboarding
- **Voice Prominence**: Voice input option is highlighted as primary input method throughout the flow
- **Simplified Entry**: Ensure this screen is not text-heavy; focus on clear next steps
- **Time Estimate**: Include subtle text: "This will take about 5 minutes and will help everyone support you better"

## Accessibility Considerations
- **Large, clear text**: Minimum 16pt font size suitable for older adults or those with vision impairments
- **High contrast ratio**: Exceeding WCAG 2.1 AA standards (4.5:1 minimum)
- **Voice instruction**: Clear introduction to voice features as accessibility enhancement
- **Screen reader optimization**: Proper heading structure with descriptive labels
- **Touch targets**: Sized appropriately (minimum 44x44px) for users with dexterity challenges
- **Keyboard navigation**: Full support for those using adaptive technologies
- **Reading level**: Text written at 6th-8th grade reading level for clarity
- **Color independence**: All information conveyed without relying solely on color

## Completion Flow
- **On Continue**: Smooth transition animation to next screen
- **On Skip**: Gentle acknowledgment toast and transition to dashboard with reminder card
- **Visual Continuity**: Visual elements from onboarding carry through to maintain context

## Final Notes
This welcome screen establishes that the care recipient is a full participant in their care journey, not a passive recipient. The emphasis on voice input acknowledges that typing may be challenging for some users while also symbolically reinforcing that their voice—their preferences and choices—matters in the care relationship. The screen balances warmth with respect, avoiding infantilizing language while providing clear guidance on the purpose and value of the setup process.
