# Veda CRM - Vercel Deployment Guide

## Prerequisites
✅ Vercel account (https://vercel.com)
✅ GitHub account with repository
✅ MongoDB Atlas account (for production database)

## Pre-Deployment Checklist

Before deploying, ensure:
- [ ] All code is committed to GitHub
- [ ] `package.json` has all dependencies
- [ ] `.env.example` is filled out with your actual values
- [ ] MongoDB URI is ready (from MongoDB Atlas)
- [ ] Vercel CLI installed (optional but recommended)

## Deployment Steps

### Step 1: Set Up MongoDB Atlas (if not already done)

1. Go to https://www.mongodb.com/cloud/atlas
2. Create a free cluster
3. Create a database user with a strong password
4. Add your IP to the whitelist (or use 0.0.0.0/0 for development)
5. Copy your connection string: `mongodb+srv://username:password@cluster.mongodb.net/veda-crm?retryWrites=true&w=majority`

### Step 2: Deploy via GitHub (Recommended)

**Option A: First-time deployment**

1. Go to https://vercel.com/new
2. Click "Import Git Repository"
3. Search for your `Ismail26477/veda-crm` repository
4. Click "Import"
5. Configure the project:
   - **Framework Preset**: Other (it will auto-detect Vite)
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`
   - **Install Command**: `npm install`

**Option B: Already have Vercel project**

1. Just push to GitHub on the `main` branch
2. Vercel automatically redeploys

### Step 3: Add Environment Variables

In your Vercel project dashboard:

1. Go to **Settings → Environment Variables**
2. Add these variables:

```
MONGODB_URI = mongodb+srv://username:password@cluster.mongodb.net/veda-crm?retryWrites=true&w=majority
NODE_ENV = production
VITE_API_URL = https://your-project-name.vercel.app
JWT_SECRET = (generate a random 32+ character string)
```

**For optional features:**
- `EMAIL_USER` = Your Gmail address
- `EMAIL_PASSWORD` = Your Gmail app password
- Other API keys as needed

### Step 4: Verify Deployment

After Vercel shows "Ready", check:

1. Visit your deployed URL: `https://your-project.vercel.app`
2. Open browser DevTools → Console
3. Check for any error messages
4. Try logging in with your credentials
5. Verify dashboard loads and fetches data

## Using Vercel CLI (Optional Alternative)

```bash
# Install Vercel CLI
npm install -g vercel

# Login
vercel login

# Deploy from project root
vercel --prod

# Pull environment variables
vercel env pull
```

## Project Architecture

- **Frontend**: Vite + React + TypeScript (builds to `/dist`)
- **Backend**: Express.js API (runs as serverless functions from `/api/server.js`)
- **Database**: MongoDB (cloud-hosted via Atlas)

## API Endpoints

All routes are prefixed with `/api/`:
- `/api/auth` - Authentication & login
- `/api/leads` - Lead management
- `/api/callers` - Caller management
- `/api/activities` - Activity tracking
- `/api/call-logs` - Call logging
- `/api/dashboard` - Dashboard data
- `/api/followups` - Follow-up management
- `/api/reports` - Reports & analytics
- `/api/settings` - User settings
- `/api/meetings` - Meeting management
- `/api/brokers` - Broker management
- `/api/integrations` - Integrations

## Default Login

Once deployed:
- **Email**: admin@gmail.com
- **Password**: admin123

## Post-Deployment Tasks

### Add Sample Data
```bash
# Run locally (not on Vercel)
npm run seed
```

### Monitor Deployment
1. Go to Vercel Dashboard
2. View Deployments tab
3. Check build logs if issues occur
4. View Function logs for API errors

## Troubleshooting

### ❌ Build Fails: "Cannot find module"
**Solution**: 
- Run `npm install` locally
- Ensure all imports have correct relative paths
- Check that all dependencies are in `package.json`

### ❌ API returns 404
**Solution**:
- Verify `vercel.json` exists and is correct
- Check that `/api/server.js` is properly configured
- Ensure MongoDB connection string is in environment variables
- Check Vercel Function logs

### ❌ CORS Errors
**Solution**:
- Update `server/index.js` CORS origin to include your Vercel domain
- Example: `https://your-project.vercel.app`

### ❌ MongoDB Connection Fails
**Solution**:
- Verify MONGODB_URI is correct in Vercel environment variables
- Check MongoDB Atlas whitelist includes 0.0.0.0/0 or Vercel IPs
- Test connection string locally first

### ❌ Frontend doesn't load data
**Solution**:
- Open browser Console (F12)
- Check if API calls show errors
- Verify `VITE_API_URL` environment variable matches your deployed domain
- Check that backend environment variables are set

## Rollback to Previous Version

1. Go to Vercel Dashboard → Deployments
2. Find previous working deployment
3. Click the deployment
4. Click "Promote to Production"

## Performance Tips

- Frontend assets are cached for 1 year
- HTML is cached for 1 hour (can change in `vercel.json`)
- Serverless functions timeout after 30 seconds
- Cold starts may take 1-2 seconds

## Need Help?

- Check Vercel logs: Dashboard → Deployments → Click deployment
- Read Function logs: Dashboard → Functions → Select function
- Vercel Docs: https://vercel.com/docs
- Support: https://vercel.com/support
\`\`\`

\`\`\`typescript file="" isHidden
