# 🚀 Deploy SDS to Render

## Prerequisites
- GitHub account with your code pushed
- Render account (free): [render.com](https://render.com)

## Deployment Steps

### **Step 1: Push to GitHub**
```bash
cd v:\sds
git add .
git commit -m "Add Render configuration"
git push origin main
```

### **Step 2: Create Render Account**
1. Go to [render.com](https://render.com)
2. Sign up with GitHub (recommended)
3. Authorize Render to access your repositories

### **Step 3: Create New Web Service**
1. Click **"New +"** → **"Web Service"**
2. Connect your GitHub repository: `vishnudeepak213/sds`
3. Configure the service:

**Settings:**
- **Name:** `sds-crowd-analysis` (or your choice)
- **Region:** Oregon (Free tier available)
- **Branch:** `main`
- **Runtime:** Python 3
- **Build Command:** 
  ```
  pip install -r requirements.txt
  ```
- **Start Command:**
  ```
  streamlit run streamlit_app.py --server.port=$PORT --server.address=0.0.0.0 --server.headless=true
  ```
- **Plan:** Free

### **Step 4: Environment Variables** (Optional)
Click **"Environment"** → Add:
- `PYTHON_VERSION` = `3.9.18`
- `STREAMLIT_SERVER_HEADLESS` = `true`

### **Step 5: Deploy**
1. Click **"Create Web Service"**
2. Wait 5-10 minutes for build to complete
3. Your app will be live at: `https://sds-crowd-analysis.onrender.com`

---

## Auto-Deploy Setup

✅ **Automatic deployments enabled by default!**

Every push to your `main` branch will automatically deploy to Render.

---

## System Packages (If Needed)

If you need system packages (like from `packages.txt`), add to your `render.yaml`:

```yaml
services:
  - type: web
    name: sds-crowd-analysis
    env: python
    buildCommand: |
      apt-get update
      apt-get install -y libgl1-mesa-glx libglib2.0-0
      pip install -r requirements.txt
```

---

## Troubleshooting

### Build Fails
- Check build logs in Render dashboard
- Ensure `requirements.txt` is correct
- May need to reduce dependencies for free tier

### App Times Out
- Free tier sleeps after 15 min inactivity
- First request may take 30-60 seconds to wake up
- Upgrade to paid tier for always-on

### Out of Memory
- Free tier: 512 MB RAM
- Reduce model size: use `yolov8n.pt` instead of `yolov8s.pt`
- Disable augment/TTA in config.yaml

---

## Render vs Other Platforms

| Feature | Render | Hugging Face | Streamlit Cloud |
|---------|--------|--------------|-----------------|
| Free Tier | ✅ 750 hrs/mo | ✅ Limited | ✅ Limited |
| Custom Domain | ✅ | ❌ | ❌ |
| Sleep After Idle | ✅ 15 min | ❌ | ✅ |
| Build Time | ~5-10 min | ~3-5 min | ~2-3 min |
| RAM | 512 MB | 16 GB | 1 GB |
| Storage | Ephemeral | Persistent | Ephemeral |

---

## Cost Optimization Tips

**Free Tier Limits:**
- 750 hours/month free
- Sleeps after 15 minutes of inactivity
- 512 MB RAM

**To Stay Free:**
- Use `yolov8n.pt` (smallest model)
- Set `augment: false` in config
- Reduce `max_frames` in slider
- Disable TTA features

**Upgrade to Starter ($7/mo):**
- Always on (no sleep)
- 1 GB RAM
- Better for production use

---

## Files Created for Render

✅ `render.yaml` - Service configuration
✅ `Procfile` - Start command
✅ `.streamlit/config.toml` - Streamlit settings

---

## Support

If deployment fails:
- Check [Render Docs](https://render.com/docs)
- View logs in Render Dashboard
- Contact: support@render.com
