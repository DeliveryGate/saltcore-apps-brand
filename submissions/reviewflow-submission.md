# ReviewFlow — App Submission Pack
**App Name:** ReviewFlow — Product Reviews
**Tagline:** Product reviews that actually match your brand. Better design, smarter requests, more conversions.

---

## Short Description (160 chars)
Collect and display product reviews with fully customisable widgets. One review request per order. AI summaries. Food-specific templates. Free plan available.

---

## Full Description

Judge.me has 18,000 reviews. So why do so many merchants hate how their review widgets look?

ReviewFlow fixes the design problem. Every widget theme is premium out of the box — minimal, bold, soft, modern, or food-specific. Then customise colours, fonts, layout, and border radius to match your brand exactly. In under 2 minutes.

**Why ReviewFlow is different:**

✓ **DESIGN FIRST** — 5 premium widget themes, fully customisable to match any brand. Minimal, Bold, Soft, Modern, and Food & Hospitality. Adjust colours, fonts, border radius, and layout. No more generic widgets that look wrong.

✓ **SMART REQUESTS** — One review request per order, not one per product. We don't spam your best customers. A customer buying 10 items gets one email, not ten.

✓ **FOOD BUSINESS TEMPLATES** — Ask about taste, freshness, allergen accuracy, packaging, and whether they'd reorder. Questions generic apps can't ask.

✓ **AI SUMMARIES (Pro)** — "Based on 47 reviews, customers love the flavour and fast delivery. A few mentioned smaller portions than expected." Real insight, not generic text.

✓ **PHOTO & VIDEO REVIEWS** — All plans include photo reviews. Video reviews on Pro.

✓ **ONE-CLICK IMPORT** — Move from Judge.me, Loox, or Yotpo in minutes. CSV import on Starter+.

**Widget types:**
- Product reviews list
- Star rating badge for collection pages
- All reviews store page
- AI summary callout
- Review photo gallery

**Works with:** Online Store 2.0, Google Shopping rich snippets (Starter+)

---

## Key Benefits

**1. The review widget that actually matches your brand**
Every widget theme is designed to look premium out of the box. The live preview widget builder lets you adjust colours, fonts, border radius, and layout with instant preview. Your reviews should look like they belong on your store — not like they came from a third-party app.

**2. One review request per order — not one per product**
Judge.me's biggest complaint: it sends a separate review email for each product in an order. Buy 5 products, get 5 emails. ReviewFlow sends one email that covers the whole order. Customers are more likely to respond, and you're less likely to get marked as spam.

**3. Food-specific review questions your customers actually want to answer**
General apps ask "was it good?" Food businesses need to know: was it fresh? Was the allergen info accurate? Would you reorder? ReviewFlow has purpose-built templates for food, fashion, beauty, and more — so you collect data that's actually useful.

---

## Pricing Tiers

| Plan | Price | Features |
|------|-------|----------|
| **Free** | $0/month | Up to 50 reviews/month, photo reviews, basic widget (minimal theme), watermark |
| **Starter** | $19/month | Unlimited reviews, all 5 custom widget themes, auto review requests (1 per order), custom questions, CSV import from Judge.me/Loox/Yotpo, Google Shopping rich snippets, analytics, multiple widget placements, email support |
| **Pro** | $39/month | Everything in Starter + AI review summaries, video reviews, food & category templates, review groups across variants, priority support |

---

## App Category
**Store content** → Product reviews

---

## Keywords
reviews, product reviews, review app, star ratings, Judge.me alternative, photo reviews, food reviews, review widget, Loox alternative, customer reviews

---

## Support Email
hello@saltai.app

## Privacy Policy
https://saltai.app/privacy/reviewflow

## App URL
https://reviewflow-production-production.up.railway.app

---

## Feature Checklist

- [x] Tagline confirmed: "Product reviews that actually match your brand."
- [x] No false claims — all features built and verified
- [x] QR differentiators: design-first, 1-per-order requests, food templates, AI summaries
- [x] PLANS object matches submission: Free / Starter ($19) / Pro ($39)
- [x] CSV import gated to Starter+ ✓
- [x] AI summaries gated to Pro ✓
- [x] Theme extension: 5 blocks (product_reviews, star_badge, all_reviews, review_summary, photo_gallery)
- [x] Smart review requests: webhook → ReviewRequest → cron send → 1 email per order
- [x] Hosted review form at /review?token=xxx
- [x] Food template: taste, freshness, allergen accuracy, would-reorder questions
- [x] Postmark email service
- [x] OpenAI gpt-4o-mini for AI summaries
- [x] Founding member credit wired in

---

## Build Notes

**Architecture:**
- Node/Express backend + Prisma/Neon PostgreSQL
- React/Polaris admin frontend
- Theme app extension with 5 liquid blocks
- Hosted review form (not in-store, no theme requirement)
- `/api/cron/send-requests` endpoint for Railway cron

**Key differentiators vs Judge.me:**
- Widget builder with live preview
- 5 design-first themes vs Judge.me's generic defaults
- 1 review request email per order (Judge.me sends per product)
- Food/category-specific question templates
- AI summaries per product (Pro)

**Key differentiators vs Loox:**
- More granular design customisation
- Better price point (Starter $19 vs Loox $9.99 but with CSV import, custom questions)
- Food templates
- Starter includes custom widget design (Loox charges more for this)

SaltCore Group Limited | May 2026 | App 22
