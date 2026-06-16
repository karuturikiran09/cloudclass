# Assignment Creation & Grading Feature

## Overview
Faculty can now create assignments, view student submissions, and grade them directly from the dashboard.

## Features

### 1. Create Assignments
Faculty can create assignments with:
- **Title**: Assignment name
- **Description**: Detailed instructions
- **Due Date**: Submission deadline
- **Maximum Points**: Total points possible

### 2. View Assignments
- See all assignments for a course
- View submission count for each assignment
- Expandable submission details

### 3. Grade Submissions
- View all student submissions
- See submission date and files
- Enter grades (0 to max points)
- Provide written feedback
- Save grades individually

### 4. Track Progress
- Submission count displayed
- Student names and submission dates
- File links for review
- Current grades visible

## How to Use

### Create an Assignment

1. **Login as Faculty**
2. **Select a Course** from your courses
3. **Click "Assignments" tab**
4. **Click "Create Assignment"** button
5. **Fill in details**:
   - Title: "Week 1 Quiz"
   - Description: "Complete the quiz on HTML basics"
   - Due Date: Select from calendar
   - Max Points: 100
6. **Click "Create Assignment"**
7. Assignment appears in the list

### View Submissions

1. **Go to Assignments tab**
2. **Find your assignment**
3. **Click "View Submissions (X)"** button
4. Submissions table expands showing:
   - Student names
   - Submission dates
   - File links
   - Current grades
   - Feedback

### Grade a Submission

1. **Open submissions** for an assignment
2. **Click "View File"** to review student work
3. **Enter grade** in the Grade column (0-100)
4. **Enter feedback** in the Feedback column
5. **Click "Save Grade"** button
6. Grade is saved and student can see it

### Close Submissions View

1. **Click "Close"** button at bottom
2. Returns to assignments list

## Assignment Card Layout

```
┌─────────────────────────────────────────────────┐
│ Week 1 Quiz                    [View Submissions (3)] │
│ Due: 02/25/2026 | Max Points: 100              │
│                                                 │
│ Complete the quiz on HTML basics               │
│                                                 │
│ [Submissions Table - when expanded]            │
└─────────────────────────────────────────────────┘
```

## Submissions Table

| Student | Submitted At | File | Grade | Feedback | Actions |
|---------|-------------|------|-------|----------|---------|
| John Doe | 02/20/2026 | [View File] | [80] | [Good work] | [Save Grade] |
| Jane Smith | 02/21/2026 | [View File] | [95] | [Excellent] | [Save Grade] |

## API Endpoints

### Create Assignment
```
POST /api/assignments
Headers: Authorization: Bearer <token>
Body: {
  title: string,
  description: string,
  course: ObjectId,
  dueDate: Date,
  maxPoints: number
}
```

### Get Course Assignments
```
GET /api/assignments/course/:courseId
Headers: Authorization: Bearer <token>
```

### Grade Submission
```
POST /api/assignments/:id/grade/:submissionId
Headers: Authorization: Bearer <token>
Body: {
  grade: number,
  feedback: string
}
```

## Database Schema

### Assignment Model
```javascript
{
  title: String (required),
  description: String,
  course: ObjectId (required),
  dueDate: Date (required),
  maxPoints: Number (default: 100),
  submissions: [{
    student: ObjectId,
    fileUrl: String,
    submittedAt: Date,
    grade: Number,
    feedback: String
  }],
  createdAt: Date
}
```

## Grading Workflow

### Step 1: Student Submits
- Student uploads assignment file
- Submission appears in faculty view
- Shows as "ungraded" (no grade value)

### Step 2: Faculty Reviews
- Faculty clicks "View File"
- Reviews student work
- Returns to grading table

### Step 3: Faculty Grades
- Enters numerical grade
- Adds written feedback
- Clicks "Save Grade"

### Step 4: Grade Saved
- Grade stored in database
- Student can view grade
- Appears in student performance metrics

## Validation

### Assignment Creation
- ✅ Title is required
- ✅ Due date is required
- ✅ Max points must be positive number
- ✅ Description is optional

### Grading
- ✅ Grade must be between 0 and max points
- ✅ Feedback is optional
- ✅ Only faculty can grade
- ✅ Only course owner can grade their assignments

## User Interface

### Empty State
- **No Assignments**: "No assignments created yet"
- **No Submissions**: "No submissions yet"

### Visual Feedback
- **Create button**: Blue primary button
- **View Submissions**: Gray secondary button
- **Save Grade**: Green approve button
- **Close**: Gray secondary button

### Form Layout
- Clean, card-based design
- Clear labels and placeholders
- Date picker for due dates
- Number input for points
- Textarea for descriptions

## Benefits

✅ **Streamlined Grading**: Grade all submissions in one place
✅ **Quick Feedback**: Provide immediate feedback to students
✅ **Progress Tracking**: See submission counts at a glance
✅ **Organized**: All assignments grouped by course
✅ **Flexible**: Set custom point values
✅ **Transparent**: Students see grades and feedback

## Integration with Student Performance

Grades automatically feed into:
- Student Performance dashboard
- Average grade calculations
- Performance bars and metrics
- Individual student reports

## Current Status
✅ Frontend: http://localhost:3000
✅ Backend: http://localhost:5000
✅ MongoDB: Running locally
✅ Assignment Creation: Fully functional
✅ Submission Viewing: Working
✅ Grading System: Implemented
✅ Feedback System: Active

## Future Enhancements (Optional)
- File upload directly in platform
- Bulk grading
- Grade distribution charts
- Assignment templates
- Rubric-based grading
- Late submission penalties
- Grade curves
- Export grades to CSV
