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

---

## Step 3 — Set up Firebase (Google Sign-In)

The app uses Firebase Authentication (Google provider) and Firestore to persist logins and reviews. Both are **free** on Firebase's Spark plan.

### 3a — Create a Firebase project

1. Go to [console.firebase.google.com](https://console.firebase.google.com)
2. Click **Add project** → name it `frank-finder` → click through (disable Google Analytics if you like)
3. Once created, click the **web icon `</>`** to add a web app → name it `frank-finder` → click **Register app**
4. Copy the `firebaseConfig` object shown — you'll paste these values into `index.html`

### 3b — Enable Google Sign-In

1. In the left sidebar go to **Build → Authentication → Sign-in method**
2. Click **Google** → toggle **Enable** → set your support email → **Save**

### 3c — Create a Firestore database

1. In the left sidebar go to **Build → Firestore Database**
2. Click **Create database** → choose **Start in test mode** → pick a region (e.g., `us-east1`) → **Enable**

> Test mode allows reads/writes for 30 days. Before going public, update the rules under **Firestore → Rules** to:
> ```
> rules_version = '2';
> service cloud.firestore {
>   match /databases/{database}/documents {
>     match /users/{uid} {
>       allow read, write: if request.auth != null && request.auth.uid == uid;
>     }
>     match /reviews/{reviewId} {
>       allow read: if true;
>       allow create: if request.auth != null;
>       allow update, delete: if request.auth != null && request.auth.uid == resource.data.userId;
>     }
>   }
> }
> ```

### 3d — Add your Vercel domain to Firebase Auth

1. In **Authentication → Settings → Authorized domains** click **Add domain**
2. Add your Vercel URL (e.g., `frank-finder-xyz.vercel.app`) and your custom domain if you have one

### 3e — Paste your config into the app

Open `index.html` (and `frank-finder.html`) and find this block near the top of the `<script>`:

```javascript
const firebaseConfig = {
  apiKey:            "YOUR_API_KEY",
  authDomain:        "YOUR_PROJECT_ID.firebaseapp.com",
  projectId:         "YOUR_PROJECT_ID",
  storageBucket:     "YOUR_PROJECT_ID.appspot.com",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId:             "YOUR_APP_ID"
};
```

Replace each placeholder with the real values from Step 3a. Then:

```bash
git add .
git commit -m "Add Firebase config"
git push
```

Vercel redeploys automatically. Google Sign-In will now work on your live site.
