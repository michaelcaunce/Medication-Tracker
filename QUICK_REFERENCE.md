# Medication Tracker - Quick Reference Guide

## 🔐 Login Credentials

**Admin:** admin@mayfield.test / Password123!  
**Staff:** staff@mayfield.test / Password123!

---

## ⚡ Quick Actions

### Dashboard
- **View Today's Schedule** → Click "Dashboard" in navigation
- **Mark Medication Given** → Click "✓ Mark Done" on schedule item
- **Test Alerts** → Click "⏰ Time Simulator" button
- **Refresh Data** → Click "🔄 Refresh" button

### When Alert Appears
- **✓ Administered** → Medication has been given (logs and dismisses)
- **Snooze 5/10/15m** → Need more time (reschedules alert)
- **Open Details** → View student information (keeps alert active)

### Students
- **Add Student** → Students page → "+ Add Student"
- **Edit Student** → Find student → "Edit" button
- **View Details** → Find student → "View" button
- **Link Medication** → Student detail → "+ Link Medication"
- **Search** → Type in search box at top of list

### Medications
- **Add Medication** → Medications page → "+ Add Medication"
- **Edit Medication** → Find medication → "Edit" button
- **View Details** → Find medication → "View" button
- **Search** → Type in search box at top of list

### Administration Log
- **View Log** → Click "Log" in navigation
- **Search Entries** → Type in search box
- **Filter by Status** → Use status dropdown
- **Export to CSV** → Click "📥 Export CSV"

### Admin (Admin Only)
- **Add Staff** → Admin page → "+ Add Staff"
- **Edit Staff** → Find staff → "Edit" button
- **Deactivate** → Find staff → "Deactivate" button
- **Delete** → Find staff → "Delete" button

---

## 🎨 Colour Codes

- 🔵 **Blue** = Upcoming (not yet due)
- 🟠 **Orange** = Due now (within 15 minutes)
- 🔴 **Red** = Overdue (more than 15 minutes late)
- 🟢 **Green** = Completed (already administered)

---

## ⏰ Alert Status Meanings

- **PENDING** = Scheduled but not yet due
- **DUE** = Currently due (0-15 minutes)
- **OVERDUE** = Past due time (15+ minutes)
- **DONE** = Administered and logged
- **SNOOZED** = Temporarily postponed

---

## 📋 Required Fields

### Adding a Student
- ✅ Name
- ✅ Class
- ✅ Date of Birth
- ⭕ Notes (optional)

### Adding a Medication
- ✅ Name
- ✅ Dosage
- ✅ Repeat Rule (Daily/Weekly/As-needed)
- ✅ Administration Times (e.g., 09:00, 15:00)
- ⭕ Instructions (optional)

### Adding Staff (Admin)
- ✅ Name
- ✅ Email (must be unique)
- ✅ Password (minimum 8 characters)
- ✅ Role (Staff or Admin)

---

## 🔧 Time Simulator

**Purpose:** Test alerts without waiting for real time

**How to Use:**
1. Dashboard → "⏰ Time Simulator"
2. Enter minutes to advance (e.g., 60 = 1 hour forward)
3. Click OK
4. Alerts appear immediately if medications are due

**Example Times to Test:**
- **08:00** → 1 medication
- **08:30** → 1 medication
- **09:00** → 3 medications (multiple alerts)
- **12:00** → 2 medications
- **15:00** → 1 medication
- **18:00** → 1 medication

**Reset:** Enter 0 to return to real time

---

## 🚨 Troubleshooting

### Alert Not Appearing
1. Check you're logged in
2. Verify time is correct
3. Refresh the page
4. Use Time Simulator to test

### Audio Not Playing
1. Check device volume
2. Check browser permissions
3. Click anywhere on page first

### Can't Log In
1. Verify email and password
2. Check account is active
3. Try demo accounts

### Data Not Saving
1. Check localStorage is enabled
2. Not in private/incognito mode
3. Clear browser cache

---

## 📱 Browser Support

✅ Chrome 90+  
✅ Edge 90+  
✅ Firefox 88+  
✅ Safari 14+

---

## 🔒 Security Tips

- Always log out on shared computers
- Don't share your password
- Report suspicious activity
- Keep browser updated

---

## 📞 Need Help?

1. Check USER_GUIDE.md for detailed instructions
2. Check TECHNICAL_DOCUMENTATION.md for technical details
3. Contact your IT administrator (Michael)

---

## 🎯 Daily Workflow

**Morning:**
1. Log in
2. Review Dashboard
3. Check for overdue items

**During Day:**
1. Keep app open in browser tab
2. Respond to alerts promptly
3. Mark medications as administered immediately

**End of Day:**
1. Review Administration Log
2. Verify all medications given
3. Log out

---

## ⌨️ Keyboard Shortcuts

- **Tab** → Navigate between fields
- **Enter** → Submit forms
- **Ctrl+F** / **Cmd+F** → Search in tables

---

**Version:** 1.0 | **Last Updated:** October 2025