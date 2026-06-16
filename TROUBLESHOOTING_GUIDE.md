# CloudClass Troubleshooting Guide

## Current Status
✅ **Frontend**: Compiled successfully at http://localhost:3000
✅ **Backend**: Running on port 5000
✅ **MongoDB**: Connected and running
✅ **All warnings**: Fixed

## How to Access the Application

### Step 1: Open Browser
Go to: **http://localhost:3000**

### Step 2: You Should See
- **Landing Page** with two cards:
  - Student Login (Purple theme)
  - Faculty Login (Pink theme)

### Step 3: Choose Your Role
- Click "Student Login" for student access
- Click "Faculty Login" for faculty access

## Common Issues & Solutions

### Issue 1: Blank Page or "Cannot GET /"
**Solution**: 
1. Clear browser cache: Ctrl+Shift+R (Windows) or Cmd+Shift+R (Mac)
2. Check if frontend is running: http://localhost:3000
3. Check browser console (F12) for errors

### Issue 2: "Cannot connect to server"
**Solution**:
1. Verify backend is running: http://localhost:5000/api/health
2. Check backend terminal for errors
3. Restart backend: `cd backend && npm run dev`

### Issue 3: MongoDB Connection Error
**Solution**:
1. Check if MongoDB is running
2. Verify MongoDB process in Task Manager
3. Restart MongoDB (see MongoDB section below)

### Issue 4: Login Not Working
**Solution**:
1. Check browser console (F12) for errors
2. Verify you're using correct role login
3. Check Network tab for API call status
4. Try registering a new account

### Issue 5: Pending Enrollments Not Showing
**Solution**:
1. Open browser console (F12)
2. Look for logs: "Courses fetched", "Pending enrollments response"
3. Verify student has requested enrollment
4. Check you're logged in as course creator

## Testing the Complete Flow

### Test 1: Student Registration & Login
```
1. Go to http://localhost:3000
2. Click "Student Login"
3. Click "Don't have an account? Register"
4. Fill in:
   - Name: Test Student
   - Email: student@test.com
   - Password: test123
5. Click "Register as Student"
6. Should redirect to student dashboard
```

### Test 2: Faculty Registration & Login
```
1. Go to http://localhost:3000
2. Click "Faculty Login"
3. Click "Don't have an account? Register"
4. Fill in:
   - Name: Test Faculty
   - Email: faculty@test.com
   - Password: test123
5. Click "Register as Faculty"
6. Should redirect to faculty dashboard
```

### Test 3: Course Creation (Faculty)
```
1. Login as faculty
2. Click "Create Course"
3. Fill in:
   - Title: Web Development 101
   - Code: WEB101
   - Description: Learn web development
4. Click "Create"
5. Course should appear in grid
```

### Test 4: Student Enrollment
```
1. Login as student
2. Go to "Browse Courses" tab
3. Find "Web Development 101"
4. Click "Enroll Now"
5. Button should change to "⏳ Pending Approval"
```

### Test 5: Faculty Approval
```
1. Login as faculty (course creator)
2. Click on "Web Development 101" course
3. Should see orange badge "1 pending"
4. Click "Pending Approvals (1)" tab
5. Should see student in table
6. Click "✓ Approve"
7. Student should move to "Student Performance" tab
```

### Test 6: Assignment Creation
```
1. Login as faculty
2. Select a course
3. Click "Assignments" tab
4. Click "Create Assignment"
5. Fill in details
6. Click "Create Assignment"
7. Assignment should appear in list
```

### Test 7: Video Upload
```
1. Login as faculty
2. Select a course
3. Click "Course Videos" tab
4. Click "Upload Video"
5. Fill in video details
6. Click "Upload"
7. Video should appear in grid
```

## Restart Services

### Restart Frontend
```bash
# Stop: Ctrl+C in frontend terminal
cd frontend
npm start
```

### Restart Backend
```bash
# Stop: Ctrl+C in backend terminal
cd backend
npm run dev
```

### Restart MongoDB
```bash
# If running as service
net stop MongoDB
net start MongoDB

# If running manually
# Stop: Ctrl+C in MongoDB terminal
mongod --dbpath C:\data\db
```

## Check Service Status

### Check Frontend
```
Open: http://localhost:3000
Should see: Landing page with login options
```

### Check Backend
```
Open: http://localhost:5000/api/health
Should see: {"status":"ok","message":"CloudClass API is running"}
```

### Check MongoDB
```bash
# In command prompt
mongosh
# Should connect without errors
```

## Browser Console Debugging

### Open Console
1. Press F12
2. Click "Console" tab

### Look For:
- ✅ No red errors
- ✅ "Courses fetched" logs (when selecting course)
- ✅ "Pending enrollments response" logs
- ❌ Any red error messages

### Common Console Errors:

**Error**: "Failed to fetch"
**Solution**: Backend not running, restart backend

**Error**: "401 Unauthorized"
**Solution**: Token expired, logout and login again

**Error**: "Network Error"
**Solution**: Check if backend is on port 5000

## Database Verification

### Check Collections
```javascript
// In mongosh
use cloudclass
show collections
// Should see: users, courses, assignments, videos

// Check users
db.users.find().pretty()

// Check courses
db.courses.find().pretty()

// Check pending enrollments
db.courses.find({ "pendingStudents.0": { $exists: true } }).pretty()
```

## Port Conflicts

### If Port 3000 is Busy
```bash
# Find process using port 3000
netstat -ano | findstr :3000
# Kill process (replace PID)
taskkill /PID <PID> /F
```

### If Port 5000 is Busy
```bash
# Find process using port 5000
netstat -ano | findstr :5000
# Kill process (replace PID)
taskkill /PID <PID> /F
```

## Clear Everything and Start Fresh

### Clear Browser Data
1. Press Ctrl+Shift+Delete
2. Select "Cached images and files"
3. Select "Cookies and other site data"
4. Click "Clear data"

### Clear npm Cache
```bash
npm cache clean --force
```

### Reinstall Dependencies
```bash
# Frontend
cd frontend
rm -rf node_modules
npm install

# Backend
cd backend
rm -rf node_modules
npm install
```

## Expected Behavior

### Landing Page (/)
- Shows CloudClass title
- Two cards: Student Login, Faculty Login
- Feature badges at bottom
- Gradient background

### Student Login (/student/login)
- Purple gradient background
- Graduation cap icon
- Email and password fields
- "Login as Student" button
- Links to register and faculty login

### Faculty Login (/faculty/login)
- Pink gradient background
- Teacher icon
- Email and password fields
- "Login as Faculty" button
- Links to register and student login

### Student Dashboard
- Tabs: My Profile, My Courses, Browse Courses
- Can enroll in courses
- Can view videos and assignments
- Profile with avatar

### Faculty Dashboard
- Tabs: Courses, Pending Approvals, Students, Assignments, Videos
- Can create courses
- Can approve enrollments
- Can create assignments and grade
- Can upload videos

## Still Having Issues?

### Check These Files Exist:
```
frontend/src/components/LandingPage.js
frontend/src/components/StudentLogin.js
frontend/src/components/FacultyLogin.js
frontend/src/components/StudentRegister.js
frontend/src/components/FacultyRegister.js
frontend/src/components/Dashboard.js
frontend/src/components/FacultyDashboard.js
```

### Verify Routes in App.js:
```javascript
/                      → LandingPage
/student/login         → StudentLogin
/student/register      → StudentRegister
/faculty/login         → FacultyLogin
/faculty/register      → FacultyRegister
/dashboard             → Dashboard (role-based)
```

### Check Environment Variables:
```
backend/.env should contain:
PORT=5000
MONGODB_URI=mongodb://localhost:27017/cloudclass
JWT_SECRET=cloudclass_secret_key_change_in_production_2024
NODE_ENV=development
```

## Quick Health Check

Run these commands to verify everything:

```bash
# Check if services are running
curl http://localhost:3000
curl http://localhost:5000/api/health

# Check MongoDB
mongosh --eval "db.version()"

# Check Node version
node --version

# Check npm version
npm --version
```

## Contact Information

If issues persist:
1. Check browser console (F12)
2. Check backend terminal for errors
3. Check MongoDB logs
4. Verify all services are running
5. Try the test flows above

## Success Indicators

✅ Landing page loads at http://localhost:3000
✅ Can navigate to student/faculty login
✅ Can register new accounts
✅ Can login successfully
✅ Dashboard loads with correct role
✅ Can create courses (faculty)
✅ Can enroll in courses (student)
✅ Can approve enrollments (faculty)
✅ No errors in browser console
✅ Backend responds at port 5000
✅ MongoDB is connected
