# Publish GentleTrack on Google Play Store

## Step 1: Prepare the Privacy Policy

You need a public URL for the privacy policy. Options:
- Host on GitHub Pages (free)
- Host on your own website
- Use a free privacy policy hosting service

---

## Step 2: Build the Production AAB

Play Store requires AAB (Android App Bundle), not APK.

```bash
cd /Users/yts/lab/apps/habittracker/gentletrack
eas build --platform android --profile production
```

---

## Step 3: Create the App in Play Console

1. Go to [play.google.com/console](https://play.google.com/console)
2. Click **Create app**
3. Fill in:
   - App name: `GentleTrack`
   - Default language: `English (United States)`
   - App or Game: **App**
   - Free or Paid: **Free**
4. Check both declaration boxes → Click **Create app**

---

## Step 4: Complete "Set up your app" Checklist

Play Console shows ~12 tasks. Go through each:

### 4a. Privacy Policy
- Paste your privacy policy URL

### 4b. App Access
- Select **"All functionality is available without special access"**
- Save

### 4c. Ads
- Select **"No, my app does not contain ads"**
- Save

### 4d. Content Rating
- Click **Start questionnaire**
- Category: pick **"Utility, Productivity, Communication, or Other"**
- Answer all questions **No** (no violence, no sexual content, no gambling, etc.)
- Save → **Submit**
- You'll get a rating of **Everyone**

### 4e. Target Audience
- Select age group: **18 and over** (safest, avoids COPPA requirements)
- Save

### 4f. News App
- Select **No**
- Save

### 4g. Data Safety
- Does your app collect or share any user data? → **Yes**
- Click **Next**
- Is any data encrypted in transit? → **No** (all local)
- Do you provide a way for users to request data deletion? → **No**
- **Data types collected:**
  - Check **"Purchase history"** under Financial
  - Purpose: **"App functionality"**
  - Is it required? **Yes**
  - All other categories: **Not collected**
- Data shared with third parties: **None**
- Save → **Submit**

### 4h. Government Apps
- Select **No**

### 4i. Financial Features
- Select **No** (unless prompted, some regions skip this)

---

## Step 5: Store Listing

Go to **Grow** → **Main store listing**:

- **App name**: `GentleTrack — Habit Tracker`
- **Short description** (80 chars):
  `Guilt-free habit tracking. No streaks, no shame. Just gentle daily logging.`
- **Full description**: See below or write your own (4000 chars max)
- **App icon**: 512x512 PNG
- **Feature graphic**: 1024x500 PNG (required)
- **Phone screenshots**: minimum 2, recommended 4-8
  - Format: JPEG or PNG, 16:9 ratio, min 320px, max 3840px per side
- **Category**: Health & Fitness
- **Contact details**: developer email (required)

Click **Save**

---

## Step 6: Create the In-App Product

1. Go to **Monetize** → **In-app products**
2. Click **Create product**
3. Product ID: `premium_unlock`
4. Name: `Premium Unlock`
5. Description: `Unlock unlimited habit tracking`
6. Default price: **$5.99**
7. Click **Save** → **Activate**

---

## Step 7: Set Up RevenueCat

### 7a. Create Google Cloud Service Account

1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Select or create a project
3. **APIs & Services** → **Library** → search **"Google Play Android Developer API"** → **Enable**
4. **IAM & Admin** → **Service Accounts** → **Create Service Account**
5. Name: `revenuecat-gentletrack`
6. Role: skip (we'll set it in Play Console)
7. Click **Done**
8. Click the service account → **Keys** → **Add Key** → **Create new key** → **JSON** → **Create**
9. Save the downloaded JSON file

### 7b. Link in Play Console

1. Play Console → **Setup** → **API access**
2. Link your Google Cloud project
3. Under **Service accounts** → find your account → **Grant access**
4. Permissions: **Admin** (or at minimum: View app information, View financial data, Manage orders and subscriptions)
5. Save

### 7c. Configure RevenueCat

1. Go to [app.revenuecat.com](https://app.revenuecat.com)
2. Create a new **Project**: `GentleTrack`
3. Add **Google Play app**: package name `com.gentletrack.app`
4. Upload the service account JSON key
5. Go to **Entitlements** → Create: identifier `premium`
6. Go to **Offerings** → Create offering `default`
7. Add package → select `premium_unlock` product → attach to `premium` entitlement
8. Go to **API Keys** → copy the **public Google API key**
9. Replace the test key in `src/features/premium/constants.ts` with the production key

---

## Step 8: Upload the AAB

1. Play Console → **Production** → **Create new release**
2. **App signing**: click **Continue** (let Google manage signing)
3. Upload the AAB file
4. Release name: `1.10.0`
5. Release notes: `First release — guilt-free habit tracking with unlimited customization`
6. Click **Review release**

---

## Step 9: Select Countries

1. **Production** → **Countries/regions**
2. Click **Add countries/regions**
3. Select all or pick specific ones
4. Save

---

## Step 10: Submit for Review

1. Go back to **Production** → your release
2. Click **Start rollout to Production**
3. Confirm

Review takes **1-3 days**. You'll get an email when approved.

---

## Quick Reference

| Item | Value |
|---|---|
| Package name | `com.gentletrack.app` |
| Version | `1.10.0` |
| Version code | `8` |
| Product ID | `premium_unlock` |
| Price | $5.99 |
| RevenueCat entitlement | `premium` |
| Category | Health & Fitness |
| Content rating | Everyone |
| Target audience | 18+ |
