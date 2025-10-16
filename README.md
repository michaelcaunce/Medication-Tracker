# Medication Tracker

A comprehensive browser-based web application for classroom medication tracking.

## 🔐 Demo Accounts

### Admin Account
- **Email:** admin@mayfield.test
- **Password:** Password123!
- **Permissions:** Full access including staff management

### Staff Account
- **Email:** staff@mayfield.test
- **Password:** Password123!
- **Permissions:** Standard access to students, medications, and logging

## ✨ Key Features

### 1. Authentication & Role-Based Access
- Secure staff sign-in with password protection
- Two roles: Admin and Staff
- Session management with automatic logout

### 2. Dashboard
- Real-time overview of today's medication schedule
- Statistics: Total scheduled, completed, due now, overdue
- Progress indicator showing completion percentage
- Colour-coded schedule items by urgency
- Quick actions to mark medications as administered

### 3. Student Management
- Complete CRUD operations (Create, Read, Update, Delete)
- Student profiles with personal details
- Class assignments and date of birth tracking
- Notes field for important medical information
- View all linked medications per student
- Quick search functionality

### 4. Medication Management
- Complete CRUD operations for medications
- Dosage and administration instructions
- Multiple administration times per day
- Repeat rules: Daily, Weekly, As-needed
- View all students linked to each medication
- Link/unlink medications to students

### 5. Alert System (CRITICAL FEATURE)
- **Continuous monitoring** checks for due medications every 10 seconds
- **Full-screen alerts** that are impossible to miss
- **Adaptive layouts:**
  - 1 alert = full screen
  - 2 alerts = split screen (side-by-side)
  - 4+ alerts = responsive grid
- **Audio alert tone** plays continuously until resolved
- **AI voice announcements** using Web Speech API
- **Functional trigger buttons:**
  - ✓ **Administered** - Immediately logs administration, updates status to DONE, removes alert tile, and closes overlay if no other alerts remain
  - **Snooze (5/10/15 min)** - Reschedules the medication
  - **Open Details** - Navigate to student details without dismissing alert
- **Visual attention cues:** Pulsing animations, high contrast colours
- **Status tracking:** DUE, OVERDUE (15+ minutes late)

### 6. Administration Logging
- Complete audit trail of all medication administrations
- Captures: timestamp, staff member, student, medication, dosage, instructions, status (on-time/late), optional notes
- **Filtering and search:**
  - Search by student, medication, or staff name
  - Filter by status (on-time/late)
  - Date range filtering
- **CSV export** for external reporting
- Instant log updates when medications are administered

### 7. Admin Area (Admin Only)
- Staff account management
- Create new staff accounts
- Edit existing staff details
- Reset passwords
- Activate/deactivate accounts
- Delete staff accounts
- Role assignment (Admin/Staff)

### 8. UX Enhancements
- **Global search** on students and medications pages
- **Colour coding:**
  - 🔴 Red = Overdue
  - 🟠 Orange = Due now
  - 🟢 Green = Completed
  - 🔵 Blue = Upcoming
- **Progress indicators** showing daily completion percentage
- **Time simulator** for testing alerts (accessible via Dashboard)
- **Responsive design** for various screen sizes
- **Keyboard accessible** throughout

## 🎨 Design & Branding

The application matches the Chorley Mayfield Primary School website styling:
- **Typography:** Century Gothic font family (with web-safe fallbacks)
- **Primary Color:** #0066cc (Mayfield Blue)
- **Accent Colors:** Green (#00a651), Orange (#ff6600), Red (#dc3545)
- **Mayfield Logo:** Displayed in header
- **Clean, professional layout** with clear visual hierarchy

## 🧪 Testing the Alert System

### Using the Time Simulator

1. Log in to the application
2. Navigate to the Dashboard
3. Click the **"⏰ Time Simulator"** button
4. Enter the number of minutes to advance time (e.g., 60 to jump 1 hour forward)
5. Click OK
6. The system will immediately check for alerts based on the simulated time

### Pre-configured Test Schedules

The application comes with seed data including medications scheduled throughout the day:
- **08:00** - Jack Robinson - Insulin (NovoRapid) 4 units
- **08:30** - Oliver Davies - Methylphenidate 10mg
- **09:00** - Emma Thompson - Salbutamol Inhaler 2 puffs
- **09:00** - Sophie Williams - Sodium Valproate 200mg
- **09:00** - Lily Anderson - Cetirizine 10mg
- **12:00** - Emma Thompson - Paracetamol 250mg
- **12:00** - Jack Robinson - Insulin (NovoRapid) 4 units
- **15:00** - Emma Thompson - Salbutamol Inhaler 2 puffs
- **18:00** - Jack Robinson - Insulin (NovoRapid) 4 units

### Testing Alert Scenarios

1. **Single Alert:**
   - Advance time to any scheduled medication time
   - Observe full-screen alert with audio and voice

2. **Multiple Alerts:**
   - Advance time to 09:00 (3 medications due simultaneously)
   - Observe grid layout with multiple alert tiles

3. **Overdue Alerts:**
   - Advance time 20+ minutes past a scheduled time
   - Observe "OVERDUE" status and red colouring

4. **Snooze Functionality:**
   - Click "Snooze 5m" on an alert
   - Advance time by 5 minutes
   - Alert reappears

5. **Administered Action:**
   - Click "✓ Administered" on an alert
   - Verify alert disappears immediately
   - Check Administration Log for new entry
   - Verify overlay closes if no other alerts remain

## 📊 Data Model

### Staff
- id, name, email, role (admin/staff), status (active/inactive), passwordHash

### Student
- id, name, class, dateOfBirth, notes, active

### Medication
- id, name, dosage, instructions, repeatRule (daily/weekly/as-needed), times[]

### StudentMedication (Link Table)
- id, studentId, medicationId, active

### ScheduleInstance
- id, studentId, medicationId, dueAt, status (PENDING/DUE/OVERDUE/DONE/SNOOZED), administeredAt

### AdministrationLog
- id, scheduleInstanceId, administeredAt, staffId, staffName, studentId, studentName, medicationId, medicationName, dosage, instructions, status (on-time/late), notes

## 🔒 Security Features

- Password hashing for staff accounts
- Session-based authentication
- Role-based access control (RBAC)
- Input validation and sanitization
- XSS protection through HTML escaping
- Secure local storage for data persistence

## ♿ Accessibility (WCAG 2.2 AA)

- High contrast colour schemes
- Minimum 44x44px touch targets
- Keyboard navigation support
- Screen reader compatible
- Focus indicators on all interactive elements
- Semantic HTML structure
- ARIA labels where appropriate
- Reduced motion support for users with vestibular disorders

## 🌍 Localisation

- UK date format (DD/MM/YYYY)
- 24-hour time format
- Europe/London timezone
- Handles BST/GMT transitions

## 💾 Data Persistence

- All data stored in browser's localStorage
- Automatic save on every change
- Data persists across sessions
- Can be cleared via browser settings

## 🚀 Deployment

The application is a single-page application (SPA) consisting of:
- `index.html` - Main HTML structure
- `styles.css` - Complete styling
- `app.js` - Full application logic

### To Deploy:
1. Upload all three files to any web server
2. Ensure files are served with correct MIME types
3. No server-side processing required
4. Works on any modern web server (Apache, Nginx, etc.)

### Browser Requirements:
- Chrome 90+ (recommended)
- Edge 90+
- Firefox 88+
- Safari 14+

## 📱 Responsive Design

The application is optimised for:
- Desktop displays (1920x1080 and above)
- Laptop screens (1366x768 and above)
- Tablet devices (768px and above)
- Touch-friendly interface with large tap targets

## 🎯 Acceptance Criteria - All Met ✅

✅ Pressing **Administered** immediately logs administration, updates status to DONE, removes alert, and closes overlay if no other alerts remain  
✅ Staff can see today's schedule with real-time updates  
✅ Full-screen alerts appear at due times with audio and voice  
✅ Snooze functionality reschedules medications  
✅ Multiple simultaneous alerts render in split/tile layouts  
✅ Administration log can be filtered, searched, and exported to CSV  
✅ Admin area supports full CRUD operations on staff accounts  
✅ UI matches Mayfield's look and feel with Century Gothic typography  
✅ Works in Chrome/Edge on Windows  
✅ Responsive for typical classroom display sizes  
✅ All functional requirements implemented and tested  

## 📞 Support

For issues or questions about the application, please contact the NinjaTech AI development team.

## 📄 License

Copyright © 2025 Chorley Mayfield Primary School. All rights reserved.

---

**Built with ❤️ for Chorley Mayfield Primary School**