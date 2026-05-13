### App 1 — Leasing (Competitor Intelligence)

**Tagline.** Daily-refreshing intelligence on every SF multifamily competitor listing — rents, concessions, days-on-market, removals.

**Status.** Live. Daily ETL via GitHub Actions. Next.js dashboard deployable to Vercel.

**Audience.** Mosser only.

**The problem.** Mosser watches what competitors are doing on rents and concessions across SF. The old way was a shared-drive Excel that corrupted itself constantly, lost history, and didn't dedupe across sources. Teams were re-pulling the same data every week and still missing concession changes in between.

**What it does.** Scrapes the public listings pages of eight SF competitor operators (2B Living, Gaetani, Brick + Timber, Trinity SF, Parkmerced/Maximus, RentSFNow/Veritas, Mosser Living, Rentals Inc.) plus a Craigslist SF multifamily filter. Normalizes to a Postgres schema with full Type 2 history. Attributes anonymous Craigslist posts back to operators via an editable fingerprint dictionary (URL, phone, email, address, phrase, building pattern weights). Captures listing photos via perceptual hash + sha256 to dedupe across sources and survive Craigslist's 7-day URL expiry.

**Use cases.**

- Daily concession parity check before setting new vacancy quotes
- "Where is Veritas pricing one-bedrooms in the Mission this month" — answered against the comparable subset
- Days-on-market by operator and submarket
- Removal event tracking (listings that disappear without a known lease — flags pre-removed inventory)
- Craigslist activity by operator (some operators rely on CL; others don't — useful color for who's struggling)
- Source data for the Pricing and Revenue Management app

**Stack.** Neon Postgres 15 (free tier, pgvector), Cloudflare R2 for photo thumbnails, Next.js 14 on Vercel, Python ETL (HTTP scraping for AppFolio/RentCafe, Playwright for JS-rendered + Craigslist), GitHub Actions cron at 6am PT daily.

**Access.** Mosser leasing leads, Mosser asset management, LFI team. Read access via Excel on SharePoint (Power Query → Postgres ODBC) for non-technical users; AI Q&A via the project chat using a read-only DB role.

**Owner.** Justin / LFI. Hosted in `04-mosser.apps/leasing`. Daily run health is on the app's `/health` route.
