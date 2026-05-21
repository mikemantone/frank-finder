# Frank Finder — Deployment Guide

## File Structure

```
Hotdog Helper/
├── index.html        ← the app (Vercel serves this at your root URL)
├── vercel.json       ← Vercel config (static site, no build step)
├── .gitignore        ← keeps junk out of your repo
└── DEPLOY.md         ← this file
```

---

## Step 1 — Push to GitHub

### First time setup (create the repo)

1. Go to [github.com/new](https://github.com/new)
2. Name it something like `frank-finder`
3. Leave it **Public** (required for free Vercel hosting) or Private
4. Do **not** initialize with a README or .gitignore (you already have one)
5. Click **Create repository**

### From your terminal, inside this folder:

```bash
# Navigate to the project folder
cd "/Users/michaelmantone/Documents/Claude/Projects/Hotdog Helper"

# Initialize git
git init

# Stage all files
git add .

# First commit
git commit -m "Initial commit — Frank Finder app"

# Point to your new GitHub repo (replace YOUR_USERNAME and YOUR_REPO_NAME)
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git

# Push
git branch -M main
git push -u origin main
```

### For future updates:

```bash
git add .
git commit -m "Describe what you changed"
git push
```

---

## Step 2 — Deploy on Vercel

1. Go to [vercel.com](https://vercel.com) and sign in (or sign up — free tier works great)
2. Click **Add New → Project**
3. Click **Import** next to your `frank-finder` GitHub repo
4. On the configuration screen:
   - **Framework Preset:** Other
   - **Root Directory:** `./` (leave as-is)
   - **Build Command:** *(leave blank)*
   - **Output Directory:** `.` (a single dot)
5. Click **Deploy**

Vercel will give you a live URL like `https://frank-finder-xyz.vercel.app` within about 30 seconds.

### Auto-deploys

After the first deploy, every `git push` to `main` automatically triggers a new Vercel deployment. No extra steps needed.

---

## Custom Domain (optional)

1. In Vercel, go to your project → **Settings → Domains**
2. Add your domain (e.g., `frankfinder.com`)
3. Follow Vercel's DNS instructions for your registrar
