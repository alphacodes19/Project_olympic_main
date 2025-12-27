# Quick Start - Deployment Checklist

Follow these steps in order to deploy your application:

## ✅ Pre-Deployment Checklist

- [ ] Code is pushed to GitHub
- [ ] Generated a Django SECRET_KEY
- [ ] Created account on Render.com (or Railway.app)
- [ ] Reviewed DEPLOYMENT.md guide

## 🚀 Quick Deployment Steps

### 1. Generate Secret Key
```bash
python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"
```
**Copy and save this key!**

### 2. Push to GitHub
```bash
git add .
git commit -m "Ready for deployment"
git push origin main
```

### 3. Deploy on Render
1. Go to [render.com](https://render.com) → New Web Service
2. Connect your GitHub repo
3. Configure:
   - **Build Command:** `pip install -r requirements.txt && python manage.py collectstatic --noinput`
   - **Start Command:** `gunicorn olympics.wsgi`
4. Add Environment Variables:
   - `SECRET_KEY` = [your generated key]
   - `DEBUG` = `False`
   - `ALLOWED_HOSTS` = `your-app-name.onrender.com` (update after first deploy)
5. Click "Create Web Service"

### 4. After First Deploy
1. Go to Shell tab in Render
2. Run: `python manage.py migrate`
3. Update `ALLOWED_HOSTS` with your actual Render URL
4. Visit your app URL!

## 📝 Environment Variables Needed

```
SECRET_KEY=your-secret-key-here
DEBUG=False
ALLOWED_HOSTS=your-app-name.onrender.com
```

## 🔗 Full Guide

See [DEPLOYMENT.md](DEPLOYMENT.md) for detailed instructions and troubleshooting.


