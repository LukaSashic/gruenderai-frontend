# GründerAI Frontend - Vercel Deployment Guide

## 📦 What's Included

This frontend provides the user interface for the GründerAI assessment:
- **index.html**: Professional landing page
- **assessment.html**: Interactive assessment interface
- **results.html**: Detailed results and recommendations

## 🚀 Deploy to Vercel (Recommended)

### Prerequisites
- GitHub account
- Vercel account (sign up at vercel.com)
- **Backend API URL from Railway** (complete backend deployment first!)

### Step 1: Update API Configuration

**CRITICAL:** Before deploying, update the API URL in `assessment.html`:

```javascript
// In assessment.html, find this line (around line 158):
const API_BASE_URL = 'YOUR_RAILWAY_API_URL';

// Replace with your actual Railway URL (NO trailing slash):
const API_BASE_URL = 'https://gruenderai-backend-production.up.railway.app';
```

### Step 2: Prepare Repository

```bash
# Create a new GitHub repository
# Repository name: gruenderai-frontend

# Clone locally
git clone https://github.com/YOUR_USERNAME/gruenderai-frontend.git
cd gruenderai-frontend

# Copy all frontend files to this directory:
# - index.html
# - assessment.html
# - results.html

# Initialize git and push
git add .
git commit -m "Initial frontend setup"
git push origin main
```

### Step 3: Deploy on Vercel

#### Option A: Deploy via Vercel Website (Easiest)

1. Go to https://vercel.com
2. Click "Add New..." → "Project"
3. Import your `gruenderai-frontend` repository
4. Configure:
   - Framework Preset: **Other** (static site)
   - Root Directory: **./` (leave as default)
   - Build Command: (leave empty)
   - Output Directory: **`./`** (leave as default)
5. Click "Deploy"

#### Option B: Deploy via Vercel CLI

```bash
# Install Vercel CLI
npm i -g vercel

# Login
vercel login

# Deploy
cd gruenderai-frontend
vercel

# Follow prompts:
# - Set up and deploy? Yes
# - Which scope? (Your account)
# - Link to existing project? No
# - Project name: gruenderai-frontend
# - Directory: ./
# - Override settings? No

# For production deployment:
vercel --prod
```

### Step 4: Get Your URL

Vercel will provide URLs like:
- Preview: `https://gruenderai-frontend-xyz123.vercel.app`
- Production: `https://gruenderai-frontend.vercel.app`

### Step 5: Add Custom Domain (Optional)

In Vercel dashboard:
1. Go to your project → Settings → Domains
2. Add your domain (e.g., `gruenderai.com`)
3. Follow DNS configuration instructions
4. Vercel handles SSL automatically

### Step 6: Update CORS in Backend

After deployment, update your backend's CORS settings in `main.py`:

```python
allow_origins=[
    "http://localhost:3000",
    "http://localhost:5173",
    "https://*.vercel.app",
    "https://gruenderai.com",           # Your custom domain
    "https://www.gruenderai.com",       # www version
    "https://gruenderai-frontend.vercel.app"  # Your Vercel domain
]
```

Commit and push this change - Railway will auto-deploy!

## ✅ Testing Your Deployment

### 1. Landing Page
Visit: `https://your-domain.vercel.app`
- Should see landing page with hero section
- "Assessment starten" button should work

### 2. Assessment Flow
Click "Assessment starten":
- Should load first question
- Progress bar should update
- Options should be selectable
- Can complete all 15 questions

### 3. Results Page
After completing assessment:
- Should show overall score
- Should display dimension breakdown
- Should show recommendations and next steps

### 4. Check Console
Open browser console (F12) and verify:
- No CORS errors
- API calls succeed
- Session ID is stored

## 🐛 Troubleshooting

### "Failed to start assessment" Error
- **Cause**: API URL incorrect or backend not running
- **Fix**: Verify `API_BASE_URL` in assessment.html matches your Railway URL exactly
- **Test**: Visit `YOUR_RAILWAY_URL/health` - should return JSON

### CORS Error in Console
- **Cause**: Backend doesn't allow your frontend domain
- **Fix**: Update CORS `allow_origins` in backend main.py to include your Vercel domain
- **Wait**: Railway needs to redeploy (1-2 minutes)

### Questions Not Loading
- **Cause**: API endpoint issue
- **Fix**: Check browser console for specific error
- **Verify**: Test API directly with curl:
  ```bash
  curl -X POST YOUR_RAILWAY_URL/api/assessment/start \
    -H "Content-Type: application/json" \
    -d '{"language":"de"}'
  ```

### Results Page Shows "No Results"
- **Cause**: sessionStorage not persisting
- **Fix**: Ensure you're on same domain between assessment and results
- **Check**: Browser console → Application → Session Storage

## 🔄 Updates and Redeployment

Vercel automatically redeploys on git push:

```bash
# Make changes to HTML files
git add .
git commit -m "Update landing page copy"
git push origin main

# Vercel auto-deploys in ~30 seconds!
```

For immediate preview before production:
```bash
vercel  # Deploy to preview URL first
# Test thoroughly
vercel --prod  # Then deploy to production
```

## 🎨 Customization

### Update Branding
- Change logo text in headers (🚀 GründerAI)
- Update color scheme in CSS `:root` variables
- Modify hero text and CTAs

### Update Copy
- Landing page: Edit `index.html` hero and features sections
- Assessment: Modify questions in backend `questions.py`
- Results: Customize recommendations in backend `engine.py`

### Add Analytics
Insert before closing `</head>` tag:

```html
<!-- Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=YOUR_GA_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'YOUR_GA_ID');
</script>
```

## 📊 Monitoring

Vercel provides:
- Automatic deployment logs
- Analytics (with Pro plan)
- Real-time error tracking
- Performance metrics

Access in Vercel dashboard.

## 💰 Pricing

Vercel offers:
- **Free tier**: Perfect for this project
  - 100 GB bandwidth
  - Unlimited deployments
  - Automatic SSL
- **Pro**: $20/month (only if you need more bandwidth)

## ✅ Deployment Checklist

- [ ] Backend deployed to Railway
- [ ] Copied Railway API URL
- [ ] Updated `API_BASE_URL` in assessment.html
- [ ] Created GitHub repository
- [ ] Pushed frontend files
- [ ] Deployed to Vercel
- [ ] Tested landing page
- [ ] Completed full assessment flow
- [ ] Verified results display correctly
- [ ] Updated backend CORS with Vercel domain
- [ ] (Optional) Added custom domain

## 🎯 Production Readiness

Before going live:
- [ ] Test on mobile devices
- [ ] Verify all links work
- [ ] Check spelling/grammar
- [ ] Add privacy policy link
- [ ] Add contact information
- [ ] Set up analytics
- [ ] Test with real users (5-10 people)

## 🆘 Support

Common issues and solutions:
1. **White screen**: Check browser console for errors
2. **CORS**: Update backend allow_origins
3. **Slow loading**: Check Railway backend is running
4. **Broken layout**: Clear browser cache

---

**🎉 Congratulations!** Your GründerAI assessment platform is now live!

**Next Steps:**
1. Share with test users
2. Collect feedback
3. Iterate based on results
4. Plan business plan generation phase
