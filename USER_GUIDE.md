# Mayfield Medication Tracker - User Guide

## Table of Contents
1. [Getting Started](#getting-started)
2. [Login](#login)
3. [Dashboard](#dashboard)
4. [Managing Students](#managing-students)
5. [Managing Medications](#managing-medications)
6. [Handling Alerts](#handling-alerts)
7. [Administration Log](#administration-log)
8. [Admin Functions](#admin-functions)
9. [Testing with Time Simulator](#testing-with-time-simulator)
10. [Troubleshooting](#troubleshooting)

---

## Getting Started

### System Requirements
- Modern web browser (Chrome 90+, Edge 90+, Firefox 88+, Safari 14+)
- Internet connection
- Screen resolution: 1366x768 or higher recommended
- Audio enabled for alert notifications

---

## Login

### Demo Accounts

**Admin Account:**
- Email: `admin@mayfield.test`
- Password: `Password123!`

**Staff Account:**
- Email: `staff@mayfield.test`
- Password: `Password123!`

### Logging In

1. Enter your email address
2. Enter your password
3. Click **"Sign In"**
4. You'll be redirected to the Dashboard

### Security
- Your session remains active until you log out
- Always log out when finished, especially on shared computers
- Passwords are securely hashed and stored

---

## Dashboard

The Dashboard is your home screen showing today's medication schedule.

### Dashboard Overview

**Statistics Cards:**
- **Total Scheduled** - All medications scheduled for today with progress bar
- **Completed** - Medications already administered (green)
- **Due Now** - Medications currently due (orange)
- **Overdue** - Medications past their due time (red)

**Today's Schedule:**
- Lists all scheduled medications in chronological order
- Colour-coded by status:
  - Blue border = Upcoming
  - Orange background = Due now
  - Red background = Overdue
  - Green checkmark = Completed

### Quick Actions

- **🔄 Refresh** - Manually refresh the dashboard
- **⏰ Time Simulator** - Test alerts by advancing time (see Testing section)
- **✓ Mark Done** - Quickly mark a medication as administered

---

## Managing Students

### Viewing Students

1. Click **"Students"** in the navigation bar
2. Use the search bar to find specific students
3. Click **"View"** to see student details

### Adding a New Student

1. Go to Students page
2. Click **"+ Add Student"**
3. Fill in the form:
   - **Name** (required)
   - **Class** (required)
   - **Date of Birth** (required)
   - **Notes** (optional - for medical information)
4. Click **"Save"**

### Editing a Student

1. Find the student in the list
2. Click **"Edit"** or view the student and click **"Edit Student"**
3. Update the information
4. Click **"Save"**

### Student Detail Page

The student detail page shows:
- Personal information (name, class, DOB)
- Medical notes
- All linked medications with dosages and times
- Recent administration history

### Linking Medications to Students

1. Go to the student's detail page
2. Click **"+ Link Medication"**
3. Select a medication from the dropdown
4. Click **"Save"**

### Unlinking Medications

1. Go to the student's detail page
2. Find the medication in the list
3. Click **"Unlink"**
4. Confirm the action

---

## Managing Medications

### Viewing Medications

1. Click **"Medications"** in the navigation bar
2. Use the search bar to find specific medications
3. Click **"View"** to see medication details

### Adding a New Medication

1. Go to Medications page
2. Click **"+ Add Medication"**
3. Fill in the form:
   - **Name** (required) - e.g., "Salbutamol Inhaler"
   - **Dosage** (required) - e.g., "2 puffs"
   - **Instructions** (optional) - e.g., "Administer when needed"
   - **Repeat Rule** (required) - Daily, Weekly, or As-needed
   - **Administration Times** (required) - Comma-separated, e.g., "09:00, 15:00"
4. Click **"Save"**

### Editing a Medication

1. Find the medication in the list
2. Click **"Edit"** or view the medication and click **"Edit Medication"**
3. Update the information
4. Click **"Save"**

### Medication Detail Page

The medication detail page shows:
- Medication name and dosage
- Administration times
- Repeat schedule
- Instructions
- All students currently taking this medication

---

## Handling Alerts

### How Alerts Work

The system continuously monitors for due medications. When a medication becomes due:

1. **Full-screen alert appears** - Cannot be missed
2. **Audio tone plays** - Continuous beeping until resolved
3. **Voice announcement** - AI voice reads: "Attention. [Student Name]. [Medication Name] [Dosage]. Please administer now."
4. **Visual cues** - Pulsing animation and high-contrast colours

### Alert Layouts

- **1 alert** = Full screen
- **2 alerts** = Split screen (side-by-side)
- **4+ alerts** = Grid layout

### Alert Actions

Each alert tile has three action options:

#### 1. ✓ Administered (Green Button)
**Use this when you have given the medication to the student.**

What happens:
- Alert immediately disappears
- Entry is logged in Administration Log
- Status changes to DONE
- If no other alerts remain, the overlay closes completely
- Audio and voice stop

#### 2. Snooze (Orange Buttons)
**Use this when you need a few more minutes.**

Options:
- Snooze 5m
- Snooze 10m
- Snooze 15m

What happens:
- Alert disappears temporarily
- Medication is rescheduled for the selected time
- Alert will reappear at the new time

#### 3. Open Details (Gray Button)
**Use this to view more information about the student.**

What happens:
- Navigates to the student's detail page
- Alert remains active in the background
- You can return to see the alert again

### Overdue Medications

If a medication is more than 15 minutes late:
- Status changes to **OVERDUE**
- Alert shows "OVERDUE by X minutes"
- Logged as "late" when administered

---

## Administration Log

The Administration Log is your complete audit trail.

### Viewing the Log

1. Click **"Log"** in the navigation bar
2. View all medication administrations in reverse chronological order

### Log Information

Each entry shows:
- **Date/Time** - When the medication was administered
- **Student** - Who received the medication
- **Medication** - What was given
- **Dosage** - How much was given
- **Staff** - Who administered it
- **Status** - On-time or Late
- **Notes** - Any additional comments

### Filtering the Log

**Search:**
- Type in the search box to filter by student, medication, or staff name

**Status Filter:**
- Select "On Time" or "Late" from the dropdown
- Select "All" to see everything

### Exporting to CSV

1. Click **"📥 Export CSV"**
2. File downloads automatically
3. Open in Excel or Google Sheets
4. Filename format: `medication-log-DD-MM-YYYY.csv`

---

## Admin Functions

**Note:** These features are only available to users with the Admin role.

### Accessing Admin Area

1. Log in with an admin account
2. Click **"Admin"** in the navigation bar

### Managing Staff Accounts

#### Adding New Staff

1. Click **"+ Add Staff"**
2. Fill in the form:
   - **Name** (required)
   - **Email** (required) - Must be unique
   - **Password** (required) - Minimum 8 characters
   - **Role** (required) - Staff or Admin
3. Click **"Save"**

#### Editing Staff

1. Find the staff member in the list
2. Click **"Edit"**
3. Update information
4. To change password, enter a new one (leave blank to keep current)
5. Click **"Save"**

#### Deactivating Staff

1. Find the staff member
2. Click **"Deactivate"**
3. Confirm the action
4. Staff member can no longer log in
5. Can be reactivated later

#### Activating Staff

1. Find the deactivated staff member
2. Click **"Activate"**
3. Staff member can log in again

#### Deleting Staff

1. Find the staff member
2. Click **"Delete"**
3. Confirm the action
4. **Warning:** This is permanent and cannot be undone

---

## Testing with Time Simulator

The Time Simulator allows you to test the alert system without waiting for real time to pass.

### How to Use

1. Go to the Dashboard
2. Click **"⏰ Time Simulator"**
3. Enter the number of minutes to advance:
   - Positive number = Jump forward (e.g., 60 = 1 hour ahead)
   - Negative number = Go back (e.g., -30 = 30 minutes back)
4. Click **"OK"**
5. The system immediately checks for alerts based on the new simulated time

### Example Test Scenarios

**Test a single alert:**
```
Current time: 08:00
Enter: 30
Result: Time becomes 08:30 - Oliver Davies' medication alert appears
```

**Test multiple alerts:**
```
Current time: 08:00
Enter: 60
Result: Time becomes 09:00 - Three alerts appear (Emma, Sophie, Lily)
```

**Test overdue alert:**
```
Current time: 09:00
Enter: 20
Result: Time becomes 09:20 - Alerts show "OVERDUE by 20 minutes"
```

**Reset time:**
```
Enter: 0
Result: Returns to real current time
```

### Pre-configured Test Times

The seed data includes medications at these times:
- **08:00** - 1 medication
- **08:30** - 1 medication
- **09:00** - 3 medications (great for testing multiple alerts)
- **12:00** - 2 medications
- **15:00** - 1 medication
- **18:00** - 1 medication

---

## Troubleshooting

### Alerts Not Appearing

**Check:**
1. Are you logged in?
2. Is the time correct? (Use Time Simulator to test)
3. Are there medications scheduled for the current time?
4. Refresh the page and try again

### Audio Not Playing

**Check:**
1. Is your device volume turned up?
2. Does your browser allow audio? (Check browser permissions)
3. Try clicking anywhere on the page first (browsers require user interaction)

### Voice Not Speaking

**Check:**
1. Does your browser support Web Speech API? (Chrome and Edge work best)
2. Is text-to-speech enabled in your system settings?
3. Try refreshing the page

### Data Not Saving

**Check:**
1. Is localStorage enabled in your browser?
2. Do you have enough storage space?
3. Are you in private/incognito mode? (Data won't persist)

### Can't Log In

**Check:**
1. Are you using the correct email and password?
2. Is the account active? (Admin can check)
3. Try the demo accounts to verify the system works

### Page Not Loading

**Check:**
1. Is your internet connection working?
2. Try refreshing the page (Ctrl+F5 or Cmd+Shift+R on Apple devices)
3. Clear your browser cache
4. Try a different browser

### Alert Won't Dismiss

**Check:**
1. Did you click the "✓ Administered" button?
2. Wait a moment - the system may be processing
3. Refresh the page if stuck

---

## Best Practices

### Daily Workflow

1. **Morning:**
   - Log in at the start of the day
   - Review today's schedule on the Dashboard
   - Check for any overdue items from previous day

2. **During the Day:**
   - Keep the application open in a browser tab
   - Respond to alerts promptly
   - Mark medications as administered immediately after giving them

3. **End of Day:**
   - Review the Administration Log
   - Check that all scheduled medications were given
   - Log out when finished

### Alert Management

- **Respond quickly** - Alerts are time-sensitive
- **Use Administered** - As soon as medication is given
- **Use Snooze** - Only if you need a few minutes
- **Don't ignore** - Overdue medications need attention

### Data Entry

- **Be accurate** - Double-check student and medication details
- **Be complete** - Fill in all required fields
- **Be consistent** - Use standard formats for times (HH:MM)
- **Add notes** - Include important medical information

### Security

- **Log out** - Always log out on shared computers
- **Protect passwords** - Don't share your login credentials
- **Report issues** - Tell an admin if you see any problems

---

## Keyboard Shortcuts

- **Tab** - Navigate between fields and buttons
- **Enter** - Submit forms or activate buttons
- **Escape** - Close modals (when implemented)
- **Ctrl+F** or **Cmd+F** - Use browser search on tables

---

## Support

If you encounter any issues not covered in this guide:

1. Try refreshing the page
2. Check the Troubleshooting section
3. Contact your IT administrator (Michael)

---

**Last Updated:** October 2025  
**Version:** 1.0  
**Application:** Medication Tracker