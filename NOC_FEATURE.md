# NOC (No Objection Certificate) Feature

## Overview
The NOC feature allows students to request No Objection Certificates from faculty for various purposes like internships, placements, higher studies, projects, and events.

## Features

### Student Features
1. **Request NOC** - Students can submit NOC requests with:
   - Course selection (from enrolled courses)
   - Purpose (internship, placement, higher-studies, project, event, other)
   - Organization name
   - Description/reason
   - Start and end dates

2. **View NOC Status** - Students can see all their NOC requests with:
   - Status badges (Pending, Approved, Rejected)
   - Color-coded cards based on status
   - Faculty remarks (if any)
   - Request details

3. **Delete Pending Requests** - Students can delete NOC requests that are still pending

### Faculty Features
1. **View NOC Requests** - Faculty can see all NOC requests for their courses
2. **Approve NOC** - Faculty can approve requests with optional remarks
3. **Reject NOC** - Faculty can reject requests with mandatory reason
4. **View Student Details** - See student name, email, and request details

## Database Schema

### NOC Model
```javascript
{
  student: ObjectId (ref: User),
  course: ObjectId (ref: Course),
  faculty: ObjectId (ref: User),
  purpose: String (enum),
  description: String,
  organizationName: String,
  startDate: Date,
  endDate: Date,
  status: String (pending/approved/rejected),
  facultyRemarks: String,
  approvedAt: Date,
  rejectedAt: Date,
  timestamps: true
}
```

## API Endpoints

### Student Endpoints
- `GET /api/noc/student` - Get all NOC requests for logged-in student
- `POST /api/noc` - Create new NOC request
- `DELETE /api/noc/:id` - Delete pending NOC request

### Faculty Endpoints
- `GET /api/noc/faculty` - Get all NOC requests for faculty
- `GET /api/noc/course/:courseId` - Get NOC requests for specific course
- `POST /api/noc/:id/approve` - Approve NOC request
- `POST /api/noc/:id/reject` - Reject NOC request
- `DELETE /api/noc/:id` - Delete NOC request

## UI Components

### Student Dashboard
- New "NOC Requests" tab in sidebar (📄 icon)
- Request NOC form with all required fields
- NOC list with color-coded status cards
- Delete button for pending requests

### Faculty Dashboard
- New "NOC Requests" tab in sidebar (📄 icon)
- NOC list showing student details
- Approve/Reject buttons for pending requests
- Remarks input via prompt dialogs

## Status Colors
- **Pending**: Orange/Yellow background
- **Approved**: Green background
- **Rejected**: Red background

## Validation
- Students must be enrolled in the course to request NOC
- Only pending NOC requests can be deleted by students
- Faculty can only approve/reject NOCs for their own courses
- Rejection requires a reason/remark

## Usage Flow

1. **Student submits NOC request**
   - Selects enrolled course
   - Fills in purpose, organization, dates, description
   - Submits request

2. **Faculty reviews request**
   - Views request in NOC Requests tab
   - Reviews student details and purpose
   - Approves with optional remarks OR rejects with mandatory reason

3. **Student views status**
   - Sees updated status (Approved/Rejected)
   - Reads faculty remarks
   - Can delete if still pending

## Files Modified/Created

### Backend
- `backend/models/NOC.js` - NOC model
- `backend/routes/noc.js` - NOC routes
- `backend/server.js` - Added NOC routes

### Frontend
- `frontend/src/components/Dashboard.js` - Student NOC feature
- `frontend/src/components/Dashboard.css` - NOC styling
- `frontend/src/components/FacultyDashboard.js` - Faculty NOC feature
- `frontend/src/components/FacultyDashboard.css` - NOC styling

## Testing
1. Login as student
2. Navigate to "NOC Requests" tab
3. Click "Request NOC"
4. Fill form and submit
5. Login as faculty
6. Navigate to "NOC Requests" tab
7. Approve or reject the request
8. Login back as student to see updated status
