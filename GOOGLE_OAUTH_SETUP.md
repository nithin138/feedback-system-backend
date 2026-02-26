# 🔐 Google OAuth Setup Guide

## ✅ Configuration Complete!

Your Google OAuth credentials have been configured:
- **Client ID**: `YOUR_GOOGLE_CLIENT_ID`
- **Client Secret**: `YOUR_GOOGLE_CLIENT_SECRET`

---

## 🚀 Quick Start

### 1. Configure Google Cloud Console

You need to add authorized redirect URIs in your Google Cloud Console:

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Select your project
3. Navigate to **APIs & Services** → **Credentials**
4. Click on your OAuth 2.0 Client ID
5. Add these **Authorized redirect URIs**:
   ```
   http://localhost:5000/api/auth/google/callback
   http://localhost:3000/auth/callback
   ```
6. Add these **Authorized JavaScript origins**:
   ```
   http://localhost:5000
   http://localhost:3000
   ```
7. Click **Save**

### 2. Start the Application

**Backend**:
```bash
cd backend
npm run dev
```

**Frontend**:
```bash
cd frontend
npm start
```

### 3. Test Google Sign-In

1. Go to http://localhost:3000/login
2. Click "Continue with Google"
3. Select your Google account
4. Grant permissions
5. You'll be redirected back and logged in!

---

## 📋 How It Works

### Authentication Flow

1. **User clicks "Continue with Google"**
   - Frontend redirects to: `http://localhost:5000/api/auth/google`

2. **Backend initiates OAuth**
   - Redirects to Google's OAuth consent screen
   - User selects account and grants permissions

3. **Google redirects back**
   - Redirects to: `http://localhost:5000/api/auth/google/callback`
   - Backend receives authorization code

4. **Backend processes authentication**
   - Exchanges code for user profile
   - Creates or finds user in database
   - Generates JWT token

5. **Backend redirects to frontend**
   - Redirects to: `http://localhost:3000/auth/callback?token=JWT_TOKEN`
   - Frontend stores token and user data
   - Redirects to appropriate page (feed or pending approval)

---

## 🔧 Configuration Files

### Backend (.env)
```env
GOOGLE_CLIENT_ID=YOUR_GOOGLE_CLIENT_ID
GOOGLE_CLIENT_SECRET=YOUR_GOOGLE_CLIENT_SECRET
GOOGLE_CALLBACK_URL=http://localhost:5000/api/auth/google/callback
FRONTEND_URL=http://localhost:3000
```

### Frontend (.env)
```env
REACT_APP_API_URL=http://localhost:5000
REACT_APP_GOOGLE_CLIENT_ID=YOUR_GOOGLE_CLIENT_ID
```

---

## 🎯 Features Implemented

### Login Page
- ✅ "Continue with Google" button
- ✅ Redirects to Google OAuth
- ✅ Handles authentication flow

### Register Page
- ✅ "Continue with Google" button
- ✅ Creates new user with Google account
- ✅ Default role: Student

### OAuth Callback Page
- ✅ Receives token from backend
- ✅ Stores token and user data
- ✅ Redirects based on user role
- ✅ Error handling

### Backend
- ✅ Google OAuth strategy configured
- ✅ User creation/linking
- ✅ JWT token generation
- ✅ Redirect to frontend with token

---

## 🔒 Security Features

1. **Secure Token Handling**
   - JWT tokens generated server-side
   - Tokens passed via URL parameter (one-time use)
   - Stored in localStorage after validation

2. **User Linking**
   - If email exists, links Google account
   - Prevents duplicate accounts
   - Maintains user data integrity

3. **Role Management**
   - Google users default to "student" role
   - Can be changed by admin if needed
   - Faculty still require approval

---

## 🧪 Testing Google OAuth

### Test Scenario 1: New User Registration
1. Click "Continue with Google" on login/register page
2. Select a Google account not in the system
3. Grant permissions
4. **Expected**: New user created as student, redirected to feed

### Test Scenario 2: Existing User Login
1. Register with email/password first
2. Logout
3. Click "Continue with Google" with same email
4. **Expected**: Google account linked, logged in successfully

### Test Scenario 3: OAuth User Login
1. Register with Google
2. Logout
3. Try to login with email/password
4. **Expected**: Error message: "This account uses Google sign-in"

---

## 🐛 Troubleshooting

### Error: "redirect_uri_mismatch"

**Problem**: The redirect URI is not authorized in Google Cloud Console

**Solution**:
1. Go to Google Cloud Console
2. Add `http://localhost:5000/api/auth/google/callback` to Authorized redirect URIs
3. Save and wait a few minutes for changes to propagate

### Error: "Access blocked: This app's request is invalid"

**Problem**: JavaScript origins not configured

**Solution**:
1. Add `http://localhost:5000` and `http://localhost:3000` to Authorized JavaScript origins
2. Save changes

### Error: "OAuth not configured"

**Problem**: Environment variables not loaded

**Solution**:
1. Check backend/.env has correct GOOGLE_CLIENT_ID and GOOGLE_CLIENT_SECRET
2. Restart backend server
3. Check backend logs for "✅ Google OAuth configured"

### User stuck on callback page

**Problem**: Token not being processed correctly

**Solution**:
1. Check browser console for errors
2. Verify backend is running
3. Check network tab for API calls
4. Clear localStorage and try again

---

## 📱 Production Deployment

### Update Redirect URIs

When deploying to production, update these in Google Cloud Console:

**Authorized redirect URIs**:
```
https://yourdomain.com/api/auth/google/callback
https://yourfrontend.com/auth/callback
```

**Authorized JavaScript origins**:
```
https://yourdomain.com
https://yourfrontend.com
```

### Update Environment Variables

**Backend**:
```env
GOOGLE_CALLBACK_URL=https://yourdomain.com/api/auth/google/callback
FRONTEND_URL=https://yourfrontend.com
```

**Frontend**:
```env
REACT_APP_API_URL=https://yourdomain.com
```

---

## 🎓 User Experience

### For Students
1. Click "Continue with Google"
2. Select Google account
3. Immediately access the feed
4. Post anonymously

### For Faculty
1. Click "Continue with Google"
2. Select Google account
3. Redirected to "Pending Approval" page
4. Wait for admin approval
5. Access system after approval

### For Existing Users
1. Google account automatically linked to existing email
2. Can use either Google or email/password to login
3. Seamless experience

---

## 📊 Database Impact

### New User via Google
```javascript
{
  email: "user@gmail.com",
  name: "John Doe",
  role: "student",
  oauthProvider: "google",
  oauthId: "google-user-id",
  password: null,
  anonymousId: "AS_12345", // for students
  approvalStatus: "approved" // or "pending" for faculty
}
```

### Linked Account
```javascript
{
  email: "user@gmail.com",
  name: "John Doe",
  role: "student",
  oauthProvider: "google", // updated
  oauthId: "google-user-id", // added
  password: "hashed-password", // kept
  // ... other fields
}
```

---

## ✅ Checklist

Before testing:
- [ ] Google Cloud Console configured
- [ ] Redirect URIs added
- [ ] JavaScript origins added
- [ ] Backend .env updated
- [ ] Frontend .env updated
- [ ] Backend server running
- [ ] Frontend server running
- [ ] MongoDB running

---

## 🎉 Success!

Your Google OAuth integration is complete and ready to use!

**Test it now**:
1. Go to http://localhost:3000/login
2. Click "Continue with Google"
3. Sign in with your Google account
4. Start using the app!

---

## 📞 Support

If you encounter issues:
1. Check browser console for errors
2. Check backend logs
3. Verify Google Cloud Console settings
4. Review TROUBLESHOOTING.md
5. Check network tab for failed requests

---

**Note**: Make sure to keep your Client Secret secure and never commit it to public repositories!
