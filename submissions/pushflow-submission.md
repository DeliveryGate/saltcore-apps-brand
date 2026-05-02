# PushFlow — Bulk Import, Export & Migrate — App Store Submission

## 1. App Name (max 30 chars)
PushFlow — Bulk Import & Export

## 2. Tagline (max 100 chars)
Import, export, update and migrate all your Shopify store data.

## 3. Short Description (max 160 chars)
The modern alternative to Matrixify. Bulk import and export products, customers, orders, blog posts, collections, redirects, metafields, and more.

## 4. Full Description

PushFlow handles bulk data management for Shopify merchants — import, export, update, and migrate products, customers, orders, blog posts, collections, redirects, metafields, and more.

Built for merchants who need to:
✓ Migrate from another platform to Shopify
✓ Bulk update thousands of products at once
✓ Import customers and their order history
✓ Manage content across multiple stores
✓ Automate recurring data updates via scheduled URL imports
✓ Export store data for analysis or backup

**14+ data types supported:**
Products · Custom Collections · Smart Collections · Customers · Blog Posts · Pages · Redirects · Orders · Draft Orders · Discounts · Metafields · Metaobjects · Navigation Menus · B2B Companies

**Flexible column mapping** — upload any CSV, JSON, or Excel format and map columns to Shopify fields. No rigid templates required. Auto-detects standard column names including Matrixify-compatible column headers.

**5 operation modes:** Create · Update · Merge (create-or-update) · Delete · Replace

**Full export functionality** — export any data type as CSV or JSON with optional filters.

**Validation engine** — validates data before processing: missing required fields, invalid emails, malformed URLs. Preview all issues before committing to import.

**Job history** — every import and export is tracked with row-level results. Download error reports and re-run failed rows.

**Scheduled jobs (Pro+)** — recurring imports from any URL on a configurable schedule.

**Pricing:**
Free: 10 items per data type
Starter ($29/month): 1,000 rows per job, all core data types
Pro ($49/month): 50,000 rows, all 14+ data types, 3 parallel jobs
Enterprise ($199/month): Unlimited rows, 10 parallel jobs, priority support

Support: hello@saltai.app
Privacy: https://saltai.app/privacy/pushflow

## 5. Support URL
https://saltai.app/support/pushflow

## 6. Privacy Policy URL
https://saltai.app/privacy/pushflow

## 7. Categories
Primary: Store management
Secondary: Marketing

## 8. Pricing Model
Freemium — Shopify recurring billing, 7-day free trial on all paid plans

## 9. Plans

### Free
- 10 rows per job
- Blog Posts, Pages, Redirects only
- Export included

### Starter — $29/month (7-day trial)
- 1,000 rows per job
- Products, Collections, Customers, Blog Posts, Pages, Redirects
- Scheduling, 1 parallel job

### Pro — $49/month (7-day trial)
- 50,000 rows per job
- All 14+ data types
- Scheduling, 3 parallel jobs

### Enterprise — $199/month (7-day trial)
- Unlimited rows
- 10 parallel jobs, priority support

## 10. App URL
https://shopify-pushflow-production.up.railway.app

## 11. Required Scopes
read_products, write_products, read_customers, write_customers,
read_orders, write_orders, read_draft_orders, write_draft_orders,
read_content, write_content, read_price_rules, write_price_rules,
read_discounts, write_discounts, read_redirects, write_redirects,
read_publications, write_publications, read_metaobjects, write_metaobjects,
read_files, write_files, read_shopify_payments_payouts, read_events

## 12. Build log
Rebuilt: 2 May 2026
- Full architecture rebuild: blog-posts-only → 14+ data types
- New Prisma schema: ImportJob, ImportResult, ExportJob, ImportTemplate, ScheduledJob
- Universal file parser: CSV (auto-delimiter detection), JSON, XLSX (SheetJS)
- Column mapper with auto-detection + Matrixify-compatible column names
- Validation engine with per-row error/warning reporting
- Products: multi-variant grouping, options, inventory level updates, SEO metafields
- Customers, Collections (custom + smart), Pages, Redirects importers
- Orders, Draft Orders, Discounts, Metafields importers
- Metaobjects, Menus, Files, Companies via GraphQL Admin API
- Universal exporter: all data types, CSV/JSON, filter support
- 3-step import wizard: type + upload → column mapping → live progress
- Job detail: retry-failed, error CSV download, per-row result view
- Billing v4: Shopify lineItems plan config, Free/Starter $29/Pro $49/Enterprise $199
