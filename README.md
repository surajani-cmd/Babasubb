# Babasubb — Real Database Edition

This version runs on a real Supabase (Postgres) database instead of on-device
storage. Accounts, wallet balances, and transaction history are now genuinely
stored in the cloud and sync across any device you log in from.

## Before this will work
You must have already run, in your Supabase project's SQL Editor:
1. The main schema (`babasubb_schema.sql` — profiles, wallets, transactions tables)
2. The phone lookup table (`phone_lookup.sql` — lets login work by phone number)

And in **Authentication → Sign In / Providers → Email**, "Confirm email" must be OFF.

## What's real now
- Signup creates a genuine Supabase Auth account (properly hashed password, not a toy hash)
- Login, session persistence, and logout all run through real Supabase Auth
- **Forgot password is fully real** — it sends an actual email with a reset link
  (this is why signup now requires a real email address)
- Wallet balance and every transaction are real rows in a real Postgres database,
  protected by Row Level Security so each user can only ever see their own data

## What's still simulated (no external API connected)
- The OTP step at signup shows the code directly on-screen rather than truly
  texting it (no SMS provider connected)
- "Funding" your wallet instantly credits it — no real card/bank is charged
- Buying airtime/data deducts real wallet balance and logs a real transaction,
  but doesn't top up an actual phone line (no telecom aggregator connected yet)

## Step 1 — Put it on GitHub
Upload these 5 files to your repo root: `index.html`, `manifest.json`, `sw.js`,
`icon-192.png`, `icon-512.png`.

## Step 2 — Turn on GitHub Pages
Repo → Settings → Pages → Deploy from branch `main`, folder `/ (root)`.
You'll get a URL like `https://yourusername.github.io/babasubb/`.

## Step 3 — Important: tell Supabase about your live URL
Password reset links need to know where to send people back to.
In Supabase: **Authentication → URL Configuration**
- Set **Site URL** to your GitHub Pages URL
- Add the same URL under **Redirect URLs**

Without this step, "Forgot password" emails will send but the reset link won't
land back on your app correctly.

## Step 4 — Test it
Open your GitHub Pages URL, sign up with a real email, fund your wallet, buy
airtime, log out, log back in — even from a different device — and your account
and balance will be exactly where you left them.

## Step 5 — Package as an installable APK
Go to **pwabuilder.com**, paste your GitHub Pages URL, "Package for stores" →
Android → download the `.apk`. Install it on your phone the same way as before.

---
### Where to go from here
This is now a genuinely real accounts + wallet + transactions system. What's
left to become a production fintech product:
- A licensed payment gateway (Paystack, Flutterwave) for real wallet funding
- A licensed bills/airtime aggregator (VTPass, Reloadly) for real purchases
- A security review before handling real users' real money — PIN verification
  currently happens client-side, which is fine for a demo but should move to a
  server-side check (a Supabase Edge Function) before going live
