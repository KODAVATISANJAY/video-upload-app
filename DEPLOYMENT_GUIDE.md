# Video Upload App - Deployment Guide

This guide provides step-by-step instructions to deploy the Video Upload App to production with a publicly accessible URL.

## Deployment Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    User Browser                              │
└────────────────────┬────────────────────────────────────────┘
                     │
           ┌─────────▼──────────┐
           │  Vercel (Frontend) │
           │  React + Vite      │
           │  https://...       │
           └─────────┬──────────┘
                     │
           ┌─────────▼──────────┐
           │  Render (Backend)  │
           │  Express API       │
           │  https://...       │
           └─────────┬──────────┘
                     │
           ┌─────────▼──────────┐
           │  MongoDB Atlas     │
           │  Cloud Database    │
           │  Secure Connection │
           └────────────────────┘
```

## Prerequisites

- GitHub account (already have)
- MongoDB Atlas account (free tier available)
- Render account (free tier available)
- Vercel account (free tier available)
- Credit card for free tier services (may be required for verification)

## Part 1: Set Up MongoDB Atlas

### Step 1: Create MongoDB Atlas Account

1. Go to https://www.mongodb.com/cloud/atlas
2. Click "Sign Up" or "Sign In"
3. Create a free account
4. Verify your email

### Step 2: Create a Cluster

1. Click "Create a new project" or use "My First Project"
2. Name it "video-upload-app"
3. Click "Create Project"
4. Click "Create a Database"
5. Select "M0 Free" tier (unlimited free tier)
6. Choose your region (closest to your location)
7. Click "Create Cluster"
8. Wait for cluster to be created (5-10 minutes)

### Step 3: Create Database User

1. In the cluster, go to "Security" → "Database Access"
2. Click "Add New Database User"
3. Create username: `video_upload_user`
4. Set password (save this!)
5. Grant "Atlas Admin" role
6. Click "Create User"

### Step 4: Configure Network Access

1. Go to "Security" → "Network Access"
2. Click "Add IP Address"
3. Select "Allow Access from Anywhere" (for development)
4. Click "Confirm"

### Step 5: Get Connection String

1. Click "Connect" on your cluster
2. Select "Connect your application"
3. Choose "Node.js" driver
4. Copy the connection string
5. Replace `<password>` with your database user password
6. Replace `myFirstDatabase` with `video-upload-app`

Example:
```
mongodb+srv://video_upload_user:password@cluster0.xxxxx.mongodb.net/video-upload-app?retryWrites=true&w=majority
```

Save this connection string for later!

## Part 2: Deploy Backend to Render

### Step 1: Push Code to GitHub

1. Make sure all your code is committed and pushed
2. The repository is already at: https://github.com/KODAVATISANJAY/video-upload-app

### Step 2: Create Render Account

1. Go to https://render.com
2. Click "Sign up" (you can use GitHub)
3. Connect your GitHub account
4. Authorize Render to access your repositories

### Step 3: Create Backend Service

1. On Render dashboard, click "+ New +"
2. Select "Web Service"
3. Search for "video-upload-app"
4. Select the repository
5. Connect it

### Step 4: Configure Backend Service

**Name:** video-upload-app-backend

**Environment:**
- Runtime: Node
- Build Command: `npm install`
- Start Command: `npm start`

**Environment Variables:**
Click "Add Environment Variables" and add:

```
MONGODB_URI = mongodb+srv://video_upload_user:YOUR_PASSWORD@cluster0.xxxxx.mongodb.net/video-upload-app?retryWrites=true&w=majority
JWT_SECRET = your_super_secret_jwt_key_change_this_in_production
PORT = 5000
NODE_ENV = production
CORS_ORIGIN = https://your-vercel-frontend-url.vercel.app
```

**Plan:** Free (for initial deployment)

5. Click "Create Web Service"
6. Wait for deployment to complete (2-5 minutes)
7. Copy the URL: `https://video-upload-app-backend.onrender.com`

### Step 5: Update Backend server.js

Update CORS configuration in `backend/src/server.js`:

```javascript
app.use(cors({
  origin: process.env.CORS_ORIGIN || 'http://localhost:3000',
  credentials: true
}));
```

Add this to your server.js and commit to GitHub.

## Part 3: Deploy Frontend to Vercel

### Step 1: Create Vercel Account

1. Go to https://vercel.com
2. Sign up (recommend using GitHub)
3. Connect your GitHub account

### Step 2: Create Frontend Project

1. Click "Add New..." → "Project"
2. Find "video-upload-app" repository
3. Import it

### Step 3: Configure Frontend

**Root Directory:** `frontend`

**Build Command:** `npm run build`

**Output Directory:** `dist`

**Environment Variables:**
```
VITE_API_URL = https://video-upload-app-backend.onrender.com/api
```

4. Click "Deploy"
5. Wait for deployment to complete
6. Copy the URL: `https://video-upload-app.vercel.app`

### Step 4: Update Render Backend CORS

1. Go back to Render dashboard
2. Click on your backend service
3. Go to "Settings"
4. Update `CORS_ORIGIN` environment variable:
   ```
   https://video-upload-app.vercel.app
   ```
5. Redeploy by triggering a new build

## Part 4: Testing Deployment

### Test Backend API

```bash
# Health check
curl https://video-upload-app-backend.onrender.com/api/health

# Should return:
# {"message":"Server is running"}
```

### Test Frontend

1. Open https://video-upload-app.vercel.app in browser
2. Should load without errors
3. Try registering a new account
4. Try uploading a video
5. Verify video appears in list
6. Test delete functionality

## Part 5: Production Checklist

- [ ] MongoDB Atlas cluster is running
- [ ] Database user created with strong password
- [ ] Backend deployed on Render
- [ ] Frontend deployed on Vercel
- [ ] Environment variables configured on both services
- [ ] CORS properly configured
- [ ] JWT secret is strong (not default)
- [ ] Database connection string is secure
- [ ] API health check passes
- [ ] Frontend loads without errors
- [ ] Registration/Login works
- [ ] Video upload works
- [ ] Video list displays correctly
- [ ] Video streaming works
- [ ] Delete functionality works

## Deployment URLs

Once deployed, your application will be available at:

- **Frontend:** https://your-vercel-domain.vercel.app
- **Backend API:** https://your-render-domain.onrender.com
- **Database:** MongoDB Atlas (secure)

## Troubleshooting

### Backend won't connect to MongoDB

1. Check MongoDB Atlas connection string
2. Verify IP whitelist includes "Allow from Anywhere"
3. Verify database user password is correct
4. Check environment variable is set correctly
5. Review Render logs: Settings → Logs

### Frontend can't connect to Backend

1. Check VITE_API_URL environment variable
2. Verify backend is running (check health endpoint)
3. Check CORS configuration on backend
4. Open browser DevTools → Network tab to see requests
5. Check browser console for errors

### Vercel build fails

1. Check build logs on Vercel dashboard
2. Ensure `frontend` is root directory
3. Verify all dependencies are in package.json
4. Check vite.config.ts is correct
5. Try rebuilding from Vercel dashboard

### Render deployment fails

1. Check deployment logs
2. Ensure backend folder has all required files
3. Verify package.json has correct start script
4. Check Node version compatibility
5. Ensure all environment variables are set

## Cost Considerations

- **MongoDB Atlas:** Free tier (512MB storage)
- **Render:** Free tier (limited compute)
- **Vercel:** Free tier (unlimited deployments)

For production with higher traffic:
- Upgrade MongoDB to paid tier
- Upgrade Render to paid tier
- Consider CDN for file storage (S3, etc.)

## Next Steps

1. Test all features thoroughly
2. Create a demo video (see DEMO_VIDEO_GUIDE.md)
3. Update README with deployed URLs
4. Monitor logs and performance
5. Set up error tracking (Sentry, etc.)
6. Consider adding analytics

## Documentation Links

- [MongoDB Atlas Docs](https://docs.atlas.mongodb.com/)
- [Render Deployment Docs](https://render.com/docs)
- [Vercel Deployment Docs](https://vercel.com/docs)
- [Express CORS Guide](https://expressjs.com/en/resources/middleware/cors.html)

## Support

For deployment issues:
1. Check respective platform documentation
2. Review application logs
3. Test locally first
4. Check GitHub issues for similar problems
5. Contact support for each platform

---

**Deployment Status:** Ready for production
**Last Updated:** December 26, 2025
