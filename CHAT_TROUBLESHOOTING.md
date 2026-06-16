# Chat Feature Troubleshooting Guide

## "Failed to send message" Error - Solutions

### Quick Fixes

#### 1. Check if you're logged in
- Make sure you're logged in as a student or faculty
- Check if the token is valid in localStorage
- Try logging out and logging back in

#### 2. Verify course selection
- Make sure you have selected a course
- The chat tab should only appear after selecting a course
- Try selecting the course again

#### 3. Check backend connection
- Backend should be running on http://localhost:5000
- MongoDB should be connected
- Check browser console for detailed error messages

#### 4. Verify enrollment (for students)
- Students must be enrolled and approved in the course
- Faculty must be the course creator
- Check if you have access to other course features

### Detailed Debugging Steps

#### Step 1: Open Browser Console
1. Press F12 or right-click → Inspect
2. Go to Console tab
3. Try sending a message
4. Look for error messages

#### Step 2: Check Network Tab
1. Open Developer Tools (F12)
2. Go to Network tab
3. Try sending a message
4. Look for the POST request to `/api/messages`
5. Check the response status and error message

#### Step 3: Verify Backend Logs
The backend now has detailed logging. Check the terminal where backend is running for:
```
Received message request: { course: '...', sender: '...', content: '...' }
Message saved: ...
Message populated successfully
```

If you see an error, it will show the exact issue.

#### Step 4: Test the Messages Endpoint
Open browser and go to:
```
http://localhost:5000/api/messages/test
```

You should see:
```json
{ "message": "Messages route is working!" }
```

If this doesn't work, the messages route is not loaded properly.

### Common Issues and Solutions

#### Issue 1: "No token, authorization denied"
**Solution**: 
- Log out and log back in
- Clear localStorage and login again
- Check if token is being sent in headers

#### Issue 2: "Course not found" or Invalid course ID
**Solution**:
- Make sure you selected a course
- Try refreshing the page
- Select the course again from "My Courses"

#### Issue 3: "Not enrolled in course"
**Solution** (for students):
- Check if your enrollment was approved
- Go to "Browse Courses" and check enrollment status
- Wait for faculty approval

#### Issue 4: Backend not responding
**Solution**:
- Restart backend: Stop and run `npm run dev` in backend folder
- Check if MongoDB is running
- Verify port 5000 is not in use

#### Issue 5: CORS error
**Solution**:
- Backend has CORS enabled
- Make sure backend is running on port 5000
- Check if frontend is on port 3000

### Manual Testing Steps

#### Test 1: Verify Login
```javascript
// In browser console
console.log(localStorage.getItem('token'));
// Should show a JWT token
```

#### Test 2: Verify Course Selection
```javascript
// In browser console (on Dashboard page)
// Check if selectedCourse is set
// Look for course ID in the page
```

#### Test 3: Test API Directly
```javascript
// In browser console
const token = localStorage.getItem('token');
fetch('http://localhost:5000/api/messages/test')
  .then(r => r.json())
  .then(console.log);
// Should show: { message: "Messages route is working!" }
```

#### Test 4: Send Test Message
```javascript
// In browser console (replace COURSE_ID with actual course ID)
const token = localStorage.getItem('token');
fetch('http://localhost:5000/api/messages', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': `Bearer ${token}`
  },
  body: JSON.stringify({
    course: 'COURSE_ID',
    content: 'Test message',
    type: 'text'
  })
})
.then(r => r.json())
.then(console.log)
.catch(console.error);
```

### Error Messages Explained

#### "Failed to send message: undefined"
- Network error or backend not running
- Check if backend is on port 5000
- Check browser console for CORS errors

#### "Failed to send message: No token, authorization denied"
- Not logged in or token expired
- Solution: Log out and log back in

#### "Failed to send message: Course validation failed"
- Invalid course ID
- Solution: Make sure course is selected

#### "Failed to send message: User validation failed"
- Invalid user ID in token
- Solution: Log out and log back in

### Still Not Working?

If none of the above solutions work:

1. **Restart Everything**:
   ```bash
   # Stop backend (Ctrl+C)
   # Stop frontend (Ctrl+C)
   # Restart MongoDB
   mongod --dbpath C:\data\db
   
   # Start backend
   cd backend
   npm run dev
   
   # Start frontend
   cd frontend
   npm start
   ```

2. **Clear Browser Data**:
   - Clear localStorage
   - Clear cookies
   - Hard refresh (Ctrl+Shift+R)

3. **Check MongoDB**:
   ```bash
   # Connect to MongoDB
   mongosh
   
   # Check if messages collection exists
   use cloudclass
   show collections
   
   # Check if you can insert a test message
   db.messages.insertOne({
     course: ObjectId(),
     sender: ObjectId(),
     content: "test",
     type: "text",
     createdAt: new Date()
   })
   ```

4. **Verify Dependencies**:
   ```bash
   # In backend folder
   npm install
   
   # In frontend folder
   npm install
   ```

### Getting More Help

If you're still experiencing issues:

1. Check the detailed error in browser console
2. Check backend terminal for error logs
3. Copy the exact error message
4. Check if other features (announcements, discussions) are working
5. Verify you can create and view other content

### Success Indicators

You'll know the chat is working when:
- ✅ Chat tab appears after selecting a course
- ✅ You can type in the input field
- ✅ Clicking "Send" clears the input
- ✅ Message appears in the chat area
- ✅ Message shows your name and timestamp
- ✅ Messages auto-refresh every 5 seconds
- ✅ Unread counter updates

### Additional Notes

- Messages are course-specific
- Only enrolled students and course faculty can chat
- Messages refresh every 5 seconds automatically
- Last 100 messages are shown
- You can delete your own messages
- Faculty can delete any message

---

**If you see the exact error message, please share it for more specific help!**
