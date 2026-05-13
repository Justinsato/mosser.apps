### App 2 — Pricing and Revenue Management

**Tagline.** Supports the leasing team in setting rents — balancing restrictive covenants and reporting requirements while keeping value creation the goal.

**Status.** Underway. Scaffolding complete (Brickston Cloud SQL connector, Pricing Neon schema templates); build plan estimates 9–10 working days across 9 phases.

**Audience.** Mosser only.

**The problem.** Leasing is the team actually setting rents — across 20 properties and ~2,561 units, every month. They're balancing three things at once: where the market is, what we're allowed to do (AGA, banking, lender covenants, lender and investor reporting, §37.10C / RealPage), and what the underwriting needs to create value. Doing that in Excel against partial data costs us on both sides — rent left on the table and rent we shouldn't have pushed.

**What it does.** Supports the leasing team with rent recommendations that respect every constraint and stay pointed at value creation. First-party Mosser data + public listing data only.

- Reads `mosser.properties`, `mosser.units`, `mosser.leases`, `pbi.pbi_unit_status`, `pbi.pbi_concessions` from Brickston Cloud SQL via the Auth Proxy (read-only, IAM-scoped, allowlisted)
- Reads `v_active_listings`, `v_recent_price_moves`, `v_cl_ads_attributed`, `operators` from the Leasing Neon (read-only role)
- Writes into its own Pricing Neon (separate project from Leasing): `mosser_mirror`, `competitor_mirror`, `pricing.comp_sets`, `pricing.market_gap_snapshot`, `pricing.recommendations`

Outputs vacancy-reset recommendations with comp-set evidence, concession parity flags, market gap by floor plan and submarket, and AGA-bounded renewal ranges (current AGA: 1.6% for 3/1/2026–2/28/2027).

**Use cases.**

- Vacancy-reset rent recommendations for the leasing team, with the comp evidence behind them
- AGA-bounded renewal ranges that respect banking rules and lender covenants
- Concession parity alerts when the comp set moves and our offers don't
- Reporting-ready outputs that fit lender and investor templates
- Audit trail of what was recommended vs. what was set — closes the loop on whether recs are landing
- Defensible-by-design under §37.10C and the RealPage settlement

**Stack.** Python (psycopg3, Cloud SQL Auth Proxy default), Neon Postgres, BI rendering layer TBD, deploy + cron in phase 7. Compliant-by-construction with §37.10C and the RealPage settlement.

**Access.** Mosser asset management, LFI pricing analyst seat. Restricted because outputs incorporate first-party unit and lease data. Recommendations get circulated outside the app via standard memo channels.

**Owner.** Justin / LFI. Hosted in `04-mosser.apps/pricing`. Hard kill switch documented in `BUILD_PLAN.md` Section 7 if the regulatory perimeter changes.
