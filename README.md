# HONBLUE Sales Intelligence Dashboard

A standalone, interactive sales analytics dashboard for HONBLUE covering 2019–2025.

## Files

```
honblue-dashboard/
├── index.html      # Main dashboard application
├── accounts.js     # Account-level data (2019–2025, all years)
├── divisions.js    # Division breakdown data (AEC, CP, DP, LFG — 2024–2025)
└── README.md       # This file
```

## Features

- **Year selector** — Compare any two years from 2019–2025 (2026 partial)
- **Segments** — Revenue by industry segment with drill-down
- **Divisions** — AEC / CP / DP / LFG breakdown
- **Revenue Trends** — 7-year trendline with COVID context
- **Attrition Watch** — Dynamic list, configurable threshold
- **Expansion** — Growth accounts and moderate decline targets
- **LFG Lumpiness** — Post-project fall-off detection
- **Whitespace** — Known targets with zero revenue
- **By Salesperson** — Individual rep performance and drill-down
- **Account Search** — Full-text search across all 3,000+ accounts

## Run locally

Just open `index.html` in a browser:

```bash
open index.html
# or
python3 -m http.server 3000
# then visit http://localhost:3000
```

> Note: Chrome may block local file access for the JS imports. Use `python3 -m http.server` if you see a blank screen.

---

## Deploy to GitHub + Vercel (5 minutes)

### Step 1 — Push to GitHub

```bash
# From inside the honblue-dashboard folder:
git init
git add .
git commit -m "Initial dashboard"

# Create a new repo on github.com (name it: honblue-dashboard)
# Then connect and push:
git remote add origin https://github.com/YOUR_USERNAME/honblue-dashboard.git
git branch -M main
git push -u origin main
```

### Step 2 — Deploy on Vercel

1. Go to [vercel.com](https://vercel.com) → Sign in with GitHub
2. Click **"Add New Project"**
3. Import your `honblue-dashboard` repo
4. Vercel auto-detects it as a static site — no config needed
5. Click **Deploy**

You'll get a URL like: `https://honblue-dashboard.vercel.app`

### Step 3 — Set a custom URL (optional)

In Vercel project settings → Domains, you can:
- Add a custom domain (e.g. `sales.honblue.com`) if you own it
- Or use a Vercel subdomain like `honblue-sales.vercel.app`

### Password protection (optional, recommended)

Vercel Pro ($20/mo) adds password protection. Free alternative:

Add a `.vercel.json` file:
```json
{
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        { "key": "Cache-Control", "value": "no-store" }
      ]
    }
  ]
}
```

For basic auth, use Vercel Edge Middleware (free tier):
Create `middleware.js` in the root:
```js
import { NextResponse } from 'next/server'

export function middleware(request) {
  const auth = request.headers.get('authorization')
  if (auth === 'Basic ' + btoa('honblue:YOUR_PASSWORD')) {
    return NextResponse.next()
  }
  return new NextResponse('Unauthorized', {
    status: 401,
    headers: { 'WWW-Authenticate': 'Basic realm="HONBLUE"' }
  })
}
```

## Updating data

When you export a new Excel file:
1. Re-run the Python extraction script (ask Claude)
2. Replace `accounts.js` and `divisions.js`
3. `git add . && git commit -m "Data refresh" && git push`
4. Vercel auto-deploys in ~30 seconds

## Data notes

- `2026` values are partial (through ~3/30/2026) — labeled with `*` in the UI
- Division data (AEC/CP/DP/LFG) only available for 2024–2025
- Salesperson assignments from "CRM Primary" column in NAV sheet
- ~3,100 accounts with any revenue across all years
