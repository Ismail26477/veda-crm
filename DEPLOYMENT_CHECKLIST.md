# Veda CRM - Deployment Checklist

## Phase 1: Pre-Deployment Preparation ⚙️

### Local Setup
- [ ] Node.js installed (v16 or higher)
- [ ] Git installed and configured
- [ ] Repository cloned or initialized
- [ ] All files committed to Git

### Dependencies
- [ ] `npm install` completed successfully
- [ ] No dependency errors in output
- [ ] `npm run build` runs without errors locally
- [ ] `npm run dev` works locally

### Environment Setup
- [ ] `.env.example` exists in root
- [ ] `.env.local` created locally (copy from .env.example)
- [ ] Local `.env.local` has test MongoDB URI
- [ ] Tested locally: `npm run dev`
- [ ] Dashboard loads with sample data locally

### MongoDB Setup
- [ ] MongoDB Atlas account created
- [ ] Free cluster created
- [ ] Database user created (username + strong password)
- [ ] IP whitelist includes 0.0.0.0/0 (or your IP)
- [ ] Connection string copied: `mongodb+srv://...`
- [ ] Tested: Can connect from local machine

### Code Quality
- [ ] No console errors in local dev
- [ ] API routes working locally
- [ ] Database queries working
- [ ] No sensitive keys in code (only in .env)
- [ ] Git history clean (all changes committed)

---

## Phase 2: GitHub & Vercel Setup 📦

### GitHub
- [ ] Repository pushed to GitHub
- [ ] Latest code on `main` branch
- [ ] No uncommitted changes
- [ ] Branch is set to `main` (or your default)

### Vercel Account
- [ ] Vercel account created at vercel.com
- [ ] GitHub connected to Vercel
- [ ] Can see your GitHub repositories

### Import to Vercel
- [ ] New project created in Vercel
- [ ] Repository imported: `Ismail26477/veda-crm`
- [ ] Framework detected as "Vite"
- [ ] Build command: `npm run build`
- [ ] Output directory: `dist`

---

## Phase 3: Environment Variables on Vercel 🔐

### Required Variables
- [ ] `MONGODB_URI` = your production MongoDB connection string
- [ ] `NODE_ENV` = `production`
- [ ] `VITE_API_URL` = your Vercel domain (get this after first deploy)
- [ ] `JWT_SECRET` = random 32+ character string (use crypto for generation)

### Optional Variables (if using these features)
- [ ] `EMAIL_USER` = your email (optional)
- [ ] `EMAIL_PASSWORD` = your email app password (optional)
- [ ] Other API keys if needed (optional)

### Variable Format Check
- [ ] No extra spaces in variable names
- [ ] No quotes around values
- [ ] Connection strings are complete
- [ ] Sensitive values never in .env.example (only in actual Vercel settings)

---

## Phase 4: Configuration Files ✅

### vercel.json
- [ ] File exists in project root
- [ ] Contains:
  - `buildCommand`: `npm run build`
  - `outputDirectory`: `dist`
  - `rewrites` for API and SPA
  - `functions` configuration

### api/server.js
- [ ] File exists at `/api/server.js`
- [ ] Has `export default app`
- [ ] Loads environment variables
- [ ] All routes imported:
  - auth, leads, callers, activities
  - call-logs, dashboard, reports, settings
  - followups, brokers, integrations, meetings

### vite.config.ts
- [ ] Correct output directory: `dist`
- [ ] Has build configuration
- [ ] Alias for `@` path is correct

### package.json
- [ ] `build` script exists: `vite build`
- [ ] All dependencies listed
- [ ] No typos in script names
- [ ] `type: "module"` for ES modules

---

## Phase 5: Deploy! 🚀

### Initial Deployment
- [ ] Go to https://vercel.com/new
- [ ] Click "Import Git Repository"
- [ ] Select your repository
- [ ] Click "Import"
- [ ] Wait for automatic configuration

### Add Environment Variables
- [ ] Navigate to Settings → Environment Variables
- [ ] Add all required variables from Phase 3
- [ ] Click "Save"
- [ ] Click "Redeploy" or wait for automatic redeploy

### Monitor Build
- [ ] Watch build progress in Deployments tab
- [ ] Check for any error messages
- [ ] Build should complete successfully
- [ ] Preview URL should be available

---

## Phase 6: Post-Deployment Testing 🧪

### Verify Frontend
- [ ] Click deployed URL
- [ ] Page loads (no blank screen)
- [ ] No console errors (F12 → Console)
- [ ] CSS and styling looks correct
- [ ] All images load

### Verify Login
- [ ] Login page appears
- [ ] Can enter credentials
- [ ] Submit button works
- [ ] Error messages show for invalid login
- [ ] Can log in with admin@gmail.com / admin123

### Verify Dashboard
- [ ] Dashboard page loads
- [ ] Data appears (no "loading..." forever)
- [ ] All widgets show data
- [ ] Charts render correctly
- [ ] Navigation works

### Verify API
- [ ] Open DevTools (F12 → Network)
- [ ] Navigate to different pages
- [ ] API calls to `/api/*` complete successfully
- [ ] No 404 errors
- [ ] No CORS errors
- [ ] Response times are reasonable (< 2 seconds)

### Verify Database
- [ ] Data loads from MongoDB
- [ ] Can see leads, callers, activities
- [ ] Numbers match expected data
- [ ] No "failed to fetch" messages

---

## Phase 7: Monitoring & Maintenance 📊

### Monitor Deployments
- [ ] Bookmark your Vercel project dashboard
- [ ] Check deployment logs periodically
- [ ] Monitor build duration
- [ ] Watch for failed deployments

### Monitor Logs
- [ ] Check Vercel Function logs for API errors
- [ ] Monitor error rates
- [ ] Look for database connection issues

### Updates & Changes
- [ ] When you make code changes, commit and push
- [ ] Vercel automatically redeploys
- [ ] Monitor new deployments for issues
- [ ] Rollback if issues occur

### Scaling (Future)
- [ ] Monitor function duration
- [ ] If slow, increase serverless function memory (in vercel.json)
- [ ] Consider database optimization if queries are slow
- [ ] Add caching if needed

---

## Phase 8: Rollback & Troubleshooting 🔧

### If Deployment Fails

**Build Error?**
- [ ] Check Vercel build logs
- [ ] Run `npm run build` locally to reproduce
- [ ] Fix issues locally
- [ ] Commit and push

**API Not Working?**
- [ ] Verify MONGODB_URI is correct in Vercel
- [ ] Check MongoDB Atlas is accepting connections
- [ ] Verify IP whitelist includes Vercel IPs (0.0.0.0/0)
- [ ] Check api/server.js is properly configured

**Frontend Issues?**
- [ ] Check console errors
- [ ] Verify vite.config.ts output directory
- [ ] Ensure all assets are in `/public` or imported

### Rollback
- [ ] Go to Vercel Deployments
- [ ] Find last working deployment
- [ ] Click three-dot menu
- [ ] Select "Promote to Production"

---

## Success Criteria ✅

Your deployment is successful when:

- ✅ Frontend loads without errors
- ✅ Login page appears
- ✅ Can authenticate with admin@gmail.com / admin123
- ✅ Dashboard displays data from MongoDB
- ✅ All navigation works
- ✅ No console errors (F12)
- ✅ API calls return correct data (Network tab)
- ✅ Database queries work properly
- ✅ Mobile view is responsive

---

## Quick Reference

### Important URLs
- Vercel Dashboard: https://vercel.com/dashboard
- MongoDB Atlas: https://cloud.mongodb.com
- Your Project GitHub: https://github.com/Ismail26477/veda-crm
- Deployed App: https://your-project.vercel.app (after deployment)

### Essential Commands
```bash
# Test locally before deployment
npm install
npm run build
npm run dev

# Check build for issues
npm run lint

# Push changes
git add .
git commit -m "Your message"
git push origin main
```

### Environment Variables Reminder
Never commit these to Git:
- MONGODB_URI (contains password)
- JWT_SECRET
- EMAIL_PASSWORD
- API_KEYS

Always use `.env.local` locally and Vercel Settings for production.

---

## Final Notes

1. **First deploy takes longer** - Cold start is normal for serverless
2. **Redeploys are faster** - Vercel caches dependencies
3. **Monitor logs initially** - Check for any issues in first 24 hours
4. **Test thoroughly** - Better to catch issues before going public
5. **Keep backups** - MongoDB Atlas has automatic backups

---

**When ready to deploy, start with `/QUICK_DEPLOY.md` for the 5-minute version!**

Good luck! 🚀
