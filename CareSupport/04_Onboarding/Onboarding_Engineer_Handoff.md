
# 🛠️ CareSupport Onboarding — Engineer Handoff README

## Overview
This README serves as the central orientation document for engineering implementation of the CareSupport iOS onboarding experience. It includes structure, order, special considerations, and role-dependent behavior. It is specifically written for the lead iOS developer building the onboarding flows.

---

## 📁 Folder Path
This file lives in:  
`/09_ReadMe_and_Handoff/README_Onboarding_Engineer_Handoff.md`

---

## 🎯 Goal of Onboarding
- Establish care roles and context
- Form core user relationships (caregiver <-> recipient)
- Capture key info to personalize dashboard
- Prime users emotionally and reduce friction
- Minimize cognitive load with step-by-step flows
- Set up user session and data objects

---

## 🧭 Where to Start
1. Begin with `/03_Screen_by_Screen_Specs/01_Role-Declaration.md` — this initiates all branching logic.
2. Proceed through the **Caregiver Flow**, then **Care Recipient**, then **Universal Screens**.
3. Use `/02_Specs/onboarding-spec.md` for full logic reference and pathing.
4. Reference `/01_Flowcharts/caresupport-onboarding-flow.svg` for top-level diagram.

---

## 🧩 Component Order (Build Priority)

### 1. Shared Setup
- Role Declaration
- Data Objects from `/05_Tech_Notes/data-fields.json`

### 2. Caregiver Flow (Primary)
- Define Relationship
- Identify Care Recipient
- Care Situation
- Primary Challenge
- Quick Win Action
- Invite Care Recipient
- Add to Care Circle
- Permissions Overview
- Dashboard Intro

### 3. Care Recipient Flow
- Confirm Invite
- Privacy Preferences
- Express Needs
- Today's Plan Preview
- Consent Confirmation
- Emotional Message

### 4. Universal Screens
- Consent + Terms
- Error States
- Accessibility
- Session Resume & Skipped Actions
- Language Toggle

---

## ❤️ Emotional Cues (from `/02_Specs/emotional-logic.md`)
Ensure these appear at:
- End of Caregiver Flow
- End of Recipient Flow
- First Invite Screen
- After first Quick Win

Use default messaging if not customized.

---

## ⚙️ Required Behaviors
- Stateful session logic
- Resume onboarding at correct step
- Skipped steps saved and flagged
- Pre-fill from invite links
- Analytics event on each completed screen

---

## 📦 Assets
- Flowchart (`/01_Flowcharts/caresupport-onboarding-flow.svg`)
- UI samples (`/04_Component_Assets/sample-ui-wires.png`)
- Voice UI notes (`/06_Voice_UI_Notes`)
- Accessibility rules (`/08_Accessibility_Standards`)

---

## ✅ MVP Acceptance Criteria
- All "Must Have" onboarding specs built and tested
- Successful invite and account creation across roles
- WCAG 2.1 AA accessibility met
- Onboarding takes < 3 minutes per user
- Local testing on iOS 14+

---

## Final Notes
This onboarding flow is emotionally aware, role-adaptive, and essential to CareSupport’s brand and functionality. If any ambiguity arises, defer to `/02_Specs/onboarding-spec.md` and consult product lead.

We’re building something that truly supports those who support others.

