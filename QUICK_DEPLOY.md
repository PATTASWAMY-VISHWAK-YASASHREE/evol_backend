# ⚡ Quick Deploy to Google Cloud Run

## 📋 Files Added to Your Repo

```
✅ Dockerfile           - Container image definition
✅ .dockerignore       - Excludes unnecessary files from build
✅ .gcloudignore       - Excludes files from GCP upload
✅ cloudbuild.yaml     - Automatic build/deploy config
✅ app.yaml            - Alternative App Engine config
✅ CLOUD_RUN_DEPLOY.md - Detailed deployment guide
✅ QUICK_DEPLOY.md     - This quick reference
```

## 🚀 Deploy in 3 Steps

### Step 1: Install Google Cloud SDK

Download from: https://cloud.google.com/sdk/docs/install

### Step 2: Login and Setup

```powershell
# Login to Google Cloud
gcloud auth login

# Create/select project
gcloud config set project YOUR_PROJECT_ID

# Enable required services
gcloud services enable cloudbuild.googleapis.com run.googleapis.com
```

### Step 3: Deploy

```powershell
# Navigate to your project
cd C:\Users\chinn\OneDrive\Desktop\scrape\reco

# Deploy to Cloud Run (all-in-one command)
gcloud run deploy jewelry-recommendation-api \
  --source . \
  --region us-central1 \
  --allow-unauthenticated \
  --memory 1Gi \
  --port 8080

# Get your URL
gcloud run services describe jewelry-recommendation-api \
  --region us-central1 \
  --format 'value(status.url)'
```

## 🌐 GitHub Integration (Auto-Deploy on Push)

### One-Time Setup:

1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Navigate to **Cloud Run** → **Create Service**
3. Choose **"Continuously deploy from a repository"**
4. Click **"Set up with Cloud Build"**
5. Connect **GitHub**
6. Select repo: `vara-prasad-07/evol_backend`
7. Branch: `Atm-interface`
8. Build type: **Dockerfile**
9. Dockerfile path: `/reco/Dockerfile` (if code is in reco folder)
10. Click **Create**

### Now Every Git Push Auto-Deploys! 🎉

```powershell
git add .
git commit -m "Update code"
git push origin Atm-interface
# Cloud Build automatically builds and deploys!
```

## 📝 What Gets Deployed

✅ **All necessary files**:
- `main.py` (FastAPI server)
- `recommender_engine.py`
- `style_taxonomy.py`
- `celebrities.json`, `products.json`
- **All embeddings**: `*.pkl`, `*_with_vectors.json`
- `index.html` (frontend)

❌ **Excluded** (via .dockerignore):
- Test files
- Documentation
- Git history
- Python cache

## 🔧 Update Frontend URL

After deployment, update `index.html`:

```javascript
// Find this line:
const API_URL = 'http://localhost:8000';

// Replace with your Cloud Run URL:
const API_URL = 'https://jewelry-recommendation-api-xxx.run.app';
```

Then commit and push:
```powershell
git add index.html
git commit -m "Update API URL for production"
git push origin Atm-interface
```

## 💰 Free Tier

Cloud Run free tier includes:
- **2 million requests/month**
- **360,000 vCPU-seconds**
- **180,000 GiB-seconds memory**

Your app will likely stay within free tier for moderate usage!

## 🧪 Test Your Deployment

```powershell
# Get your URL
$URL = gcloud run services describe jewelry-recommendation-api --region us-central1 --format 'value(status.url)'

# Test health endpoint
curl "$URL/health"

# Open API docs in browser
start "$URL/docs"

# Test frontend (if serving from same URL)
start "$URL/"
```

## ⚠️ Important Notes

1. **Keep embeddings**: Don't delete `.pkl` files - they make startup 10x faster
2. **Port 8080**: Cloud Run requires this port (already configured)
3. **Cold starts**: First request after idle takes ~5-10 seconds
4. **Memory**: 1Gi is sufficient for this app
5. **Region**: Choose closest to your users (us-central1, asia-south1, etc.)

## 🔗 Quick Links After Deploy

- **Your API**: `https://YOUR_SERVICE.run.app`
- **Health Check**: `https://YOUR_SERVICE.run.app/health`
- **API Docs**: `https://YOUR_SERVICE.run.app/docs`
- **Cloud Console**: [console.cloud.google.com/run](https://console.cloud.google.com/run)
- **Logs**: [console.cloud.google.com/logs](https://console.cloud.google.com/logs)

## 📚 Need More Details?

See `CLOUD_RUN_DEPLOY.md` for:
- Detailed step-by-step instructions
- Troubleshooting guide
- Security best practices
- Performance optimization
- Custom domain setup
- CI/CD configuration

## 🎉 That's It!

Your jewelry recommendation API is now live on Google Cloud Run! 🚀
