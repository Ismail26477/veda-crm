# Deployment Configuration Changes

## Files Modified for Vercel Deployment

### 1. **vercel.json** ✅
**What changed:**
- Removed deprecated `@vercel/static-build` builder
- Added `buildCommand` and `outputDirectory` for Vite
- Added `functions` configuration for serverless API
- Added cache headers for performance optimization
- Configured rewrites for SPA routing

**Key features:**
- API routes rewrite to `/api/server.js`
- SPA fallback to `index.html`
- 1-hour cache for HTML, 1-year for assets
- Serverless function memory: 1024MB
- Max duration: 30 seconds

---

### 2. **api/server.js** ✅
**What changed:**
- Fixed `.env` loading to use `.env.local`
- Removed unused imports
- Already has all routes properly configured:
  - `/api/auth`
  - `/api/leads`
  - `/api/callers`
  - `/api/activities`
  - `/api/call-logs`
  - `/api/dashboard`
  - `/api/reports`
  - `/api/settings`
  - `/api/followups`
  - `/api/brokers`
  - `/api/integrations`
  - `/api/meetings`

**What's working:**
- Express app properly configured for Vercel
- CORS enabled
- MongoDB connection handling
- Proper error handling

---

### 3. **vite.config.ts** ✅
**What changed:**
- Added minification using Terser
- Added rollup manual chunks for vendor splitting
- Optimized build output

**Benefits:**
- Smaller bundle size
- Better caching
- Improved performance

---

### 4. **.env.example** ✅ (NEW)
**Created with:**
- `MONGODB_URI` - MongoDB connection string
- `NODE_ENV` - Environment (development/production)
- `VITE_API_URL` - Frontend API endpoint
- `JWT_SECRET` - JWT signing secret
- Email configuration options
- Supabase configuration options (if needed)

**How to use:**
1. Copy to `.env.local` locally
2. Copy to Vercel Environment Variables
3. Update with your actual values

---

### 5. **package.json** ✅ (Already correct)
**Current setup:**
```json
{
  "scripts": {
    "build": "vite build",
    "dev": "concurrently \"npm run server\" \"npm run client\"",
    "client": "vite",
    "server": "nodemon server/index.js"
  }
}
```

**How Vercel uses it:**
- Runs `npm run build` to build Vite frontend
- Automatically detects Express in `/api` folder
- Deploys both frontend and API together

---

## Environment Variables to Add in Vercel

### Required
```
MONGODB_URI = mongodb+srv://user:pass@cluster.mongodb.net/veda-crm?retryWrites=true&w=majority
NODE_ENV = production
VITE_API_URL = https://your-project.vercel.app (your actual Vercel URL)
JWT_SECRET = (generate a random 32+ character string)
```

### Optional (for email features)
```
EMAIL_USER = your-email@gmail.com
EMAIL_PASSWORD = your-app-password
EMAIL_HOST = smtp.gmail.com
EMAIL_PORT = 587
```

---

## MongoDB Setup (if using MongoDB Atlas)

1. **Create Account**: https://mongodb.com/cloud/atlas
2. **Create Cluster**: 
   - Choose free tier
   - Select your region
   - Create cluster
3. **Create Database User**:
   - Username: `your-username`
   - Password: `your-strong-password`
4. **Get Connection String**:
   - Click "Connect" → "Drivers" → "Node.js"
   - Copy the connection string
   - Replace `<password>` with your password
   - Replace `myFirstDatabase` with `veda-crm`
5. **Whitelist IPs**:
   - Go to "Network Access"
   - Add `0.0.0.0/0` to allow all IPs (for development)
   - Or add Vercel's IP ranges

---

## Deployment Flow

```
┌─────────────────┐
│   Your GitHub   │
│   Repository    │
└────────┬────────┘
         │
         │ (git push)
         ▼
┌─────────────────┐
│  Vercel Detects │  ◄─── Reads vercel.json
│    Changes      │
└────────┬────────┘
         │
         ├─── npm install
         │
         ├─── npm run build
         │    └─── Vite builds frontend to /dist
         │
         ├─── Deploys /dist as static files
         │
         └─── Deploys /api/server.js as serverless function
                └─── Can call /api/auth, /api/leads, etc.
```

---

## Testing Before Deployment

### Local Testing (Recommended)
```bash
# Build locally
npm run build

# Test what Vercel will deploy
npm install -g vercel
vercel --prod --prebuilt
```

### Deployment Checklist
- [ ] All code committed to GitHub
- [ ] MONGODB_URI environment variable ready
- [ ] .env.example filled with correct values
- [ ] vite.config.ts has correct output directory
- [ ] vercel.json exists in root
- [ ] API routes are working locally
- [ ] Frontend builds without errors

---

## Common Issues & Fixes

| Issue | Cause | Fix |
|-------|-------|-----|
| Build fails | Dependencies missing | Run `npm install` |
| API 404 | Routes not found | Check vercel.json rewrites |
| CORS error | Domain not whitelisted | Add domain to CORS origin |
| No data loads | MongoDB connection fails | Verify MONGODB_URI and IP whitelist |
| Long cold start | First request slowness | Normal for serverless, caches after |

---

## Next Steps

1. **Copy .env.example to .env.local**
   ```bash
   cp .env.example .env.local
   ```

2. **Update with your values** (MongoDB URI, JWT secret, etc.)

3. **Test locally**
   ```bash
   npm install
   npm run dev
   ```

4. **Push to GitHub**
   ```bash
   git add .
   git commit -m "Ready for Vercel deployment"
   git push origin main
   ```

5. **Deploy to Vercel**
   - Go to https://vercel.com/new
   - Import your repository
   - Add environment variables
   - Click "Deploy"

6. **Monitor deployment**
   - Check Vercel Dashboard
   - View build logs if needed
   - Test your live URL

---

**For detailed instructions, read: `/DEPLOYMENT.md` and `/QUICK_DEPLOY.md`**
