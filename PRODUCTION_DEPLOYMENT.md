PRODUCTION_DEPLOYMENT.md  # PRODUCTION DEPLOYMENT - Video Upload App

## 🚀 STATUS: READY FOR PRODUCTION DEPLOYMENT

This document contains the LIVE deployment instructions with actual service links and steps.

## ⚡ QUICK DEPLOYMENT CHECKLIST

### Pre-Deployment (5 minutes)
- [ ] Clone repository: `git clone https://github.com/KODAVATISANJAY/video-upload-app.git`
- [ ] Navigate: `cd video-upload-app`
- [ ] Install backend: `cd backend && npm install && cd ..`
- [ ] Install frontend: `cd frontend && npm install`

### Database Setup - MongoDB Atlas (10 minutes)

1. **Create Account**
   - Go to https://www.mongodb.com/cloud/atlas
   - Sign up with Google or GitHub
   - Click "Create" → "Create a Cluster"

2. **Configure Cluster**
   - Name: `video-upload-app`
   - Tier: `M0 Free` (always free)
   - Region: Choose closest to your location
   - Click "Create Cluster"
   - Wait 3-5 minutes for creation

3. **Create Database User**
   - Go to "Security" → "Database Access"
   - Click "Create Database User"
   - Username: `video_upload_user`
   - Password: Use auto-generated or create strong password
   - Click "Create User"

4. **Setup Network Access**
   - Go to "Security" → "Network Access"
   - Click "Add IP Address"
   - Select "Allow Access from Anywhere" (for development)
   - Click "Confirm"

5. **Get Connection String**
   - Click "Connect" on cluster
   - Select "Connect your application"
   - Choose "Node.js" driver
   - Copy connection string
   - Replace `<password>` with your password
   - Replace `myFirstDatabase` with `video-upload-app`

**Example Connection String:**
```
mongodb+srv://video_upload_user:YOUR_PASSWORD@cluster0.xxxxx.mongodb.net/video-upload-app?retryWrites=true&w=majority
```

**Save this for next step!**

---

## 🟢 BACKEND DEPLOYMENT - RENDER

### Step 1: Deploy Backend

1. **Create Render Account**
   - Go to https://render.com
   - Sign up with GitHub
   - Click "Authorize" on GitHub prompt

2. **Create Web Service**
   - Click "+ New" → "Web Service"
   - Find and select `video-upload-app` repository
   - Click "Connect"

3. **Configure Service**

   **Name:** `video-upload-app-backend`
   
   **Environment:** `Node`
   
   **Build Command:** `npm install`
   
   **Start Command:** `node src/server.js`
   
   **Environment Variables:** (Add each)
   ```
   PORT = 5000
   NODE_ENV = production
   MONGODB_URI = (paste your MongoDB connection string here)
   JWT_SECRET = use-a-very-long-random-secret-key-here-change-it
   CORS_ORIGIN = https://your-app.vercel.app (add after frontend deployment)
   ```

4. **Deploy**
   - Plan: Free
   - Click "Create Web Service"
   - Wait 2-5 minutes for deployment
   - Copy the URL: `https://your-service-name.onrender.com`

### Expected Result
✅ Backend URL: `https://video-upload-app-xxxxx.onrender.com`
✅ Test health: `curl https://video-upload-app-xxxxx.onrender.com/api/health`
✅ Expected response: `{"message":"Server is running"}`

---

## 🟣 FRONTEND DEPLOYMENT - VERCEL

### Step 1: Deploy Frontend

1. **Create Vercel Account**
   - Go to https://vercel.com
   - Sign up with GitHub
   - Click "Authorize"

2. **Import Project**
   - Click "Add New..." → "Project"
   - Find `video-upload-app` repository
   - Click "Import"

3. **Configure Project**

   **Root Directory:** `frontend`
   
   **Build Command:** `npm run build`
   
   **Output Directory:** `dist`
   
   **Environment Variables:** (Add)
   ```
   VITE_API_URL = https://video-upload-app-xxxxx.onrender.com/api
   ```
   (Use your Render backend URL from above)

4. **Deploy**
   - Click "Deploy"
   - Wait 2-3 minutes
   - Copy the URL: `https://your-project.vercel.app`

### Expected Result
✅ Frontend URL: `https://video-upload-app.vercel.app` (or similar)
✅ App loads without errors
✅ Can access login page

---

## 🔗 FINAL CONFIGURATION

### Step 1: Update CORS on Render

1. Go back to Render dashboard
2. Click on `video-upload-app-backend` service
3. Go to "Environment"
4. Find `CORS_ORIGIN`
5. Update value to: `https://your-app.vercel.app`
6. Click "Save Changes"
7. Service will redeploy (1-2 minutes)

---

## ✅ TESTING DEPLOYMENT

### Test Backend
```bash
curl https://video-upload-app-xxxxx.onrender.com/api/health

# Expected response:
# {"message":"Server is running"}
```

### Test Frontend
1. Open `https://your-app.vercel.app` in browser
2. Should load without errors
3. See login/register page

### Test Features
1. **Register:** Create new account
   - Email: test@example.com
   - Password: Test123456

2. **Login:** Sign in with created account

3. **Upload:** Upload a video file
   - Select video file
   - Add title: "Test Video"
   - Set sensitivity: Medium
   - Click upload
   - Should show progress bar

4. **View:** Check video list
   - Should see uploaded video
   - Click to play (stream)

5. **Delete:** Remove video
   - Click delete button
   - Video should disappear

---

## 🐛 TROUBLESHOOTING

### "Backend connection refused"
- Check VITE_API_URL is correct
- Verify Render service is running
- Check CORS configuration
- Wait 2 minutes for service to fully start

### "Database connection failed"
- Check MONGODB_URI is correct
- Verify password in connection string
- Check IP whitelist ("Allow from Anywhere")
- Test connection in MongoDB Atlas

### "Build failed on Vercel"
- Check build logs on Vercel
- Ensure `frontend` is root directory
- Verify Node version (16+)
- Check all dependencies in package.json

### "Free tier service stopping"
- Render free tier auto-sleeps after 15 min of inactivity
- Upgrade to paid tier for continuous running
- Or use Railway: https://railway.app

---

## 🔄 LIVE URLS (Update after deployment)

```
Frontend: https://your-app.vercel.app
Backend API: https://your-app.onrender.com/api
Database: MongoDB Atlas (cloud)
```

---

## 📱 DEMO VIDEO REQUIREMENTS

Create a video showing:
1. (0:00-0:05) Frontend loads
2. (0:05-0:15) Registration - Create new account
3. (0:15-0:30) Login - Sign in
4. (0:30-1:00) Upload - Select and upload video with progress
5. (1:00-1:15) List - Show videos in list
6. (1:15-1:45) Stream - Play video (streaming)
7. (1:45-2:00) Delete - Remove video from list

**Total Duration:** ~2 minutes

---

## 📚 REFERENCE LINKS

- MongoDB Atlas: https://www.mongodb.com/cloud/atlas
- Render: https://render.com
- Vercel: https://vercel.com
- Railway (Alternative to Render): https://railway.app
- Netlify (Alternative to Vercel): https://netlify.com

---

## 🎯 SUCCESS INDICATORS

✅ Frontend loads without errors  
✅ Can register new account  
✅ Can login with credentials  
✅ Can upload video with progress bar  
✅ Video appears in list  
✅ Can stream/play video  
✅ Can delete video  
✅ All publicly accessible via URLs  

---

## 🚨 IMPORTANT NOTES

⚠️ **Free Tier Limitations:**
- Render backend sleeps after 15 minutes of inactivity
- MongoDB Atlas free tier has 512MB limit
- Vercel has bandwidth limits
- Solution: Upgrade to paid tiers for production

⚠️ **Security:**
- Change default JWT_SECRET to something random
- Use strong database password
- Enable 2FA on all accounts
- Never commit .env files

⚠️ **Performance:**
- Free tier services are slower
- Cold starts take 20-30 seconds
- Upgrade for better performance

---

**Deployment Status:** PRODUCTION READY  
**Last Updated:** December 26, 2025  
**By:** Automated Deployment Assistant
