### App 3 — Brickston

**Tagline.** AI-enabled asset management. Scales AUM by focusing AMs on what creates value and what flags risk.

**Status.** Live (LFI flagship product). Mosser onboarded as a tenant.

**Audience.** LFI product running in a dedicated Mosser-only environment. No other clients in this instance — effectively single-tenant for Mosser.

**The problem.** Yardi (and PM systems generally) are great for the books and the resident of record. They're not great for the messy stuff that actually drives the day — emails, meeting notes, lender deadlines, vendor COI expirations, permits around your buildings, NOV cure dates. That stuff lives in inboxes and people's heads. Decisions get lost, commitments slip, and the same problems keep getting rediscovered.

**What it does.** Ingests narrative and operational data sources — M365 email, M365 calendar, Granola meeting notes, Smartsheet, Teams chat — alongside per-event triggers from Brickston Cloud SQL (lease 60-day expiries, AR aging bucket crossings, permits within ~500ft of portfolio properties, NOV cure deadlines, vendor COI expirations). Extracts tasks, commitments, decisions, and entities, resolved to the right property and resident where possible. Surfaces them in a per-tenant feed and as structured records queryable via the Brickston agent. A daily portfolio-level Power BI rollup ingest summarizes high-level facts (AR total, AR 90+, work orders, leads).

**Use cases.**

- Morning briefing: tasks landing today, commitments approaching, lease and AR triggers
- "What did we decide about the Mission property HVAC vendor change" — entity-resolved decision recall
- Permit and code violation awareness around portfolio properties (proximity- and unit-tagged)
- Vendor COI expiration tracking with property attribution
- A narrative layer the Mosser team can run questions against without needing Yardi access

**Stack.** Postgres-backed `items.*` schema (tasks, commitments, decisions, entities, property_lookup), narrative connectors (M365, Granola, Smartsheet, Teams), Brickston agent over the surface, daily Power BI rollup sync at 04:03 PT.

**Access.** Mosser users only — it's a Mosser-only environment. Mosser leadership says who on their side gets in.

**Owner.** Justin / LFI. LFI builds the product and sets the roadmap; Mosser uses the instance and has loud input on what gets built next.
