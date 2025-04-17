# Universal Screens Specification
CareSupport B2C - Reusable Onboarding UI Patterns
Version: 1.0

---

This document outlines screens and interaction components that are shared across all onboarding flows, regardless of user role (Caregiver, Care Recipient, Organizer, Supporter). These components ensure UX consistency, accessibility compliance, and logical continuity throughout the onboarding experience.

---

## 1. Consent + Terms Screen

### Title:
"Before we begin, let's set the ground rules."

### Content:
- Bullet-point summary of user rights and responsibilities
- Link to full Terms of Service and Privacy Policy

### UI Elements:
- Checkbox: "I agree to the terms and privacy policy"
- Primary CTA: "Continue"
- Secondary CTA: "Read full terms"

### Logic:
- Checkbox must be selected to proceed
- Decision saved to user profile
- Confirmation timestamp stored with version of terms

---

## 2. Error Handling

### Universal Behavior:
- All screens gracefully handle form errors, API failures, and invalid inputs.

### Visual Design:
- Red text with icon for error indicators
- Toast message for network failures

### Common Patterns:
- "Required field" warning
- "Could not connect" message with retry option
- Reset form or restore previous inputs on reload

---

## 3. Accessibility Notices

### Core Standards:
- WCAG 2.1 AA compliance
- Screen reader support
- Descriptive alt text for images and icons
- Font size min: 16pt
- Color contrast: minimum 4.5:1

### Accessibility Features:
- Focus state indicators
- Keyboard navigability for all actions
- Clear, consistent language

---

## 4. Timeout & Session Logic

### User Dropoff Logic:
- Incomplete onboarding sessions are auto-saved
- Reopening the app resumes where user left off
- If inactive > 24 hours, user sees “Pick up where you left off?”

### Edge Cases:
- If user deleted or uninvited during onboarding: display soft block

---

## 5. App Restart Behaviors

- Session state preserved on quit
- Cached inputs restored
- Error recovery prompts shown if crash detected

---

## 6. Skipped Task Reminders

### Triggers:
- User skips "Invite" or "Add CareCircle Member" screen

### Reminders:
- Displayed on first dashboard login: “You skipped a step—want to finish setting up?”
- Persistent notification in user task list

---

## 7. Navigation Behavior

### Back Button:
- All screens allow back navigation unless confirmation was final
- Back flow never erases data unless confirmed by user

### Home Behavior:
- Home redirects to Role Selection if onboarding is not complete

---

## 8. Language Toggle

### Availability:
- Displayed on all onboarding screens
- Supported in: English (v1), Spanish (v1.1)

---

## 9. Offline States

### Offline Behavior:
- User is notified of no internet
- UI remains usable for forms with autosave
- On reconnect: sync and confirmation message

---

## Acceptance Criteria

- All universal patterns tested across roles
- Fallback flows verified offline and online
- Accessible by default (not added later)
