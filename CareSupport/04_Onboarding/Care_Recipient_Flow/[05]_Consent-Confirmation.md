### ✅ [05] Consent Confirmation

**Screen Title**: Consent & Participation

---

### 🎯 Purpose  
To comply with privacy requirements and reinforce the recipient’s choice in participating in shared care.

---

### 🧠 Philosophy  
Nothing about me, without me. Consent must be informed, respected, and revocable.

---

### 📲 UI Components  
- Checkbox:  
  - [x] “I consent to participating in shared care coordination with my caregiver and support team.”
- Link: View full terms and privacy policy

---

### 🔄 Logic  
- Cannot continue unless checkbox is checked  
- Logs timestamp of consent

---

### ⚙️ Technical Notes  
- Backend must log: user ID, timestamp, IP  
- Consent can be revoked later in profile settings
