# Data Management Guide

## Overview

The Medication Tracker stores all data locally in your browser's localStorage. This guide explains how to backup, restore, export, and manage your data.

---

## Understanding Data Storage

### What is localStorage?

- **Client-side storage**: Data is stored in your browser, not on a server
- **Per-browser**: Each browser has its own separate data
- **Per-device**: Data doesn't sync between devices
- **Persistent**: Data remains even after closing the browser
- **Capacity**: Typically 5-10MB (sufficient for this application)

### What Data is Stored?

The system stores:
- **Staff accounts** (usernames, passwords, roles)
- **Students** (names, classes, medical notes)
- **Medications** (names, dosages, times, instructions)
- **Student-Medication links** (which students take which medications)
- **Schedule instances** (all scheduled medication times)
- **Administration log** (complete history of administered medications)

---

## Backup Methods

### Method 1: Browser Console Backup (Recommended)

This method creates a complete backup of all your data.

**Steps:**

1. **Open the application** in your browser
2. **Open Developer Console**:
   - Chrome/Edge: Press `F12` or `Ctrl+Shift+J` (Windows) / `Cmd+Option+J` (Mac)
   - Firefox: Press `F12` or `Ctrl+Shift+K` (Windows) / `Cmd+Option+K` (Mac)
   - Safari: Enable Developer menu first, then `Cmd+Option+C`

3. **Copy and paste this command** into the console:

```javascript
// Create backup
const backup = localStorage.getItem('mayfield-medication-tracker');
const blob = new Blob([backup], { type: 'application/json' });
const url = URL.createObjectURL(blob);
const a = document.createElement('a');
a.href = url;
a.download = `mayfield-backup-${new Date().toISOString().split('T')[0]}.json`;
a.click();
```

4. **Press Enter** - A file will download with today's date (e.g., `mayfield-backup-2025-10-16.json`)

5. **Save this file** in a secure location (cloud storage, external drive, etc.)

**Backup Frequency Recommendations:**
- **Daily**: If actively using the system
- **Weekly**: For established systems with stable data
- **Before major changes**: Always backup before adding many new records
- **Before updates**: Backup before updating the application

---

### Method 2: Export Administration Log (Partial Backup)

This exports only the administration history as a CSV file.

**Steps:**

1. Log in to the application
2. Go to **Administration Log**
3. Click **"Export to CSV"** button
4. Save the file (e.g., `medication-log-2025-10-16.csv`)

**Note:** This only backs up the administration history, not students, medications, or schedules.

---

## Restore Methods

### Method 1: Browser Console Restore

This restores a complete backup created with Method 1 above.

**Steps:**

1. **Open the application** in your browser
2. **Open Developer Console** (F12)
3. **Copy and paste this command**:

```javascript
// Restore backup - Step 1: Prepare
const input = document.createElement('input');
input.type = 'file';
input.accept = '.json';
input.onchange = function(e) {
    const file = e.target.files[0];
    const reader = new FileReader();
    reader.onload = function(event) {
        try {
            // Verify it's valid JSON
            JSON.parse(event.target.result);
            // Store it
            localStorage.setItem('mayfield-medication-tracker', event.target.result);
            alert('Backup restored successfully! Please refresh the page.');
        } catch (error) {
            alert('Error: Invalid backup file. ' + error.message);
        }
    };
    reader.readAsText(file);
};
input.click();
```

4. **Press Enter**
5. **Select your backup file** (the .json file you saved earlier)
6. **Wait for confirmation** message
7. **Refresh the page** (F5 or Ctrl+R)

**Your data is now restored!**

---

### Method 2: Manual Data Entry

If you don't have a backup, you'll need to manually re-enter:
- Staff accounts
- Students
- Medications
- Link medications to students

The system will automatically generate new schedules based on your medication settings.

---

## Data Migration Between Browsers/Devices

### Scenario: Moving from Computer A to Computer B

**On Computer A (Export):**

1. Create a backup using Method 1 above
2. Save the .json file
3. Transfer file to Computer B (email, USB drive, cloud storage, etc.)

**On Computer B (Import):**

1. Open the application
2. Restore the backup using Method 1 above
3. Refresh the page

**Important Notes:**
- Both computers must use the same application version
- Data will overwrite any existing data on Computer B
- Consider backing up Computer B's data first if it has any

---

## Data Export Options

### Export Administration Log (CSV)

**Use Case:** Reporting, record-keeping, analysis

**Steps:**
1. Go to Administration Log
2. Click "Export to CSV"
3. Open in Excel, Google Sheets, or any spreadsheet software

**CSV Contents:**
- Date and time administered
- Student name
- Medication name
- Dosage
- Instructions
- Staff member
- Status (on-time/late)
- Notes

---

## Data Clearing

### Clear All Data (Factory Reset)

**Warning:** This permanently deletes ALL data. Backup first!

**Steps:**

1. Open Developer Console (F12)
2. Copy and paste:

```javascript
// Clear all data
if (confirm('⚠️ WARNING: This will delete ALL data. Are you sure?')) {
    localStorage.removeItem('mayfield-medication-tracker');
    alert('All data cleared. Page will refresh.');
    location.reload();
}
```

3. Press Enter
4. Confirm the warning
5. Page will refresh with empty system

**When to use:**
- Starting fresh with new data
- Troubleshooting data corruption
- Switching to a different school/organisation

---

## Data Verification

### Check Data Integrity

Verify your data is intact:

**Steps:**

1. Open Developer Console (F12)
2. Copy and paste:

```javascript
// Check data integrity
const data = JSON.parse(localStorage.getItem('mayfield-medication-tracker') || '{}');
console.log('Data Integrity Check:');
console.log('Staff accounts:', data.staff?.length || 0);
console.log('Students:', data.students?.length || 0);
console.log('Medications:', data.medications?.length || 0);
console.log('Student-Medication links:', data.studentMedications?.length || 0);
console.log('Schedule instances:', data.scheduleInstances?.length || 0);
console.log('Administration log entries:', data.administrationLog?.length || 0);
console.log('Total data size:', new Blob([JSON.stringify(data)]).size, 'bytes');
```

3. Press Enter
4. Review the output

**Healthy System Should Show:**
- At least 1 staff account
- Your students count
- Your medications count
- Linked medications
- Schedule instances
- Administration log entries

---

## Troubleshooting

### "Data disappeared after closing browser"

**Possible causes:**
- Browser in private/incognito mode
- Browser set to clear data on exit
- Different browser or device
- Browser cache cleared

**Solution:**
- Always use normal (non-private) browsing mode
- Check browser settings for data retention
- Use same browser and device
- Restore from backup

---

### "Storage quota exceeded"

**Cause:** Too much data (rare, but possible with years of logs)

**Solution:**

1. Export administration log to CSV
2. Clear old log entries:

```javascript
// Keep only last 90 days of logs
const data = JSON.parse(localStorage.getItem('mayfield-medication-tracker'));
const ninetyDaysAgo = new Date();
ninetyDaysAgo.setDate(ninetyDaysAgo.getDate() - 90);
data.administrationLog = data.administrationLog.filter(log => 
    new Date(log.administeredAt) > ninetyDaysAgo
);
localStorage.setItem('mayfield-medication-tracker', JSON.stringify(data));
alert('Old logs cleared. Page will refresh.');
location.reload();
```

---

### "Data corrupted or invalid"

**Symptoms:**
- Application won't load
- Errors on screen
- Missing data

**Solution:**

1. Try restoring from backup
2. If no backup, clear data and start fresh
3. Contact support if issues persist

---

## Best Practices

### Daily Operations

✅ **DO:**
- Keep the application open in the same browser
- Use normal (non-private) browsing mode
- Export administration log weekly
- Create backups before major changes

❌ **DON'T:**
- Use private/incognito mode
- Clear browser data without backing up first
- Switch between browsers without migrating data
- Assume data syncs between devices

---

### Backup Strategy

**Recommended Schedule:**

| Frequency | Method | Storage Location |
|-----------|--------|------------------|
| Daily | Administration Log CSV | Cloud storage |
| Weekly | Full JSON Backup | External drive |
| Monthly | Full JSON Backup | Multiple locations |
| Before Updates | Full JSON Backup | Safe location |

---

### Multi-Device Setup

**Not Recommended:** The current version doesn't support multi-device sync.

**If you need multi-device access:**
- Use a single dedicated computer for the system
- OR manually backup/restore when switching devices
- OR consider upgrading to a server-based version (requires backend development)

---

## Advanced: Automated Backups

### Create Automatic Daily Backup

Add this to your browser bookmarks:

```javascript
javascript:(function(){const backup=localStorage.getItem('mayfield-medication-tracker');const blob=new Blob([backup],{type:'application/json'});const url=URL.createObjectURL(blob);const a=document.createElement('a');a.href=url;a.download=`mayfield-backup-${new Date().toISOString().split('T')[0]}.json`;a.click();})();
```

**Usage:**
1. Save as bookmark named "Backup Mayfield Data"
2. Click bookmark daily to create backup
3. File downloads automatically

---

## Data Security

### Password Protection

- Staff passwords are hashed (not stored in plain text)
- Backups contain hashed passwords
- Cannot recover forgotten passwords (must reset)

### Sensitive Data

**What's stored:**
- Student names and medical information
- Medication details
- Administration records

**Security recommendations:**
- Store backups in secure locations
- Encrypt backup files if possible
- Limit access to the computer running the system
- Follow your organisation's data protection policies
- Comply with GDPR/data protection regulations

---

## Support

### Need Help?

**Common Issues:**
- Data loss → Restore from backup
- Can't access data → Check browser and device
- Storage full → Clear old logs
- Corruption → Restore from backup or start fresh

**For Technical Support:**
- Check TROUBLESHOOTING.md
- Review help.html in the application
- Contact your IT administrator (Michael)

---

## Summary

### Quick Reference

**Backup:** Console → Copy backup command → Save .json file  
**Restore:** Console → Copy restore command → Select .json file → Refresh  
**Export Log:** Administration Log → Export to CSV  
**Clear Data:** Console → Copy clear command → Confirm  

**Remember:** Always backup before major changes!

---

**Version:** 1 
**Last Updated:** October, 2025  
**Status:** Production Ready