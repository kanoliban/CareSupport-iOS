# Screen Spec: Identify Recipient

**Flow Position:** Caregiver Flow – Screen 2  
**Filename:** [02]_Identify-Recipient.md  
**Date:** April 11, 2025  
**Owner:** Product Team

---

## ✅ Purpose
This screen enables the caregiver to identify the care recipient by name. This is essential for linking tasks, medication, and care circle records to the correct individual.

---

## 🧠 User Context
The caregiver has just defined their relationship to the care recipient and is now ready to specify who they are caring for. This is an emotionally significant step—acknowledging the person at the center of their caregiving journey.

---

## 🖥️ UI Elements

- **Input: First Name** *(required)*
- **Input: Last Name** *(optional)*
- **Optional: Upload Profile Photo**
  - Accepts: JPG, PNG, HEIC
  - UI includes photo placeholder with "Add Photo" label
- **Continue Button**
  - Disabled until first name is entered

---

## 🧭 Navigation

- **Back**: Returns to “Define Relationship”
- **Continue**: Advances to “Care Situation”

---

## 🔐 Data Handling

- **Stored Fields**: `careRecipient.firstName`, `careRecipient.lastName`, `careRecipient.photo`
- **Validation**:
  - First name: minimum 1 character, alpha only
  - Last name: optional, alpha only
- **Image Handling**: Compressed to 300x300 for display, saved in encrypted blob storage

---

## ✨ Emotional Microcopy

> “This is the person you're here to support. Let’s begin with their name.”

If a photo is uploaded:
> “Beautiful. A face makes this real.”

---

## 🧪 Acceptance Criteria

- [ ] First name must be entered to enable Continue
- [ ] Image upload accepts standard formats and previews immediately
- [ ] Fields save correctly to `careRecipient` data object
- [ ] Progress saved to session if app is closed

---

## ♿ Accessibility Notes

- All fields accessible by keyboard and screen reader
- Photo button has accessible label “Upload profile photo of care recipient”

---

## 🔄 Future Enhancements

- Face recognition to confirm identity in shared devices (Phase 2)
- Auto-suggestion if contact permissions granted (Phase 2)

