# Define Relationship Screen Spec

## Screen ID
[02]_Caregiver-Flow/[01]_Define-Relationship.md

## Purpose
Allow caregivers to declare their relationship to the care recipient, establishing emotional context and setting the stage for personalized interactions.

## Role Visibility
- ✅ Caregiver
- ❌ Care Recipient
- ❌ Organizer
- ❌ Supporter

## UX Flow
- Follows "Role Declaration" screen
- Must be completed before progressing

## UI Elements
- **Prompt (Header):** "I am caring for my..."
- **Dropdown Field:** 
  - Options: Mother, Father, Spouse, Sibling, Friend, Neighbor, Grandparent, Child, Other
- **Helper Text (below dropdown):** "This helps us understand the dynamic of your care journey."

## Functional Requirements
- Selection required to proceed
- "Other" opens text field for manual input
- Selection stored for personalization logic downstream

## Emotional Intelligence Design
- Language is relational, not clinical
- Dropdown options are person-first (e.g., "Mother" not "Parent A")
- Optional: Microcopy under header — "Every relationship is different. Start by telling us who you’re supporting."

## Accessibility
- Keyboard navigable dropdown
- Screen reader labels include emotional context
- High-contrast focus state on dropdown

## Data Capture
- Field: `relationship_to_recipient`
- Type: string
- Required: yes

## Error Handling
- If left blank: prompt appears “Let us know who you’re caring for to continue.”

## Success Criteria
- User selects or types a relationship
- Data is stored in session
- CTA button “Continue” becomes active upon input