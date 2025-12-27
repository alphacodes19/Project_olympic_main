# Deployment Guide - Olympics Web Application

This guide will help you deploy your Django Olympics application to a live server. Since Heroku's free tier is no longer available, we'll use **Render** (recommended) or **Railway** as alternatives.

## Prerequisites

1. A GitHub account (to host your code)
2. A Render account (free tier available) - Sign up at [render.com](https://render.com)
3. Your project code ready to push to GitHub

---

## Step 1: Prepare Your Code

### 1.1 Generate a Django Secret Key

Run this command in your terminal to generate a secure secret key:

```bash
python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"
```

**Save this key** - you'll need it in Step 3.

### 1.2 Push Your Code to GitHub

If you haven't already, create a GitHub repository and push your code:

```bash
cd Olympics-main
git init
git add .
git commit -m "Prepare for deployment"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
git push -u origin main
```

**Important:** Make sure your `.env` file is in `.gitignore` (it should be by default) - never commit secrets to GitHub!

---

## Step 2: Create a Render Account

1. Go to [render.com](https://render.com)
2. Sign up for a free account (you can use GitHub to sign in)
3. Verify your email if required

---

## Step 3: Deploy on Render

### 3.1 Create a New Web Service

1. In your Render dashboard, click **"New +"** → **"Web Service"**
2. Connect your GitHub account if you haven't already
3. Select your repository: `YOUR_REPO_NAME`
4. Click **"Connect"**

### 3.2 Configure Your Service

Fill in the following details:

- **Name:** `olympics-app` (or any name you prefer)
- **Region:** Choose the closest region to your users
- **Branch:** `main` (or your default branch)
- **Root Directory:** Leave empty (or `Olympics-main` if your code is in a subdirectory)
- **Runtime:** `Python 3`
- **Build Command:** 
  ```bash
  pip install -r requirements.txt && python manage.py collectstatic --noinput
  ```
- **Start Command:**
  ```bash
  gunicorn olympics.wsgi
  ```

### 3.3 Set Environment Variables

Click on **"Environment"** tab and add these variables:

| Key | Value | Notes |
|-----|-------|-------|
| `SECRET_KEY` | `[Your generated secret key from Step 1.1]` | The secret key you generated |
| `DEBUG` | `False` | Always False in production |
| `ALLOWED_HOSTS` | `your-app-name.onrender.com` | Replace with your actual Render URL |
| `PYTHON_VERSION` | `3.10.12` | Python version |

**To find your Render URL:**
- After creating the service, Render will assign a URL like: `your-app-name.onrender.com`
- You can update `ALLOWED_HOSTS` later if needed

### 3.4 Advanced Settings (Optional)

- **Auto-Deploy:** Keep it enabled to automatically deploy on every push
- **Health Check Path:** `/` (default)

### 3.5 Deploy

1. Click **"Create Web Service"**
2. Render will start building and deploying your application
3. This process takes 5-10 minutes
4. You can watch the build logs in real-time

---

## Step 4: Post-Deployment Setup

### 4.1 Run Migrations

After deployment, you need to run database migrations:

1. In Render dashboard, go to your service
2. Click on **"Shell"** tab
3. Run:
   ```bash
   python manage.py migrate
   ```
4. (Optional) Create a superuser:
   ```bash
   python manage.py createsuperuser
   ```

### 4.2 Update ALLOWED_HOSTS

1. Go to **"Environment"** tab
2. Update `ALLOWED_HOSTS` with your actual Render URL:
   ```
   your-app-name.onrender.com
   ```
3. Save changes (this will trigger a redeploy)

---

## Step 5: Verify Deployment

1. Visit your Render URL: `https://your-app-name.onrender.com`
2. Test all the main features:
   - Home page loads
   - Athlete pages work
   - Country pages work
   - SPARQL queries execute

---

## Alternative: Deploy on Railway

If you prefer Railway, here's a quick guide:

### Railway Deployment Steps:

1. Sign up at [railway.app](https://railway.app)
2. Click **"New Project"** → **"Deploy from GitHub repo"**
3. Select your repository
4. Railway will auto-detect it's a Python app
5. Add environment variables:
   - `SECRET_KEY`
   - `DEBUG=False`
   - `ALLOWED_HOSTS=your-app.up.railway.app`
6. Railway will automatically:
   - Install dependencies
   - Run migrations
   - Start the server

Railway provides a PostgreSQL database automatically if needed.

---

## Troubleshooting

### Issue: Static files not loading

**Solution:** Make sure `collectstatic` runs during build. Check your build command includes:
```bash
python manage.py collectstatic --noinput
```

### Issue: Application crashes on startup

**Solution:** 
1. Check the logs in Render dashboard
2. Verify all environment variables are set correctly
3. Make sure `ALLOWED_HOSTS` includes your Render domain

### Issue: Database errors

**Solution:** 
- SQLite works on Render but has limitations
- For production, consider using Render's PostgreSQL (free tier available)
- Update `DATABASE_URL` environment variable if using PostgreSQL

### Issue: SPARQL queries not working

**Solution:**
- Verify your SPARQL endpoint is accessible from Render's servers
- Check if your endpoint requires authentication
- Review the `Sparql_queries/queries.py` file for endpoint configuration

---

## Environment Variables Reference

Create a `.env` file locally (never commit this) with:

```env
SECRET_KEY=your-secret-key-here
DEBUG=False
ALLOWED_HOSTS=your-app-name.onrender.com
DATABASE_URL=  # Optional - leave empty for SQLite
```

---

## Free Tier Limitations

**Render Free Tier:**
- Services spin down after 15 minutes of inactivity
- First request after spin-down takes ~30 seconds
- 750 hours/month free (enough for always-on if you're the only user)
- 100 GB bandwidth/month

**Railway Free Tier:**
- $5 credit/month
- Services may sleep after inactivity
- Good for development and small projects

---

## Next Steps

1. ✅ Your app is now live!
2. Consider setting up a custom domain (Render supports this)
3. Set up monitoring/error tracking (optional)
4. Configure automatic backups if using a database

---

## Support

If you encounter issues:
1. Check Render/Railway logs
2. Verify all environment variables
3. Test locally first: `python manage.py runserver`
4. Check Django's deployment checklist: https://docs.djangoproject.com/en/4.0/howto/deployment/checklist/

---

**Congratulations! Your Olympics application is now live! 🎉**


