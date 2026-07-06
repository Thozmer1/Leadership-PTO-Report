# Innovien PTO Dashboard

Internal-team PTO and earned-bonus-day tracker for the Innovien leadership team. Single-file static site (`index.html`) — no build step. Hosted on Vercel, deployed from GitHub.

## What it shows

- PTO taken (YTD), future PTO requested, and remaining PTO balance by person
- Earned bonus days taken and remaining (quarterly incentive)
- Internal employees only (Recruiting, Account Mgmt, Operations, Finance, TA Partner); client-site consultants excluded
- All figures in days (8 hrs = 1 day). Includes Approved + Pending requests.

## Monthly refresh (no code changes needed)

The dashboard ships with the current month's data embedded. To update the *live* numbers between deploys, open the site and use **Refresh Data** → drop the latest `ELT Time Off Requests` Excel export. This recomputes everything in the browser (nothing leaves the page).

To bake new data into the deployed version so everyone sees it without uploading:

1. Regenerate `index.html` with the new month's embedded data (ask Claude in the Leadership Reporting project, or replace the `const EMBEDDED = {...}` block).
2. Commit and push (see below). Vercel auto-deploys on push to `main`.

## First-time deploy (GitHub → Vercel)

1. **Create the repo** (private — this is internal data):
   ```bash
   cd "pto-dashboard-site"
   git init
   git add .
   git commit -m "Initial PTO dashboard"
   git branch -M main
   git remote add origin https://github.com/<org>/pto-dashboard.git
   git push -u origin main
   ```
2. **Connect to Vercel:** vercel.com → Add New → Project → import the repo.
   - Framework Preset: **Other**
   - Build Command: *(leave empty)*
   - Output Directory: *(leave empty — root)*
   - Deploy.
3. Vercel gives you a URL (e.g. `pto-dashboard.vercel.app`). Share with leadership. Every push to `main` redeploys automatically.

## Recurring updates

```bash
# after replacing index.html with a fresh export
git add index.html
git commit -m "PTO refresh — <month>"
git push
```

## Access control

The source data is confidential. Keep the GitHub repo **private** and enable **Vercel Password Protection** or **Vercel Authentication** (Project → Settings → Deployment Protection) so only Innovien staff can view the URL. Employee Excel exports are git-ignored and must never be committed.
