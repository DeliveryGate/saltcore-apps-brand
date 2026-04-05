# SaltCore Apps — Portfolio Website Spec

## Domain
saltcoreapps.com (or saltcore.app)

## Tech Stack
- Next.js 14 (App Router)
- Tailwind CSS
- Deployed on Railway
- Static-style — no database needed

## Page Structure

### Home (/)
**Hero:**
- Heading: "Shopify apps built for merchants who need tools that just work"
- Subheading: "Simple, focused apps from SaltCore — no bloat, no fluff"
- CTA: "Browse our apps" (scrolls to grid)

**Apps Grid (3 per row desktop, 1 mobile):**
Each card:
- App icon (SVG)
- App name
- One-line description
- Price starting from
- "View on App Store" button

Apps:
1. SafeServe — Allergen badges and nutrition labels. From Free.
2. DeliveryIQ — Delivery dates, postcodes, fees, volume discounts. $29.99/mo.
3. BlogFlow — AI blog posts using your products. From Free.
4. BundleBox — Fixed, mix & match, and volume bundles. From Free.
5. QuoteFlow — Quote requests and B2B pricing. From Free.

**Why SaltCore:**
- Built by merchants for merchants (Vanda's Kitchen reference)
- No unnecessary features — each app does one thing well
- Responsive support from real people
- Targeting Built for Shopify certification on every app

**Footer:**
SaltCore Group Limited | Links to each app | Privacy | Contact

### Individual App Pages (/apps/[name])
- App icon large
- Full description
- Screenshots carousel
- Feature list
- Pricing table
- "Install on Shopify" button
- SEO meta tags per page

## SEO
- sitemap.xml auto-generated
- robots.txt
- Meta title: "[App Name] — Shopify App by SaltCore"
- Meta description: unique per page
- Open Graph images: app icon on brand colour background
