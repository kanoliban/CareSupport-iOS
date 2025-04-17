### ✅ [02] Privacy Preferences

**Screen Title**: Set Your Privacy Preferences

---

### 🎯 Purpose  
To give the care recipient control over who sees what, when, and how—creating a sense of comfort and trust from the start.

---

### 🧠 Philosophy  
Empowerment is giving people meaningful choices. This screen introduces permission granularity in a clear, non-technical way.

---

### 📲 UI Components  
- Toggle Groups:  
  - Who can see my health updates? [Caregiver / Organizer / Supporter / None]  
  - Who can message me directly? [Caregiver / Organizer / Supporter / None]  
  - Who can change my schedule? [Caregiver only / Caregiver + Organizer / Nobody]

---

### 🔄 Logic  
- All settings are saved in user profile  
- Defaults are private (e.g., "None" or "Caregiver only")  
- Changes can be made later in Settings

---

### ⚙️ Technical Notes  
- Should update user profile record upon “Next”  
- Inform linked caregiver of chosen preferences  
