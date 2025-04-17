
# CareSupport B2C — Master Onboarding Product Requirements Document (v1.0)

## 🔍 Overview
This PRD outlines the full onboarding experience for the CareSupport B2C iOS application. It serves as a comprehensive reference for product, engineering, and design. Our guiding principle is clear:

> **Care begins with Support.**

## 🎯 Objective
To build and release a production-ready onboarding experience for iOS that:
- Empowers caregivers and care recipients to connect and coordinate
- Demonstrates immediate value
- Meets accessibility and privacy standards
- Converts free users into paying family accounts

## 👥 User Roles

| Role           | Description                                                                 |
|----------------|-----------------------------------------------------------------------------|
| Caregiver      | Primary coordinator of care; initiates account, adds members, leads flow   |
| Care Recipient | Receives care; sets privacy and participation preferences                  |
| Organizer      | Logistics and admin support; helps with scheduling and planning            |
| Supporter      | Lends help occasionally: errands, meals, rides, etc.                       |

---

## 🚀 Onboarding Entry Point

### Role Declaration (Universal Screen 1)
- Title: "Let’s begin. Who are you?"
- Options: Caregiver, Care Recipient, Organizer, Supporter
- Required: Yes
- Behavior: Branches into dedicated role-specific onboarding flow

---

## 🛠️ Caregiver Flow (Primary Path)

### Screen 2: Define Relationship
- Dropdown: “I’m caring for my…” (Mother, Father, Spouse, etc.)

### Screen 3: Identify Recipient
- Input: Name, Optional Photo

### Screen 4: Describe Situation
- Free text with autosuggest (stroke, dementia, surgery recovery, etc.)

### Screen 5: Primary Challenge
- Options:
  - Meds
  - Family Coordination
  - Appointments
  - Communication
  - Open Text

### Screen 6: Quick Win Action
- Tailored task based on selected challenge
- Examples:
  - Meds → Add a medication
  - Coordination → Invite first family member

### Screen 7: Invite Recipient
- Input: Email / Phone
- Option: “Skip for now”

### Screen 8: Add Care Circle Members
- Assign roles: Organizer, Supporter

### Screen 9: Privacy & Permissions Overview
- Summary of what members can see/do

### Screen 10: Emotional Framing
- Message: “You’ve set your care circle in motion.”

---

## 🤝 Care Recipient Flow

### Screen 2: Confirm Invite or Connect Manually
### Screen 3: Set Privacy Preferences
- Control over visibility: meds, schedule, contacts

### Screen 4: Express Needs
- Companionship, transportation, reminders, other

### Screen 5: Visual Tour
- Show “Today’s Plan” (summary screen of schedule/tasks)

### Screen 6: Consent
### Screen 7: Emotional Message
- “You’re not just receiving care—you’re shaping it.”

---

## 🧾 Organizer Flow

### Screen 2: Who Are You Supporting?
### Screen 3: View Responsibilities
- Scheduling, coordination, meal delivery, etc.

### Screen 4: Confirm or Request Connection
### Screen 5: Setup Complete

---

## 🙌 Supporter Flow

### Screen 2: How Can You Help?
- Pick tasks (rides, meals, errands)

### Screen 3: Frequency
- Weekly, Monthly, As-Needed

### Screen 4: Notification Preferences
- App, Text, Email

### Screen 5: Setup Complete

---

## 📋 Universal Screens

### Consent & Terms
- Must be checked before entering app

### Error Recovery
- Friendly, non-technical error screens with retry or skip

### Accessibility
- WCAG 2.1 AA
- Keyboard nav, color contrast, screen reader support

---

## 🧠 Emotional Intelligence Triggers

- Screen 6 (Caregiver): “You’ve done something powerful.”
- Screen 7 (Care Recipient): “You’re not just a patient. You’re part of this circle.”

---

## 🧩 Technical Considerations

- Branching logic handled via stateful sessions
- Resume flow if user drops off mid-onboarding
- Invite pre-fills recipient info
- Skipped actions = flagged in dashboard

---

## ✅ Acceptance Criteria

- Each role has 5+ unique screens
- All screens mobile-optimized
- Onboarding time < 3 min
- Invite mechanism tested
- Emotional triggers embedded
- WCAG 2.1 AA compliant
- No dead-ends; all flows resume
- 5 users per role tested pre-launch

---

## 📍Next Step:
Designers move into screen-by-screen UI specs. Engineers begin defining API endpoints and state models for onboarding.

