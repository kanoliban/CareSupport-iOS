# [08] Privacy and Permissions Overview

**Screen Type:** Required Onboarding Step  
**Audience:** Caregiver  
**Location:** `/03_Screen_by_Screen_Specs/Caregiver-Flow/`  
**Date Generated:** April 12, 2025

---

## 🧠 Purpose

This screen introduces caregivers to CareSupport’s privacy model. It ensures they understand:
- What information is shared
- Who can access specific types of data
- How the care recipient’s preferences impact visibility
- The importance of transparency in care coordination

---

## 🔐 Core Message

> “Transparency builds trust. At CareSupport, we help you coordinate care without compromising privacy.”

---

## 📊 Content Breakdown

| Section Title        | Element Type | Details |
|----------------------|--------------|---------|
| **Role Access Summary** | Visual chart | Color-coded cards showing: Caregiver, Organizer, Supporter |
| **Editable vs. Viewable** | Text blocks | Explanation of what each role can **see**, **edit**, or **comment on** |
| **Care Recipient Control** | Emphasis line | "Care recipients can limit access to sensitive info at any time." |
| **Required Acknowledgment** | Checkbox | “I understand how sharing works on CareSupport” (must be checked to continue) |
| **Progression** | CTA Button | “Continue to Dashboard” |

---

## ⚙️ Behavior

- This screen **must** be completed before accessing the app dashboard.
- Consent acknowledgment is logged as `privacy_acknowledged: true`.
- Data is stored to track onboarding comprehension for future analytics.

---

## 💬 Emotional Framing

Tone: **Respectful, clear, empowering**  
Visuals: Soft iconography with neutral background tones  
Copy principles:
- Avoid fear-based language
- Use “with you” language (e.g. “Here’s how we’ll keep you in control”)

---

## ✅ Acceptance Criteria

- Visual access chart clearly distinguishes roles
- Checkbox is functional and prevents progression until selected
- CTA moves user to final onboarding screen
- Fully accessible via screen reader
- Tested with at least 5 users before release

