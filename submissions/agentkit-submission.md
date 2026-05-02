# AgentKit for Shopify — App Store Submission

## 1. App Name (max 30 chars)
AgentKit for Shopify

## 2. Tagline (max 100 chars)
A safe, guardrailed bridge between AI agents and your Shopify store.

## 3. Short Description (max 160 chars)
Give AI agents controlled access to your Shopify store. 9 guardrails, per-store write mutex, audit log, dry-run mode, and verified mode. Built for teams using AI in production.

## 4. Full Description

### AI agents can now write to your Shopify store — safely.

AgentKit for Shopify is a backend bridge that lets AI agents (Claude, GPT-4, custom LLMs) read and write Shopify data through a secure, guardrailed API. Every action passes through 9 independent safety checks before execution. Every write is logged. Nothing happens without your control.

**What AgentKit lets AI agents do:**
- Read and write pages, blog posts, and theme assets
- List and create URL redirects
- Read products, collections, files, and metafields
- Update navigation menus
- Look up customer records (read-only)

**The 9 guardrails — every request passes all of them:**
1. **Rate limit** — monthly call limits by plan (free/builder/agency)
2. **Schema validation** — unknown resources or malformed params are rejected immediately
3. **Secrets scan** — blocks any request containing API keys, tokens, or credentials in params
4. **Read-only mode** — store-level flag that blocks all writes with one toggle
5. **Theme whitelist** — agents can only write to explicitly approved themes
6. **Live theme guard** — blocks writes to the role=main (live) theme unless explicitly overridden, with policy-level enforcement
7. **Store health check** — verifies session token and store reachability before executing
8. **Bulk write confirmation** — any bulk operation over 5 records requires `confirmed: true`
9. **Section name validation** — Shopify section names must be ≤25 chars

**Advanced features:**
- **Dry-run mode** (`params.dryRun: true`): validates all guardrails but skips execution — test before you touch anything
- **Verified mode** (`params.verifiedMode: true`): reads back the value after a write and confirms it matches — proof the write worked
- **Per-store write mutex**: concurrent writes to the same store are serialised automatically
- **Theme backup**: every theme asset overwrite is backed up to DB before execution
- **Conflict detection**: `lastKnownAt` param prevents overwriting changes made after your last read
- **Audit log**: every action (allowed or blocked) is timestamped and stored
- **Agent health endpoint** (`GET /api/health/:shop`): 8-check doctor report for your agent's store connection

**What this is NOT:**
- Not a no-code automation tool
- Not a Shopify Flow replacement
- Not a public-facing customer app — this is a developer/AI-team tool

---

## 3 Key Benefits

**1. Your AI agent can edit Shopify — without you watching every command**
The guardrail stack means your agent can't accidentally write to your live theme, exceed rate limits, inject secrets, or bulk-overwrite records without confirmation. You define what's allowed at configuration time.

**2. Full audit trail — every action logged**
Every request — whether allowed or blocked — is recorded with timestamp, resource, action, params, and outcome. When something goes wrong, you have the evidence to understand exactly what happened.

**3. Dry-run and verified mode — confidence before and after**
Run any command in dry-run mode to validate all guardrails without touching the store. Use verified mode to read back after writes and confirm the data landed correctly. Built for production AI workflows.

---

## Pricing Tiers

| Plan | Price | Calls/Month | Features |
|------|-------|-------------|----------|
| **Free** | $0/month | 100 | All guardrails, audit log, dry-run, verified mode |
| **Builder** | $49/month | 10,000 | Everything in Free + higher rate limit, email support |
| **Agency** | $149/month | Unlimited | Everything in Builder + unlimited calls, priority support |

---

## App Category
**Developer tools** → APIs and SDKs

## Required Permissions
- `read_themes`, `write_themes` — theme asset operations
- `read_content`, `write_content` — pages and blog posts
- `read_products` — product catalogue access
- `read_customers` — customer lookups (read-only)
- `read_online_store_navigation`, `write_online_store_navigation` — navigation updates
- `write_url_redirects`, `read_url_redirects` — redirect management
- `read_metaobjects`, `write_metaobjects` — metafield operations

## Support
- Support email: hello@saltai.app
- Privacy policy: https://saltai.app/privacy/agentkit

---

## App Store Tags
`AI agents` `automation` `developer tools` `theme editing` `guardrails` `API` `LLM` `Claude` `GPT`

---

## App Details

| Field | Value |
|-------|-------|
| App URL | https://agentkit-production-3dee.up.railway.app |
| Privacy Policy URL | https://saltai.app/privacy/agentkit |
| Support Email | hello@saltai.app |
| Category | Developer tools → APIs and SDKs |

## Submission Checklist
- [ ] Shopify Partners app created at partners.shopify.com (requires interactive login as robert@saltcore.co.uk)
- [ ] client_id copied from Partners and updated in shopify.app.toml
- [ ] App submitted via `shopify app deploy` after Partners config complete
- [ ] Privacy policy live: https://saltai.app/privacy/agentkit
- [ ] Tagline confirmed: "A safe, guardrailed bridge between AI agents and your Shopify store."
- [ ] No false claims — dry-run, verified mode, 9 guardrails all built and verified

## Partner Dashboard
https://partners.shopify.com/2129821664/apps
(AgentKit for Shopify — create new app, link to Railway service)
