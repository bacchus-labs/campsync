
# CampSync Product Specification

**Version:** 0.1  
**Date:** 2025-08-01

---

## 1. Product Summary
CampSync is a nationwide discovery and registration platform that aggregates youth camps and enrichment programs across the United States. Parents, guardians, and caregivers can search, compare, and book camps in one place, while camp operators gain a self‑service portal to list programs, manage inventory, and accept payments.

---

## 2. Key Use‑Cases

| Persona | Need | Primary Flow |
|---------|------|--------------|
| **Parent / Guardian** | Find camps that fit child’s interests, schedule, budget, and location | Multi‑facet search → compare → register & pay → receive reminders |
| **Teen Camper** | Discover specialty camps (STEM, sports) and see friends’ attendance | Browse/like camps → share links →
mobile ticket/pass |
| **Camp Operator** | Acquire new campers, manage rosters, collect forms/payments | Onboard → list programs → sync availability →
handle registrations |
| **School / District** | Recommend accredited camps to families | Embed white‑label search widget → track referrals |
| **Affiliates / Bloggers** | Share curated camp lists and earn commission | Generate deep links with referral codes |

---

## 3. Functional Scope

1. **Camp Marketplace** – Search by ZIP, dates, age, theme, cost, availability, accreditation.  
2. **Registration & Checkout** – Multi‑camper cart, medical forms, discounts, payment plans, saved cards, tax receipts.  
3. **Provider Console** – CRUD listings, seat management, wait‑list, coupon codes, messaging, analytics.  
4. **User Accounts** – Profiles, saved campers, preferences, favorites, calendar/ICS export.  
5. **Reviews & Ratings** – Verified post‑attendance reviews, NPS dashboards for operators.  
6. **Partner Widgets / API** – Embed search results on third‑party sites; outbound affiliate tracking.  
7. **Mobile UX** – PWA with offline tickets and push reminders.

---

## 4. Data Requirements

### 4.1 Core Entities
* **Camp** (id, provider_id, name, description, accreditation, tags, media)
* **Program / Session** (camp_id, title, start/end, price, capacity, availability, min_age, max_age)
* **Location** (geo, venue, address, timezone, indoor/outdoor flags)
* **Provider** (org details, tax status, banking)
* **Camper** (PII, medical notes, guardians, consents)
* **Registration** (camper_id, session_id, status, payment_plan, forms, waivers)

### 4.2 Integration Touch‑Points
| Domain | Needed Data | Candidate Integration |
|--------|-------------|-----------------------|
| **Camp listings** | Metadata, schedules, pricing, capacity | *ACTIVE Network Activity Search API*, *ACA “Find a Camp” directory*, *CampSite, CampMinder, UltraCamp* platform APIs |
| **Maps & Geo** | Geocoding, maps tiles, distance calc | Google Maps Platform or Mapbox |
| **Payments** | Card & ACH processing, PCI | Stripe Connect (marketplace), PayPal, Plaid |
| **Email / SMS** | Notifications, marketing | SendGrid, Twilio |
| **Analytics** | Behavior, funnels | Segment → BigQuery |
| **CRM** | Provider relations | HubSpot, Salesforce |
| **Calendar** | Family import of session dates | iCalendar (RFC 5545) feed, Google Calendar push |
| **Accreditation** | Camp compliance status | ACA accreditation feed, state license registries |

---

## 5. Key Challenges & Mitigations

| Category | Challenge | Mitigation |
|----------|-----------|------------|
| **Data Quality** | Heterogeneous schemas & stale data from ~6 vendor APIs | ETL pipeline with nightly delta ingest → canonical schema → manual review queue |
| **Availability Sync** | Real‑time seat counts to prevent oversell | Webhooks / polling every 10 min; optimistic locking at checkout |
| **Legal / Privacy** | COPPA & HIPAA (medical forms) compliance | Age‑gate accounts, parental consent, encrypted-at-rest docs, signed BAAs with vendors |
| **Payments** | Marketplace money‑flow and refunds across 50 states | Stripe Connect with separate payouts → automated W‑9 and 1099‑K |
| **Seasonality** | Traffic spikes Jan–May; quiet rest of year | Autoscaling; warm caches; off‑season cost controls |
| **Licensing Fees** | Some provider APIs have per‑call cost | Cache results; negotiate bulk data licensing; fall back to scraping public listings where legal |

---

## 6. External Data & Provider Matrix

| Source | Coverage | Access | Notes |
|--------|----------|--------|-------|
| ACTIVE Network / ACTIVEkids | 200k+ activities including camps US‑wide | REST API (key, rate‑limited) |
| American Camp Association (ACA) | ~3,900 accredited camps | Public directory + partner CSV export | Accreditation authoritative |
| CampSite | ~900 camps on SaaS platform | Paid REST API module | Full roster & enrollment endpoints |
| CampMinder | ~1,400 camps | GraphQL/REST API + Zapier | Boulder‑based; rich media |
| UltraCamp | ~650 organizations | Swagger‑based REST API | Requires per‑client tokens |
| Municipal Parks & Rec Open Data (e.g., Montgomery County, MD “Recreation Summer Camps” CSV) | Local day camps | Open CSV/JSON | Free, but coverage varies |
| YMCA / Scouts of America | 2,700+ YMCA sites; 400+ BSA councils | Partnership feeds or sitemap scrape | Large national footprint |

---

## 7. Nationwide Roll‑Out Considerations

* **Geo‑scaling** – Use PostGIS and spatial indexes for sub‑10 ms radius queries at national scale.  
* **Timezone‑aware Scheduling** – All dates stored in ISO‑8601 UTC, rendered via client TZ.  
* **Marketing Growth Loops** – Referral codes, influencer lists, SEO landing pages per city/tag.

---

## 8. KPI Dashboard (MVP)

| Metric | Goal |
|--------|------|
| Search‑to‑Checkout Conversion | ≥ 3 % |
| Average Revenue per Camper (ARPC) | > $450 |
| Provider NPS | ≥ +60 |
| Data Freshness (days) | ≤ 2 |

---

## 9. Roadmap Snapshot

1. **M0 (4 weeks)** – Nationwide camp ingest, search MVP, email wait‑list.  
2. **M1 (8 weeks)** – Checkout, payments, provider console.  
3. **M2 (12 weeks)** – Mobile PWA, reviews, affiliate widgets.  
4. **M3 (16 weeks)** – Real‑time availability sync, accreditation badges, CRM integrations.

---

