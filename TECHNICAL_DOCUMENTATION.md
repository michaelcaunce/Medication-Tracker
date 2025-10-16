# Medication Tracker - Technical Documentation

## Architecture Overview

### Technology Stack
- **Frontend:** Vanilla JavaScript (ES6+)
- **Styling:** CSS3 with CSS Custom Properties
- **Data Storage:** Browser localStorage
- **Audio:** HTML5 Audio API
- **Speech:** Web Speech API (SpeechSynthesis)
- **Architecture Pattern:** Single Page Application (SPA)

### File Structure
```
/
├── index.html          # Main HTML structure
├── styles.css          # Complete styling and theming
├── app.js             # Full application logic
├── README.md          # Project overview and setup
├── USER_GUIDE.md      # End-user documentation
└── TECHNICAL_DOCUMENTATION.md  # This file
```

---

## Data Models

### Staff
```javascript
{
  id: string,              // Unique identifier
  name: string,            // Full name
  email: string,           // Login email (unique)
  role: 'admin' | 'staff', // Access level
  status: 'active' | 'inactive', // Account status
  passwordHash: string     // Hashed password
}
```

### Student
```javascript
{
  id: string,        // Unique identifier
  name: string,      // Full name
  class: string,     // Class/year group
  dob: string,       // Date of birth (YYYY-MM-DD)
  notes: string,     // Medical notes
  active: boolean    // Soft delete flag
}
```

### Medication
```javascript
{
  id: string,                           // Unique identifier
  name: string,                         // Medication name
  dosage: string,                       // Dosage amount
  instructions: string,                 // Administration instructions
  repeatRule: 'daily' | 'weekly' | 'as-needed', // Schedule type
  times: string[]                       // Array of times (HH:MM format)
}
```

### StudentMedication (Link Table)
```javascript
{
  id: string,          // Unique identifier
  studentId: string,   // Foreign key to Student
  medicationId: string, // Foreign key to Medication
  active: boolean      // Link status
}
```

### ScheduleInstance
```javascript
{
  id: string,                    // Unique identifier
  studentId: string,             // Foreign key to Student
  medicationId: string,          // Foreign key to Medication
  dueAt: string,                 // ISO 8601 datetime
  status: 'PENDING' | 'DUE' | 'OVERDUE' | 'DONE' | 'SNOOZED',
  administeredAt?: string,       // ISO 8601 datetime (when completed)
  createdAt: string             // ISO 8601 datetime
}
```

### AdministrationLog
```javascript
{
  id: string,                    // Unique identifier
  scheduleInstanceId: string,    // Foreign key to ScheduleInstance
  administeredAt: string,        // ISO 8601 datetime
  staffId: string,               // Foreign key to Staff
  staffName: string,             // Denormalised for reporting
  studentId: string,             // Foreign key to Student
  studentName: string,           // Denormalised for reporting
  medicationId: string,          // Foreign key to Medication
  medicationName: string,        // Denormalised for reporting
  dosage: string,                // Snapshot at time of administration
  instructions: string,          // Snapshot at time of administration
  status: 'on-time' | 'late',   // Timeliness indicator
  notes: string                  // Optional staff notes
}
```

---

## State Management

### Global State Object
```javascript
const state = {
  currentUser: {
    id: string,
    name: string,
    email: string,
    role: string
  } | null,
  currentPage: string,
  data: {
    staff: Staff[],
    students: Student[],
    medications: Medication[],
    studentMedications: StudentMedication[],
    scheduleInstances: ScheduleInstance[],
    administrationLog: AdministrationLog[]
  },
  activeAlerts: ScheduleInstance[],
  alertInterval: number | null,
  audioContext: AudioContext | null,
  speechSynthesis: SpeechSynthesis
};
```

### State Persistence
- All data stored in `localStorage` under key: `mayfield_med_tracker`
- Session data stored in `sessionStorage` under key: `currentUser`
- Time simulation offset stored in `localStorage` under key: `time_simulation_offset`

---

## Core Systems

### 1. Authentication System

#### Password Hashing
```javascript
function hashPassword(password) {
  // Simple hash for demo - production should use bcrypt
  let hash = 0;
  for (let i = 0; i < password.length; i++) {
    const char = password.charCodeAt(i);
    hash = ((hash << 5) - hash) + char;
    hash = hash & hash;
  }
  return hash.toString(36);
}
```

#### Login Flow
1. User submits email and password
2. Password is hashed
3. Hash compared with stored hash
4. If match, user object stored in sessionStorage
5. Redirect to dashboard

#### Session Management
- Session persists until logout or browser close
- `checkAuth()` verifies session on page load
- `requireAuth()` guards protected routes
- `requireAdmin()` guards admin-only routes

### 2. Alert Monitoring System

#### Continuous Monitoring
```javascript
// Checks every 10 seconds
const ALERT_CHECK_INTERVAL = 10000;

function startAlertMonitoring() {
  checkForAlerts(); // Immediate check
  state.alertInterval = setInterval(checkForAlerts, ALERT_CHECK_INTERVAL);
}
```

#### Alert Detection Logic
```javascript
function checkForAlerts() {
  const now = getCurrentTime();
  const dueInstances = [];

  state.data.scheduleInstances.forEach(instance => {
    if (instance.status !== 'PENDING' && 
        instance.status !== 'DUE' && 
        instance.status !== 'OVERDUE') {
      return;
    }

    const dueTime = new Date(instance.dueAt);
    const diffMinutes = (now - dueTime) / 60000;

    if (diffMinutes >= 0) {
      // Update status
      if (diffMinutes > 15) {
        instance.status = 'OVERDUE';
      } else {
        instance.status = 'DUE';
      }
      dueInstances.push(instance);
    }
  });

  if (dueInstances.length > 0) {
    showAlerts(dueInstances);
  } else {
    hideAlerts();
  }
}
```

#### Alert Layout System
```javascript
// Dynamic layout based on alert count
if (instances.length === 1) {
  container.className += ' layout-single';  // Full screen
} else if (instances.length === 2) {
  container.className += ' layout-double';  // Split screen
} else {
  container.className += ' layout-grid';    // Grid layout
}
```

#### Audio System
```javascript
// Continuous audio loop
<audio id="alert-tone" loop>
  <source src="data:audio/wav;base64,..." type="audio/wav">
</audio>

function playAlertSound() {
  const audio = document.getElementById('alert-tone');
  if (audio && audio.paused) {
    audio.play().catch(e => console.log('Audio play failed:', e));
  }
}

function stopAlertSound() {
  const audio = document.getElementById('alert-tone');
  if (audio && !audio.paused) {
    audio.pause();
    audio.currentTime = 0;
  }
}
```

#### Voice Announcements
```javascript
function speakAlert(instance) {
  const student = state.data.students.find(s => s.id === instance.studentId);
  const medication = state.data.medications.find(m => m.id === instance.medicationId);
  
  const text = `Attention. ${student.name}. ${medication.name} ${medication.dosage}. Please administer now.`;
  
  const utterance = new SpeechSynthesisUtterance(text);
  utterance.rate = 0.9;
  utterance.pitch = 1.0;
  utterance.volume = 1.0;
  
  state.speechSynthesis.speak(utterance);
}
```

### 3. Alert Action Handlers

#### Administered Action
```javascript
function markAdministered(instanceId) {
  const instance = state.data.scheduleInstances.find(i => i.id === instanceId);
  const now = getCurrentTime();
  const dueTime = new Date(instance.dueAt);
  const isLate = now > new Date(dueTime.getTime() + 5 * 60000);

  // Update instance
  instance.status = 'DONE';
  instance.administeredAt = now.toISOString();

  // Create log entry
  state.data.administrationLog.push({
    id: generateId(),
    scheduleInstanceId: instanceId,
    administeredAt: now.toISOString(),
    staffId: state.currentUser.id,
    staffName: state.currentUser.name,
    studentId: instance.studentId,
    studentName: student.name,
    medicationId: instance.medicationId,
    medicationName: medication.name,
    dosage: medication.dosage,
    instructions: medication.instructions,
    status: isLate ? 'late' : 'on-time',
    notes: ''
  });

  saveData();

  // Remove from active alerts
  state.activeAlerts = state.activeAlerts.filter(a => a.id !== instanceId);

  // Remove tile from DOM
  const tile = document.querySelector(`[data-instance-id="${instanceId}"]`);
  if (tile) tile.remove();

  // Close overlay if no alerts remain
  if (state.activeAlerts.length === 0) {
    hideAlerts();
  }
}
```

#### Snooze Action
```javascript
function snoozeAlert(instanceId, minutes) {
  const instance = state.data.scheduleInstances.find(i => i.id === instanceId);
  
  // Reschedule
  const newDueTime = new Date(getCurrentTime().getTime() + minutes * 60000);
  instance.dueAt = newDueTime.toISOString();
  instance.status = 'SNOOZED';

  saveData();

  // Remove from active alerts
  state.activeAlerts = state.activeAlerts.filter(a => a.id !== instanceId);

  // Remove tile
  const tile = document.querySelector(`[data-instance-id="${instanceId}"]`);
  if (tile) tile.remove();

  // Close overlay if no alerts remain
  if (state.activeAlerts.length === 0) {
    hideAlerts();
  }
}
```

### 4. Schedule Generation

#### Generate Schedule Instances
```javascript
function generateScheduleInstances(fromDate) {
  const instances = [];
  const endDate = new Date(fromDate.getTime() + 24 * 60 * 60 * 1000);

  state.data.studentMedications.forEach(sm => {
    if (!sm.active) return;

    const medication = state.data.medications.find(m => m.id === sm.medicationId);
    if (!medication || medication.repeatRule === 'as-needed') return;

    medication.times.forEach(timeStr => {
      const [hours, minutes] = timeStr.split(':').map(Number);
      const scheduleDate = new Date(fromDate);
      scheduleDate.setHours(hours, minutes, 0, 0);

      if (scheduleDate >= fromDate && scheduleDate <= endDate) {
        instances.push({
          id: generateId(),
          studentId: sm.studentId,
          medicationId: sm.medicationId,
          dueAt: scheduleDate.toISOString(),
          status: 'PENDING',
          createdAt: fromDate.toISOString()
        });
      }
    });
  });

  state.data.scheduleInstances = instances;
}
```

### 5. Time Simulation

#### Time Offset System
```javascript
function getCurrentTime() {
  const offset = parseInt(localStorage.getItem(CONFIG.TIME_SIMULATION_KEY) || '0');
  return new Date(Date.now() + offset * 60000);
}

function showTimeSimulator() {
  const currentOffset = parseInt(localStorage.getItem(CONFIG.TIME_SIMULATION_KEY) || '0');
  const minutes = prompt(`Enter minutes to add to current time:`, currentOffset);
  
  if (minutes !== null) {
    localStorage.setItem(CONFIG.TIME_SIMULATION_KEY, minutes);
    renderDashboard();
    checkForAlerts(); // Immediate check
  }
}
```

### 6. Navigation System

#### SPA Router
```javascript
function navigateTo(page, param) {
  state.currentPage = page;
  const app = document.getElementById('app');
  app.innerHTML = '';

  switch (page) {
    case 'login':
      renderLogin();
      break;
    case 'dashboard':
      if (!requireAuth()) return;
      renderDashboard();
      startAlertMonitoring();
      break;
    // ... other routes
  }
}
```

### 7. Data Persistence

#### Save to localStorage
```javascript
function saveData() {
  try {
    localStorage.setItem(CONFIG.STORAGE_KEY, JSON.stringify(state.data));
    return true;
  } catch (e) {
    console.error('Failed to save data:', e);
    return false;
  }
}
```

#### Load from localStorage
```javascript
function loadData() {
  try {
    const stored = localStorage.getItem(CONFIG.STORAGE_KEY);
    if (stored) {
      state.data = JSON.parse(stored);
      return true;
    }
  } catch (e) {
    console.error('Failed to load data:', e);
  }
  return false;
}
```

---

## Security Considerations

### Current Implementation (Demo)
- Simple password hashing (not production-ready)
- Client-side authentication
- localStorage for data persistence
- No server-side validation

### Production Recommendations
1. **Authentication:**
   - Use bcrypt or Argon2 for password hashing
   - Implement JWT tokens
   - Add server-side session management
   - Implement rate limiting on login attempts

2. **Data Storage:**
   - Move to server-side database (PostgreSQL, MySQL)
   - Implement proper encryption at rest
   - Use HTTPS for all communications
   - Implement backup and recovery procedures

3. **Access Control:**
   - Implement proper RBAC on server
   - Add audit logging for all actions
   - Implement IP whitelisting if needed
   - Add two-factor authentication

4. **Input Validation:**
   - Server-side validation for all inputs
   - Sanitise all user inputs
   - Implement CSRF protection
   - Add XSS protection headers

---

## Performance Optimisation

### Current Optimisations
1. **Efficient DOM Updates:**
   - Minimal re-renders
   - Event delegation where possible
   - Debounced search inputs

2. **Data Management:**
   - In-memory state for fast access
   - Lazy loading of detail pages
   - Efficient filtering algorithms

3. **Alert System:**
   - 10-second check interval (configurable)
   - Efficient date comparisons
   - Minimal DOM manipulation

### Future Optimisations
1. Implement virtual scrolling for large lists
2. Add service worker for offline support
3. Implement data pagination
4. Add request caching
5. Optimise bundle size with tree shaking

---

## Browser Compatibility

### Tested Browsers
- Chrome 90+ ✅
- Edge 90+ ✅
- Firefox 88+ ✅
- Safari 14+ ✅

### Required APIs
- localStorage
- sessionStorage
- Web Audio API
- Web Speech API (SpeechSynthesis)
- ES6+ JavaScript features

### Polyfills Needed for Older Browsers
- Promise
- Array.prototype.find
- Array.prototype.filter
- Object.assign
- fetch API

---

## Accessibility Features

### WCAG 2.2 AA Compliance

1. **Keyboard Navigation:**
   - All interactive elements accessible via Tab
   - Focus indicators on all focusable elements
   - Logical tab order

2. **Screen Reader Support:**
   - Semantic HTML structure
   - ARIA labels where needed
   - Alt text for images
   - Descriptive link text

3. **Visual Design:**
   - High contrast colors (4.5:1 minimum)
   - Large touch targets (44x44px minimum)
   - Clear visual hierarchy
   - Readable font sizes (16px base)

4. **Motion:**
   - Respects prefers-reduced-motion
   - Optional animations
   - No auto-playing content (except alerts)

---

## Testing

### Manual Testing Checklist

**Authentication:**
- [ ] Login with valid credentials
- [ ] Login with invalid credentials
- [ ] Logout functionality
- [ ] Session persistence
- [ ] Role-based access control

**Students:**
- [ ] Create new student
- [ ] Edit student
- [ ] View student details
- [ ] Link medication to student
- [ ] Unlink medication from student
- [ ] Search students

**Medications:**
- [ ] Create new medication
- [ ] Edit medication
- [ ] View medication details
- [ ] Search medications

**Alerts:**
- [ ] Single alert display
- [ ] Multiple alerts display
- [ ] Audio playback
- [ ] Voice announcement
- [ ] Administered action
- [ ] Snooze action
- [ ] Open details action
- [ ] Overdue detection

**Administration Log:**
- [ ] View log entries
- [ ] Filter by search
- [ ] Filter by status
- [ ] Export to CSV

**Admin:**
- [ ] Create staff account
- [ ] Edit staff account
- [ ] Deactivate staff account
- [ ] Activate staff account
- [ ] Delete staff account

### Automated Testing (Future)
- Unit tests for utility functions
- Integration tests for data operations
- E2E tests for critical workflows
- Accessibility testing with axe-core

---

## Deployment

### Static Hosting
The application can be deployed to any static hosting service:
- GitHub Pages
- Netlify
- Vercel
- AWS S3 + CloudFront
- Azure Static Web Apps

### Deployment Steps
1. Upload `index.html`, `styles.css`, `app.js` to hosting
2. Configure MIME types (if needed)
3. Enable HTTPS
4. Set up custom domain (optional)
5. Configure caching headers

### Environment Configuration
No environment variables needed for current version.

For production:
- API endpoint configuration
- Authentication service URLs
- Analytics tracking IDs
- Error reporting service keys

---

## Maintenance

### Regular Tasks
1. **Data Backup:**
   - Export administration logs regularly
   - Backup localStorage data
   - Archive old records

2. **Monitoring:**
   - Check browser console for errors
   - Monitor alert system functionality
   - Verify audio/voice features

3. **Updates:**
   - Keep browser requirements updated
   - Test on new browser versions
   - Update dependencies (if any added)

### Known Limitations
1. Data stored in localStorage (10MB limit)
2. No server-side backup
3. Single-device usage (no sync)
4. Browser-dependent audio/voice
5. No offline support

---

## Future Enhancements

### Planned Features
1. **Multi-device Sync:**
   - Cloud database integration
   - Real-time updates
   - Conflict resolution

2. **Advanced Scheduling:**
   - Recurring patterns (every other day, etc.)
   - Holiday schedules
   - Temporary medication holds

3. **Reporting:**
   - Compliance reports
   - Trend analysis
   - Custom report builder

4. **Notifications:**
   - Email notifications
   - SMS alerts
   - Push notifications

5. **Mobile App:**
   - Native iOS app
   - Native Android app
   - Progressive Web App (PWA)

6. **Integration:**
   - Student information system integration
   - Health records integration
   - Parent portal

---

## API Documentation (Future)

When backend is implemented, API will follow RESTful conventions:

### Authentication
```
POST /api/auth/login
POST /api/auth/logout
GET  /api/auth/me
```

### Students
```
GET    /api/students
GET    /api/students/:id
POST   /api/students
PUT    /api/students/:id
DELETE /api/students/:id
```

### Medications
```
GET    /api/medications
GET    /api/medications/:id
POST   /api/medications
PUT    /api/medications/:id
DELETE /api/medications/:id
```

### Schedule
```
GET    /api/schedule
GET    /api/schedule/:id
POST   /api/schedule/:id/administer
POST   /api/schedule/:id/snooze
```

### Administration Log
```
GET    /api/log
GET    /api/log/:id
POST   /api/log
```

### Staff (Admin Only)
```
GET    /api/staff
GET    /api/staff/:id
POST   /api/staff
PUT    /api/staff/:id
DELETE /api/staff/:id
```

---

## Contributing

### Code Style
- Use ES6+ features
- Follow existing naming conventions
- Add comments for complex logic
- Keep functions small and focused
- Use meaningful variable names

### Git Workflow
1. Create feature branch
2. Make changes
3. Test thoroughly
4. Submit pull request
5. Code review
6. Merge to main

---

## License

Copyright © 2025 Chorley Mayfield Primary School. All rights reserved.

---

## Contact

For technical support or questions:
- Email: support@ninjatech.ai
- Documentation: See README.md and USER_GUIDE.md

---

**Last Updated:** January 2025  
**Version:** 1.0  
**Maintainer:** NinjaTech AI Team