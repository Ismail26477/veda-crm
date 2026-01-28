# Quick Deployment to Vercel (5 Minutes)

## TL;DR - Just Do This:

### 1. Get MongoDB Connection String
- Go to https://mongodb.com/cloud/atlas
- Create a free cluster or use existing one
- Get connection string: `mongodb+srv://username:password@cluster.mongodb.net/veda-crm?retryWrites=true&w=majority`

### 2. Push to GitHub
```bash
git add .
git commit -m "Ready for Vercel deployment"
git push origin main
```

### 3. Deploy to Vercel
- Go to https://vercel.com/new
- Import your GitHub repository
- Click "Import"

### 4. Add Environment Variable
- Go to **Settings → Environment Variables**
- Add `MONGODB_URI` with your connection string
- Click Save & Redeploy

### 5. Done! 🎉
Wait for build to complete, then visit your deployed URL

---

## If Something Goes Wrong

**Q: Build failed?**
A: Check logs in Vercel Dashboard → Deployments → Click failed deployment

**Q: API returns 404?**
A: Add `MONGODB_URI` environment variable in Vercel Settings

**Q: Frontend loads but no data?**
A: Make sure MongoDB whitelist includes 0.0.0.0/0 (all IPs)

**Q: Still stuck?**
A: Read the full guide in `/DEPLOYMENT.md`

---

## Login Credentials

After deployment works:
- Email: `admin@gmail.com`
- Password: `admin123`

---

That's it! Your CRM is now live. 🚀
