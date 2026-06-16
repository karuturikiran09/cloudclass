# Enrollment Approval System

## Overview
Faculty must now approve student enrollment requests before students can access course content.

## How It Works

### For Students:

1. **Browse Courses**
   - Go to "Browse Courses" tab
   - View all available courses

2. **Request Enrollment**
   - Click "Enroll Now" button
   - Request is sent to faculty
   - Button changes to "⏳ Pending Approval"

3. **Wait for Approval**
   - Student cannot access course content until approved
   - Status shows as "Pending Approval"

4. **After Approval**
   - Course appears in "My Courses" tab
   - Button shows "✓ Enrolled"
   - Can access videos and assignments

### For Faculty:

1. **View Pending Requests**
   - Select a course
   - Click "Pending Approvals" tab
   - See count badge: "Pending Approvals (3)"

2. **Review Student Requests**
   - See student name, email, and request date
   - View all pending requests in a table

3. **Approve or Reject**
   - **✓ Approve**: Student gets access to course
   - **✗ Reject**: Request is removed

4. **After Approval**
   - Student moves to "Student Performance" tab
   - Student can access course content
   - Pending count updates automatically

## Student Enrollment States

### 1. Not Enrolled
- Button: "Enroll Now" (Blue)
- Can click to request enrollment

### 2. Pending Approval
- Button: "⏳ Pending Approval" (Orange)
- Cannot access course content
- Waiting for faculty approval

### 3. Enrolled
- Button: "✓ Enrolled" (Green)
- Full access to course content
- Can view videos and assignments

## API Endpoints

### Student Endpoints
- `POST /api/courses/:id/enroll` - Request enrollment (creates pending request)
- `GET /api/courses/my-courses` - Get enrolled courses only

### Faculty Endpoints
- `GET /api/courses/:id/pending` - Get pending enrollment requests
- `POST /api/courses/:id/approve/:studentId` - Approve student enrollment
- `POST /api/courses/:id/reject/:studentId` - Reject student enrollment

## Database Schema Updates

### Course Model
```javascript
{
  title: String,
  description: String,
  faculty: ObjectId,
  students: [ObjectId],  // Approved students
  pendingStudents: [{    // NEW: Pending approval
    student: ObjectId,
    requestedAt: Date
  }],
  code: String,
  createdAt: Date
}
```

## User Flow Example

### Student Perspective:
1. Student logs in
2. Browses courses
3. Clicks "Enroll Now" on "Web Development 101"
4. Sees "⏳ Pending Approval"
5. Waits for faculty approval
6. After approval, course appears in "My Courses"
7. Can now watch videos and view assignments

### Faculty Perspective:
1. Faculty logs in
2. Selects "Web Development 101" course
3. Sees "Pending Approvals (1)" tab
4. Reviews student request
5. Clicks "✓ Approve"
6. Student now appears in "Student Performance" tab
7. Student can access course content

## Benefits

✅ **Quality Control**: Faculty can verify students before granting access
✅ **Course Management**: Better control over who joins courses
✅ **Clear Status**: Students know exactly where they stand
✅ **Notification System**: Badge shows pending count
✅ **Easy Management**: One-click approve/reject

## Current Status
✅ Frontend: http://localhost:3000
✅ Backend: http://localhost:5000
✅ MongoDB: Running locally
✅ Approval System: Fully functional
✅ Student Requests: Working
✅ Faculty Approvals: Working
