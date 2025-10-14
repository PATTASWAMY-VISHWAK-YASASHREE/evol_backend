# 🚀 Quick Deployment Answer

## Should I delete generated JSON files?

**NO! Keep everything.** Here's what to do:

### ✅ Files to KEEP (Deploy with these):
```
✓ celebrities.json                     (original data)
✓ products.json                        (original data)
✓ celebrities_with_vectors.json        (KEEP - has embeddings)
✓ products_with_vectors.json          (KEEP - has embeddings)
✓ celebrity_embeddings.pkl            (KEEP - fast loading)
✓ product_embeddings.pkl              (KEEP - fast loading)
✓ *_metadata.json                     (KEEP - metadata)
✓ main.py                             (FastAPI server)
✓ recommender_engine.py               (core logic)
✓ style_taxonomy.py                   (style mappings)
✓ requirements.txt                    (dependencies)
✓ index.html                          (frontend)
```

### ❌ Optional to DELETE (not needed in production):
```
✗ generate_celebrity_vectors.py       (only for regenerating)
✗ generate_product_vectors.py        (only for regenerating)
✗ user_questionnaire.py              (CLI only)
✗ api_test.py                        (testing only)
✗ test.py                            (testing only)
✗ __pycache__/                       (auto-ignored anyway)
```

## Why Keep Generated Files?

1. **Fast Startup**: Pre-computed embeddings load in milliseconds vs. minutes to regenerate
2. **No Model Download**: Render won't need to download sentence-transformers model (saves time & bandwidth)
3. **Consistency**: Same embeddings = same recommendations every time
4. **Free Tier Friendly**: Regenerating embeddings on every cold start would timeout on free tier

## 🎯 Deployment Commands

```powershell
# 1. Verify embeddings exist
dir *.pkl

# 2. Initialize git (if not done)
git init
git add .
git commit -m "Initial commit"

# 3. Push to GitHub
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git branch -M main
git push -u origin main

# 4. Deploy on Render
# - Go to render.com
# - New Web Service
# - Connect GitHub repo
# - It will auto-detect render.yaml
# - Click "Create Web Service"
```

## 📋 Render Configuration (Already Created)

- **Build Command**: `pip install -r requirements.txt`
- **Start Command**: `uvicorn main:app --host 0.0.0.0 --port $PORT`
- **Files**: `render.yaml` ✓ (already created)

## 📊 Total Size Check

```powershell
Get-ChildItem *.json, *.pkl | Measure-Object -Property Length -Sum

# Expected: ~5-10 MB total (well within Render limits)
```

## 🔗 After Deployment

1. **API will be at**: `https://your-app-name.onrender.com`
2. **Update index.html**:
   ```javascript
   // Change:
   const API_URL = 'http://localhost:8000';
   
   // To:
   const API_URL = 'https://your-app-name.onrender.com';
   ```
3. **Host frontend** on Netlify/Vercel or serve from same Render service

## 💡 Key Points

- ✅ Deploy with ALL files including generated embeddings
- ✅ Total size ~5-10 MB (Render handles this easily)
- ✅ First request might take 30s on free tier (cold start)
- ✅ Subsequent requests will be fast
- ✅ No regeneration needed = faster & cheaper

See `DEPLOYMENT.md` for detailed step-by-step guide!
