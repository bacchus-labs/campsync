# Data & Integrations

## Integration Touch‑Points

| Domain | Needed Data |
|--------|-------------|
| **Camp listings** | Metadata, schedules, pricing, capacity |
| **Maps & Geo** | Geocoding, maps tiles, distance calc |
| **Payments** | Card & ACH processing, PCI |
| **Email / SMS** | Notifications, marketing |
| **Analytics** | Behavior, funnels |
| **CRM** | Provider relations |
| **Calendar** | Family import of session dates |
| **Accreditation** | Camp compliance status |

## Data & Integration Requirements

| Domain | Must‑have data | Likely integration |
|--------|----------------|---------------------|
| Listings | Camp & session metadata, prices, capacity | ACTIVE Network Activity Search API |
| Accreditation | ACA‑verified status | American Camp Association directory |
| SaaS platforms | Rosters, real‑time availability | CampSite API, CampMinder API, UltraCamp API |
| Local gov’t rec | Day‑camp CSV/JSON feeds | e.g. Montgomery County “Recreation Summer Camps” (Data.gov) |
| Maps & geo | Lat/long, routing | Google Maps or Mapbox |
| Payments | Card, ACH, payouts | Stripe Connect |


## Potential camp sources

| Source | Coverage | Access | Notes |
|--------|----------|--------|-------|
| ACTIVE Network / ACTIVEkids | 200k+ activities including camps US‑wide | REST API (key, rate‑limited) |
| American Camp Association (ACA) | ~3,900 accredited camps | Public directory + partner CSV export | Accreditation authoritative |
| CampSite | ~900 camps on SaaS platform | Paid REST API module | Full roster & enrollment endpoints |
| CampMinder | ~1,400 camps | GraphQL/REST API + Zapier | Boulder‑based; rich media |
| UltraCamp | ~650 organizations | Swagger‑based REST API | Requires per‑client tokens |
| Municipal Parks & Rec Open Data (e.g., Montgomery County, MD “Recreation Summer Camps” CSV) | Local day camps | Open CSV/JSON | Free, but coverage varies |
| YMCA / Scouts of America | 2,700+ YMCA sites; 400+ BSA councils | Partnership feeds or sitemap scrape | Large national footprint |


https://www.activityhero.com/in/san-francisco-ca



### Top challenges
- Data heterogeneity & freshness across multiple vendor APIs and open‑data feeds.
- Availability sync to prevent overselling seats.
- Privacy / COPPA compliance when storing children’s PII & medical forms.
- Licensing costs & API rate limits (ACTIVE, CampSite, etc.) 