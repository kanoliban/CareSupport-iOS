### ✅ [01] Confirm Invite

**Screen Title**: Confirm Invite

---

### 🎯 Purpose  
This screen enables the care recipient to confirm a connection request from the caregiver, or to initiate the connection manually.

---

### 🧠 Philosophy  
Acknowledging that the care recipient’s participation is voluntary sets the tone for respect and agency from the very beginning. This is not just a registration step—it’s a choice.

---

### 📲 UI Components  
- If arriving via invite link:  
  - Text: “You’ve been invited by [Caregiver Name] to join their care circle.”  
  - Confirm Button: “Yes, I accept”
- If not via invite:  
  - Input field: “Enter the phone number or email of your caregiver”
  - Button: “Send Request to Connect”

---

### 🔄 Logic  
- If invite link: caregiver ID is pre-filled  
- If manual input: backend sends a connection request to caregiver  
- Once confirmed, user proceeds to privacy screen

---

### ⚙️ Technical Notes  
- Backend validates invite token or initiates connection request  
- Timeout on invite tokens after 7 days  
