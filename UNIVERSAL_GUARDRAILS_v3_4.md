# SaltCore: Universal Guardrails
**Version:** 3.4 (Updated 3 May 2026)
**Status:** Master document for all SaltCore projects
**Applies to:** Accounted, Balanced, Mellow, Scoped, Honesty, HTBH, LRAI, WowSee, Analysed, Closed, Clocked, Inspected, LevelSwitch, CloseGuard, and all future neart.ai products — AND all SaltCore Shopify apps: ReviewPulse, DeliveryIQ, BundleBox, QuoteFlow, FormFlow, LocalBooking, CorporateAccounts, TeamOrders, AllergenMatrix, BlogFlow, ReviewFlow, AgeGuard, PushFlow, SaltAI Subscriptions, MenuBuilder, SmartShop, FoodBlog, ProductFlow, SaltAI Agent (Xero), SaltAI Agent (Shopify), SafeServe, and all future DeliveryGate Shopify apps

---

## PREAMBLE

These guardrails are **non-negotiable rules** that Claude Code must follow on every build, every feature, every line of code. They prevent bugs, security issues, compliance failures, and technical debt.

**Save this document. Use it on every project. Update it as we learn.**

---

## SECTION 1: CODE QUALITY & TESTING

### Rule 1.1: Tests ALWAYS Pass Before Commit
### Rule 1.2: No Dead Code
### Rule 1.3: TypeScript Strict Mode
### Rule 1.4: ESLint & Prettier

---

## SECTION 2: SECURITY

### Rule 2.1: No Secrets in Code
### Rule 2.2: Encryption at Rest (PII/Sensitive)
### Rule 2.3: Encryption in Transit
### Rule 2.4: Authentication & Authorization
### Rule 2.5: Webhook Signature Verification
### Rule 2.6: Input Validation
### Rule 2.7: SQL Injection Prevention

---

## SECTION 3: DATA INTEGRITY & COMPLIANCE

### Rule 3.1: Immutable Audit Logs
### Rule 3.2: Soft Delete (Never Hard Delete)
### Rule 3.3: Data Retention Periods
### Rule 3.4: GDPR Compliance
### Rule 3.5: Employment Rights Act 2025 (Mellow-specific)
### Rule 3.6: Financial Data Integrity

---

## SECTION 4: DATABASE & SCHEMA

### Rule 4.1: Schema Documents Authority
### Rule 4.2: Indexing for Performance
### Rule 4.3: Migrations Are Tested
### Rule 4.4: No Sensitive Data in Logs

---

## SECTION 5: ERROR HANDLING & OBSERVABILITY

### Rule 5.1: Meaningful Error Messages
### Rule 5.2: Structured Logging
### Rule 5.3: Error Monitoring
### Rule 5.4: Performance Monitoring

---

## SECTION 6: DEVELOPMENT WORKFLOW

### Rule 6.1: Branch Strategy
### Rule 6.2: Commit Messages
### Rule 6.3: No Committing Broken Code

---

## SECTION 7: PLATFORM PATTERNS (Reuse These)

### Pattern 7.1: 2FA — @saltcore/auth
### Pattern 7.2: Subscription Billing — Paddle as MoR
### Pattern 7.3: Webhook Handling — signature first, async processing
### Pattern 7.4: Role-Based Access Control — @saltcore/auth

### Pattern 7.5: Telegram Bots — Two Per Product, Never Mixed ⛔

Every neart.ai product has exactly TWO Telegram bots:
1. **Product customer bot** — notifications TO customers only
2. **Product alerts bot** — system/CI/CD alerts TO Robert only (chat ID: 826120207)

**Rules:**
- ❌ NEVER send system alerts via the customer bot
- ❌ NEVER send customer notifications via the alerts bot
- ✅ All system alerts use `TELEGRAM_ALERTS_BOT_TOKEN`
- ✅ All customer messages use `TELEGRAM_CUSTOMER_BOT_TOKEN`
- ✅ All Telegram calls go through `@saltcore-hq/telegram`
- ❌ NEVER call `api.telegram.org` directly in product code
- ❌ NEVER fall back to customer bot token for system alerts

**Bot token env vars (standard across all products):**
```
TELEGRAM_ALERTS_BOT_TOKEN     — Robert's alerts bot (system/CI/CD)
TELEGRAM_CUSTOMER_BOT_TOKEN   — customer-facing bot
ALERT_CHAT_ID=826120207       — Robert's Telegram chat ID
```

**The failure this prevents:** PennyBot (Accounted customer bot) sent 500+
system incident alerts to Robert because devops-agent fell back to 
`TELEGRAM_BOT_TOKEN` (PennyBot) when `MOD045_TELEGRAM_BOT_TOKEN` wasn't set.
Customer saw system noise. Robert couldn't diagnose real issues.

### Pattern 7.6: Email Architecture — Two Systems, Never Mixed ⛔

**System 1 — @saltcore/auth:** signup verification, OTP, password reset, MFA
**System 2 — @saltcore-hq/email:** welcome, receipts, billing, product alerts

- ❌ NEVER import `postmark` directly in product code
- ❌ NEVER call `new postmark.ServerClient()` or `new ServerClient()` in product code
- ✅ All Postmark usage goes through `@saltcore-hq/email`

**Env vars required per product:**
```
POSTMARK_API_KEY    — Postmark server API key
POSTMARK_FROM       — verified sender (e.g. hello@imscoped.com)
POSTMARK_FROM_NAME  — sender name (e.g. Scoped) — never "Saltcore"
```

### Pattern 7.7: Machine-Readable Pricing — /pricing.md
### Pattern 7.8: Neon Read Replica — AI Database Queries

### Pattern 7.9: AI Model Routing — @saltcore/ai ⛔

- ❌ NEVER import `@anthropic-ai/sdk` directly in product code
- ❌ NEVER import `openai` directly in product code
- ✅ All AI text generation goes through `@saltcore/ai`
- ✅ Products declare tasks, not models
- ✅ Model strings live in `SALTCORE-AI-001` only

**Exception:** WowSee media pipeline (ElevenLabs, fal.ai, RunPod) uses own SDKs directly. Only text generation routes through `@saltcore/ai`.

**CI enforcement:** A grep check for `from '@anthropic-ai/sdk'` or `from 'openai'` in product code outside `packages/` must return zero results. Any result is a build-blocking violation.

### Pattern 7.10: Time Context Injection — @saltcore/time-context
### Pattern 7.11: AI Cost Control Discipline
### Pattern 7.12: Model Deprecation Protocol

### Pattern 7.13: Paddle Checkout Initialisation ⛔

The correct Paddle Billing initialisation pattern — used consistently across all products:

```javascript
// CORRECT — two separate calls, environment set first
if (process.env.NEXT_PUBLIC_PADDLE_ENVIRONMENT === 'sandbox') {
  window.Paddle.Environment.set('sandbox')
}
window.Paddle.Initialize({
  token: process.env.NEXT_PUBLIC_PADDLE_CLIENT_TOKEN,
})

// WRONG — environment passed inside Initialize (silently fails)
window.Paddle.Initialize({
  token: clientToken,
  environment: 'sandbox', // ← this does nothing in Paddle Billing
})
```

**CSP headers required for Paddle (all products):**
```
script-src: https://cdn.paddle.com https://sandbox-cdn.paddle.com https://public.profitwell.com
style-src: https://fonts.googleapis.com https://fonts.gstatic.com https://cdn.paddle.com https://sandbox-cdn.paddle.com
font-src: https://fonts.googleapis.com https://fonts.gstatic.com
connect-src: https://*.paddle.com
frame-src: https://*.paddle.com
```

**NEXT_PUBLIC_ variables must be declared in Dockerfile:**
```dockerfile
# In the builder stage — before RUN next build:
ARG NEXT_PUBLIC_PADDLE_CLIENT_TOKEN
ARG NEXT_PUBLIC_PADDLE_ENVIRONMENT
ARG NEXT_PUBLIC_APP_URL
# Add ALL NEXT_PUBLIC_ vars used in the product
ENV NEXT_PUBLIC_PADDLE_CLIENT_TOKEN=$NEXT_PUBLIC_PADDLE_CLIENT_TOKEN
ENV NEXT_PUBLIC_PADDLE_ENVIRONMENT=$NEXT_PUBLIC_PADDLE_ENVIRONMENT
ENV NEXT_PUBLIC_APP_URL=$NEXT_PUBLIC_APP_URL
```

Without ARG/ENV declarations, Railway passes build args that Docker silently ignores,
and NEXT_PUBLIC_ variables compile to `undefined` in the bundle regardless of Railway settings.

---

## SECTION 8: LAUNCHING TO PRODUCTION

### Rule 8.1: Pre-Launch Checklist
### Rule 8.2: Deployment
### Rule 8.3: Post-Launch Monitoring

---

## SECTION 9: DOCUMENTATION STANDARDS

### Rule 9.1: Code Comments
### Rule 9.2: API Documentation
### Rule 9.3: Architecture Decisions

---

## SECTION 10: MONITORING CLAUDE CODE

### Rule 10.1–10.7: Session governance, push rules, prohibited actions

---

## SECTION 11: FRONT-END WIRING & DATA PERSISTENCE

### Rule 11.1–11.6: Data persistence, no false success messages, button wiring

---

## SECTION 12: INTEGRATION & EXTERNAL API RULES

### Rule 12.1–12.8: HTTP calls, typed errors, timeouts, callbacks

---

## SECTION 13: HMRC & TAX-SPECIFIC RULES

### Rule 13.1–13.5: No hardcoded figures, UK date formats, zero tolerance

---

## SECTION 14: DEPLOYMENT, INFRASTRUCTURE & DATA INTEGRITY

### Rule 14.1–14.6: No hardcoded data, empty states, verification data

---

## SECTION 15: CLAUDE CODE EXECUTION RULES

### Rule 15.1: No Parallel Agent Delegation ⛔
### Rule 15.2–15.4: Verify output, one context, cross-reference

---

## SECTION 16: DEPLOYMENT, BRANCHING & MULTI-SERVICE ARCHITECTURE

### Rule 16.1–16.10: Branch strategy, lock files, pricing, geo-redirect

### Rule 16.11: New @saltcore/ Packages Must Be Added to Root Dockerfile ⛔

Every new `packages/[name]/` added to any monorepo must have its
`package.json` copied in the Dockerfile deps stage before first Railway deployment:

```dockerfile
COPY packages/[new-package]/package.json ./packages/[new-package]/
```

Learned from 8 consecutive Railway build failures on BUILD-SALTCORE-AI-001.

---

## SECTION 17: END-TO-END VERIFICATION & EVIDENCE

### Rule 17.1–17.7: Filesystem as ground truth, real inbox checks, synthetic failures

---

## SECTION 18: AUTHENTICATION & MIDDLEWARE VERIFICATION

### Rule 18.1–18.5: Railway URL verification, route classification, RBAC per role

---

## SECTION 19: MARKETING WEBSITE QUALITY STANDARDS

### Rule 19.1–19.6: Contrast, positioning language, no placeholder content

---

## SECTION 20: SUPPLY CHAIN HARDENING & THIRD-PARTY AI TOOL GOVERNANCE

### Rule 20.1–20.7: Pinned versions, frozen lockfile, third-party AI register

---

## SECTION 21: SEPARATION OF CONCERNS — PRE-LAUNCH CHECKLIST ⛔

*Added v3.3 — learned from SALTCORE-AUDIT-002 and SALTCORE-AUDIT-003 (April 2026).
The ecosystem audit found systematic violations across all 8 products — custom JWT stacks,
direct SDK imports, raw Postmark calls, custom webhook crypto — all gaps that got through
because there was no mandatory pre-launch check.*

No product is considered launch-ready until it passes all 8 checks below.
This checklist must be run by an independent verification agent — not the same agent
that did the building. The verification agent greps the actual code, not the session report.

### Pre-Launch Separation of Concerns Matrix

Run these 7 grep checks. Every check must return ✅ Clean before launch:

```bash
# CHECK 1 — Webhook verification (no raw crypto)
grep -r "crypto.createHmac.*paddle\|webhooks.unmarshal" \
  [repo] --include="*.ts" --include="*.tsx" \
  --exclude-dir="node_modules" --exclude-dir=".next"
# Must return NOTHING

# CHECK 2 — Direct AI SDK imports
grep -r "from '@anthropic-ai/sdk'\|from 'openai'\|import Anthropic\|import OpenAI" \
  [repo]/src [repo]/app [repo]/apps \
  --include="*.ts" --include="*.tsx" \
  --exclude-dir="node_modules" --exclude-dir=".next" --exclude-dir="packages"
# Must return NOTHING

# CHECK 3 — Custom JWT/bcrypt (not via @saltcore/auth)
grep -r "from 'jsonwebtoken'\|from 'bcryptjs'\|import jwt\|import bcrypt" \
  [repo]/src [repo]/app [repo]/apps \
  --include="*.ts" --include="*.tsx" \
  --exclude-dir="node_modules" --exclude-dir=".next" --exclude-dir="packages"
# Must return NOTHING

# CHECK 4 — Rate limiting on auth endpoints
grep -r "withRateLimit\|rateLimit" \
  [repo] --include="*.ts" --exclude-dir="node_modules" --exclude-dir=".next"
# Must be PRESENT

# CHECK 5 — Raw Postmark imports
grep -r "new postmark.ServerClient\|new ServerClient\|from 'postmark'" \
  [repo]/src [repo]/app [repo]/apps \
  --include="*.ts" --include="*.tsx" \
  --exclude-dir="node_modules" --exclude-dir=".next" --exclude-dir="packages"
# Must return NOTHING

# CHECK 6 — Raw Telegram fetch calls
grep -r "api.telegram.org" \
  [repo]/src [repo]/app [repo]/apps \
  --include="*.ts" --include="*.tsx" \
  --exclude-dir="node_modules" --exclude-dir=".next" --exclude-dir="packages"
# Must return NOTHING

# CHECK 7 — Paddle webhook package present
grep -r "verifyPaddleWebhook\|isPaddleEventDuplicate" \
  [repo] --include="*.ts" --include="*.tsx" --exclude-dir="node_modules"
# Must be PRESENT (or N/A if product has no Paddle webhooks)
```

### Rules:
- ✅ All 7 checks must pass before launch
- ✅ Verification must be run by a separate Claude Code agent from the builder
- ✅ Results committed to `~/Projects/saltcore-hq/build-logs/[PRODUCT]-PRE-LAUNCH-VERIFY.md`
- ❌ NEVER mark a product launch-ready based on the build agent's own report
- ❌ NEVER skip the verification agent to save time

---

## SECTION 22: HEALTH ENDPOINT STANDARD ⛔

*Added v3.3 — learned from SALTCORE-OPS-001 (April 2026). MOD-045 sent 500+ false
"unreachable" alerts because health endpoints returned 503 for degraded AI/queue state,
not just for actual downtime. Health endpoints must be liveness probes only.*

Every product must expose `GET /api/health` with these exact properties:

**Requirements:**
- ✅ Public — no authentication required
- ✅ Listed in `PUBLIC_ROUTES` in `middleware.ts`
- ✅ Returns HTTP 200 when service is up (DB responds)
- ✅ Returns HTTP 503 ONLY when database is unreachable
- ✅ Never returns 503 for degraded AI, queue, or external provider state
- ✅ Standard JSON response format (see below)

**Standard response format:**
```typescript
// GET /api/health
export async function GET(): Promise<NextResponse> {
  const start = Date.now()
  try {
    await prisma.$queryRaw`SELECT 1`
    return NextResponse.json({
      status: 'ok',
      product: process.env.NEXT_PUBLIC_PRODUCT_NAME ?? 'unknown',
      environment: process.env.NODE_ENV ?? 'unknown',
      timestamp: new Date().toISOString(),
      latency_ms: Date.now() - start,
      checks: { database: 'ok' }
    }, { status: 200 })
  } catch (error) {
    return NextResponse.json({
      status: 'error',
      product: process.env.NEXT_PUBLIC_PRODUCT_NAME ?? 'unknown',
      timestamp: new Date().toISOString(),
      error: error instanceof Error ? error.message : 'Unknown error',
      checks: { database: 'error' }
    }, { status: 503 })
  }
}
```

**The rule:** `/api/health` is a **liveness probe** — is the service alive?
It is NOT a **readiness probe** — is every feature fully operational?
Degraded AI, slow queues, and third-party provider issues are NOT reasons to return 503.

---

## SECTION 23: INTERNAL METRICS ENDPOINT STANDARD ⛔

*Added v3.3 — per SALTCORE-ADMIN-001 v1.2. MOD-045 and the saltcore-ops dashboard
require a consistent authenticated metrics endpoint on every product.*

Every product must expose `GET /api/internal/metrics`:

**Requirements:**
- ✅ Authenticated with `INTERNAL_SERVICE_KEY` header (`x-internal-key`)
- ✅ Returns HTTP 401 if key missing or wrong
- ✅ Returns HTTP 503 if `INTERNAL_SERVICE_KEY` env var not set
- ✅ Returns product-specific metrics in standard format
- ✅ `INTERNAL_SERVICE_KEY` must be set in Railway for all 8 products
- ✅ Same key value across all products (shared secret)

**Required env var:**
```
INTERNAL_SERVICE_KEY=<32-byte hex string generated with openssl rand -hex 32>
```

**Standard response format:**
```typescript
export async function GET(req: NextRequest): Promise<NextResponse> {
  const key = req.headers.get('x-internal-key')
  if (!process.env.INTERNAL_SERVICE_KEY) {
    return NextResponse.json({ error: 'INTERNAL_SERVICE_KEY not configured' }, { status: 503 })
  }
  if (key !== process.env.INTERNAL_SERVICE_KEY) {
    return NextResponse.json({ error: 'Unauthorized' }, { status: 401 })
  }
  return NextResponse.json({
    product: process.env.NEXT_PUBLIC_PRODUCT_NAME,
    timestamp: new Date().toISOString(),
    metrics: { /* product-specific */ },
    status: 'ok'
  })
}
```

---

## SECTION 24: MOD-045 ALERTING STANDARDS ⛔

*Added v3.3 — learned from 500+ false positive Telegram alerts (April 2026).*

### Rule 24.1: Failure Threshold Before Alerting

MOD-045 must require **3 consecutive failures** before sending any Telegram alert.
A single failure may be a Railway cold start or transient network issue.

```typescript
// Correct pattern:
const FAILURE_THRESHOLD = 3
if (consecutiveFailures >= FAILURE_THRESHOLD) {
  await alertError(product, message, context)
  resetFailureCount(product)
}
```

### Rule 24.2: Alert Message Must Include Full Context

Every MOD-045 alert must include:
- Product name
- What was checked (URL)
- What failed (HTTP status or error message)
- Consecutive failure count
- Timestamp

```typescript
// Correct alert format:
await alertError(
  productName,
  `Health check failed (${consecutiveFailures}/${FAILURE_THRESHOLD})`,
  `URL: ${url}/api/health\nStatus: ${httpStatus}\nError: ${errorMessage}\nTime: ${new Date().toISOString()}`
)
```

### Rule 24.3: MOD-045 Product Registry Must Be Current

MOD-045 must have the correct Railway URL for every product.
Wrong URLs cause permanent false positives.

**Current authoritative URL list:**
```typescript
const PRODUCTS = [
  { name: 'Scoped', url: 'https://scoped-production.up.railway.app' },
  { name: 'Accounted', url: 'https://accounted-production.up.railway.app' },
  { name: 'Analysed', url: 'https://analysed-production.up.railway.app' },
  { name: 'Mellow', url: 'https://mellow-production-351f.up.railway.app' },
  { name: 'WowSee', url: 'https://wowsee-production.up.railway.app' },
  { name: 'Inspected', url: 'https://inspected-production.up.railway.app' },
  { name: 'OwnPay', url: 'https://ownpay-production.up.railway.app' },
  { name: 'Clocked UK', url: 'https://timetrack-production-3c7e.up.railway.app' },
  { name: 'Clocked International', url: 'https://astonishing-growth-production-fdbc.up.railway.app' },
]
```

When a product's Railway URL changes — update MOD-045 registry in the same commit.

### Rule 24.4: Alerts Bot Never Customer Bot

MOD-045 and all system monitoring must use `TELEGRAM_ALERTS_BOT_TOKEN`.
If this env var is not set — log to console and throw, do not fall back to any other token.

---

## SECTION 25: DOCKERFILE PRE-FLIGHT ⛔

*Added v3.4 — learned from two consecutive Railway boot crashes on ReviewPulse (May 2026).
Crash 1: macOS Prisma binaries overwrote Alpine-built client (missing .dockerignore).
Crash 2: npx contaminated root package.json with a conflicting @prisma/client version.
Both crashes passed `git push` successfully and only failed at runtime on Railway.
These rules prevent the entire class of platform-mismatch and version-conflict Docker failures.*

**Applies to:** ALL apps with a Dockerfile — both neart.ai SaaS products and Shopify apps.

### Rule 25.1: `.dockerignore` Must Exclude `**/node_modules` ⛔

Every repo with a Dockerfile must have a `.dockerignore` that excludes node_modules.

```
# .dockerignore — minimum required entries
**/node_modules
**/.git
**/dist
```

**Why:** `COPY . .` without this overwrites Docker-built platform-specific binaries
(Prisma client, native addons) with your local macOS-compiled versions. Alpine cannot
execute macOS binaries. The error presents at runtime, not build time.

**Checklist before every Dockerfile commit:**
```bash
cat .dockerignore | grep node_modules  # must return a result
```

### Rule 25.2: `prisma generate` Must Run AFTER `COPY . .`, Never Before ⛔

```dockerfile
# CORRECT order:
COPY . .                                                          # FIRST
RUN cd web && npx prisma generate --schema=../prisma/schema.prisma  # THEN generate

# WRONG order:
RUN cd web && npx prisma generate --schema=../prisma/schema.prisma  # generates for alpine
COPY . .                                                          # OVERWRITES with macOS binary
```

**Why:** If `prisma generate` runs before `COPY . .`, it builds the correct Alpine binary —
then `COPY . .` overwrites it with your local macOS binary. The fix is one line swap,
but the failure takes 10+ minutes of Railway debug to diagnose.

### Rule 25.3: Schema Migrations Must Run in `CMD`, Not `RUN` ⛔

```dockerfile
# CORRECT — migration runs at container startup against live DATABASE_URL
CMD cd web && npx prisma db push --schema=../prisma/schema.prisma --skip-generate && node index.js

# WRONG — migration runs at build time, may target wrong DB or fail silently
RUN cd web && npx prisma db push --schema=../prisma/schema.prisma
CMD node index.js
```

**Why:** `RUN` executes at build time when `DATABASE_URL` may not be set, may point to
a dev database, or may use a connection pooler that rejects schema changes (Neon/PgBouncer
requires `DIRECT_URL` for migrations). Migrations in `CMD` run against the live environment's
database at startup, which is always correct.

**Neon/PgBouncer note:** If `DATABASE_URL` points to a pooled connection, `db push` will fail.
Set `DIRECT_URL` in Railway and reference it:
```dockerfile
CMD DATABASE_URL=$DIRECT_URL npx prisma db push ... && node index.js
```

### Rule 25.4: Never Run `npx --yes` From a Directory That Owns a `package.json` You Don't Control ⛔

```bash
# DANGEROUS — run from wrong directory
cd /app && npx --yes prisma@5.20.0 db push  # adds @prisma/client to /app/package.json

# SAFE — explicitly reference the correct binary
cd /app/web && npx --yes prisma@5.20.0 db push  # adds to web/package.json (correct)
# OR better — add prisma as a devDependency in the correct package.json and use:
cd /app/web && ./node_modules/.bin/prisma db push
```

**Why:** `npx --yes [pkg]@[version]` installs the package into the nearest `package.json`
it can write to. If the working directory is the repo root, it contaminates root
`package.json`. On the next Docker build, `npm install` at the root installs a second
copy of `@prisma/client` at a conflicting version. Prisma fails with:
*"Please make sure they have the same version"*.

### Rule 25.5: Root `package.json` Must Only Contain Root-Level Dependencies ⛔

In any project with a root `package.json` and a nested app `package.json` (e.g. `web/`):

- ✅ Root `package.json` declares only packages the root install actually needs
- ❌ NEVER add `@prisma/client`, Shopify packages, or app-level tools to root `package.json`
  unless the root Dockerfile step explicitly uses them
- ❌ NEVER commit a root `package-lock.json` that was generated by accident

**Verify after any `npx --yes` or manual `npm install` run from the repo root:**
```bash
cat package.json | grep -v dotenv | grep -v engines | grep -v scripts | grep "dependencies"
# Should return only packages you intentionally added at root level
```

**Recovery if contaminated:**
1. Remove the accidental entries from root `package.json`
2. Delete root `package-lock.json` (the root install step will regenerate it correctly)
3. Commit both changes before pushing to Railway

### Rule 25.6: Install Required Alpine OS Packages Before Any npm Steps ⛔

```dockerfile
# CORRECT — OS packages first
FROM node:18-alpine
RUN apk add --no-cache openssl    # required by Prisma on Alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
```

**Known Alpine requirements:**
| Package         | Requires               |
|-----------------|------------------------|
| @prisma/client  | openssl                |
| sharp           | vips-dev, ffi-dev      |
| canvas          | cairo-dev, pango-dev   |
| bcrypt          | python3, make, g++     |

**Symptom:** `Cannot find required binaries` or `Error: /lib/x86_64-linux-gnu/libc.so.6` at runtime.
**Diagnosis:** Missing `apk add` for a native dependency.
**Rule:** If you add a package with native binaries, check its Alpine requirements before the next Railway deploy.

### Rule 25.7: Build the Dockerfile Locally Before Pushing Any Change to It ⛔

Before pushing any commit that:
- Modifies the Dockerfile
- Adds a new Prisma model
- Adds a package with native binaries
- Changes Node.js or npm version

Run:
```bash
docker build -t [app]-preflight . 2>&1 | tail -30
# Must exit 0 with no errors
```

**Why:** A failed Railway build costs 5+ minutes of feedback loop. A failed local build
costs 20 seconds. There is no reason to discover a Dockerfile syntax error or missing
`apk add` on Railway.

**If Docker is not available locally:** Run `docker build` on a branch before merging to main.
Do not merge a Dockerfile change to main that has never been built.

---

## HOW TO USE THIS DOCUMENT

1. **Save to every repo root:** `UNIVERSAL_GUARDRAILS_v3_4.md`
2. **Reference at project start:** "Claude Code, read UNIVERSAL_GUARDRAILS_v3_4.md"
3. **Add product-specific rules** in separate doc

**Starting an autonomous session:**
```
Claude, read UNIVERSAL_GUARDRAILS_v3_4.md. Confirm you have read:
- Section 7 patterns 7.1–7.13 (note: 7.13 is NEW — Paddle checkout pattern)
- Section 10 rules 10.1–10.7
- Section 14 rules 14.2–14.6
- Section 15 rules 15.1–15.4
- Section 16 rules 16.1–16.11
- Section 17 rules 17.1–17.7
- Section 18 rules 18.1–18.5
- Section 19 rules 19.1–19.6
- Section 20 rules 20.1–20.7
- Section 21 — Separation of Concerns Pre-Launch Checklist
- Section 22 — Health Endpoint Standard
- Section 23 — Internal Metrics Endpoint Standard
- Section 24 — MOD-045 Alerting Standards
- Section 25 — Dockerfile Pre-Flight (NEW — 7 rules, mandatory for all apps with Dockerfile)
Then declare your session scope before writing any code.
```

---

**These guardrails are non-negotiable. They protect your products, your customers, and your business.**

**Last updated:** 3 May 2026
**Next review:** 1 June 2026

**Changelog:**
- v3.4 (3 May 2026): Added **Section 25 — Dockerfile Pre-Flight** (7 mandatory rules covering .dockerignore, prisma generate ordering, migration placement in CMD not RUN, npx contamination prevention, root package.json hygiene, Alpine OS package requirements, and local pre-push build verification). Updated "Applies to:" to cover all 22 SaltCore Shopify apps in addition to existing neart.ai products. Learned from: two consecutive Railway boot crashes on ReviewPulse (May 2026) — Crash 1: missing .dockerignore caused macOS Prisma binaries to overwrite Alpine client; Crash 2: npx --yes from wrong directory contaminated root package.json with conflicting @prisma/client version. Both crashes passed git push and only failed at container startup.
- v3.3 (27 April 2026): Added **Section 21 — Separation of Concerns Pre-Launch Checklist** (7-check mandatory verification matrix, independent agent required, no product launches without passing all 7). Added **Section 22 — Health Endpoint Standard** (liveness probe only, DB check only, never 503 for degraded AI/queue). Added **Section 23 — Internal Metrics Endpoint Standard** (INTERNAL_SERVICE_KEY auth, standard format, required on all 8 products). Added **Section 24 — MOD-045 Alerting Standards** (3-failure threshold, full context in alerts, correct URL registry, alerts bot never customer bot). Added **Pattern 7.13 — Paddle Checkout Initialisation** (Environment.set() before Initialize(), CSP headers required, NEXT_PUBLIC_ ARG/ENV in Dockerfile mandatory). Strengthened Pattern 7.5 (Telegram bots) and Pattern 7.6 (email) with explicit prohibition on direct imports. Learned from: SALTCORE-AUDIT-002, SALTCORE-AUDIT-003, SALTCORE-OPS-001, two independent verification passes finding 10 remaining violations after first audit claimed completion.
- v3.2 (25 April 2026): Added Rule 16.11 — New @saltcore/ packages must be added to root Dockerfile deps stage.
- v3.1 (20 April 2026): Added Section 17, Section 20, Patterns 7.10–7.12.
- v3.0 (10 April 2026): Added Section 18, Section 19.
