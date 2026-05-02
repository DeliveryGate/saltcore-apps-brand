# SaltAI Agent for Shopify — App Store Submission

## 1. App Name (max 30 chars)
SaltAI Agent for Shopify

## 2. Tagline (max 100 chars)
Shopify gave AI agents the keys to your store. AgentKit gives you the guardrails to use them safely.

## 3. Short Description (max 160 chars)
Give AI agents controlled access to your Shopify store. 11 guardrails, per-store write mutex, audit log, dry-run mode, and verified mode. Built for teams using AI in production.

## 4. Full Description

### Shopify gave AI agents the keys to your store. AgentKit gives you the guardrails to use them safely.

On April 9, 2026, Shopify launched the AI Toolkit —
giving Claude Code, Cursor, and Codex direct access to
your live Shopify store with no draft mode, no rollback,
and no guardrails.

That's powerful. And it's dangerous without the right
safety layer.

AgentKit is that safety layer.

When you run Claude Code on your Shopify store through
AgentKit, every operation goes through 13 guardrails
before touching your live store:

✓ Live theme protection — AI agents cannot write to your
  active theme without explicit confirmation
✓ Backup before overwrite — every file backed up before
  modification
✓ Regression check — if a change would break an existing
  section, the operation is blocked
✓ Link integrity — every URL verified before job complete
✓ Secrets scanner — no API keys or passwords ever written
  to theme files
✓ Read-only mode by default — all stores start in read-only
  until you explicitly allow writes
✓ Full audit log — every operation logged with timestamp,
  action, and result
✓ Dry run mode — test any operation without executing
✓ Financial confirmation guard — high-value operations
  require explicit confirmation
✓ Rate limit awareness — never hit Shopify API limits
✓ Schema validation — invalid data rejected before write
✓ Per-store policy isolation — settings per store for
  agencies managing multiple clients
✓ Verified mode — post-write readback confirms changes
  actually applied

The Shopify AI Toolkit opened the door.
AgentKit makes it safe to walk through.

**What SaltAI Agent lets AI agents do:**
- Read and write pages, blog posts, and theme assets
- List and create URL redirects
- Read products, collections, files, and metafields
- Update navigation menus
- Look up customer records (read-only)

**The 11 guardrails — every request passes all of them:**
1. **Rate limit** — monthly call limits by plan (free/builder/agency)
2. **Schema validation** — unknown resources or malformed params are rejected immediately
3. **Secrets scan** — blocks any request containing API keys, tokens, or credentials in params
4. **Read-only mode** — store-level flag that blocks all writes with one toggle
5. **Theme whitelist** — agents can only write to explicitly approved themes
6. **Live theme guard** — blocks writes to the role=main (live) theme unless explicitly overridden, with policy-level enforcement
7. **Store health check** — verifies session token and store reachability before executing
8. **Bulk write confirmation** — any bulk operation over 5 records requires `confirmed: true`
9. **Section name validation** — Shopify section names must be ≤25 chars
10. **Link integrity** — every URL written to content is verified to return HTTP 200 before the operation is marked complete
11. **Regression check** — diffs theme template sections before and after changes; blocks completion if any previously-rendering section is missing

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

Part of the SaltAI Agent platform at saltai.io.

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

Works alongside the free Shopify AI Toolkit.
Install AgentKit first, then connect your AI coding tool.

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
- Privacy policy: https://saltai.app/privacy/saltai-agent-shopify

---

## App Store Tags
`AI agents` `automation` `developer tools` `theme editing` `guardrails` `API` `LLM` `Claude` `GPT`

---

## App Details

| Field | Value |
|-------|-------|
| App URL | https://agentkit-production-3dee.up.railway.app |
| Privacy Policy URL | https://saltai.app/privacy/saltai-agent-shopify |
| Support Email | hello@saltai.app |
| Category | Developer tools → APIs and SDKs |

## Submission Checklist
- [ ] Shopify Partners app created at partners.shopify.com (requires interactive login as robert@saltcore.co.uk)
- [ ] client_id copied from Partners and updated in shopify.app.toml
- [ ] App submitted via `shopify app deploy` after Partners config complete
- [ ] Privacy policy live: https://saltai.app/privacy/saltai-agent-shopify
- [ ] Tagline confirmed: "Authenticated AI agent access to Shopify Admin API. By SaltAI."
- [ ] No false claims — dry-run, verified mode, 11 guardrails all built and verified

## Partner Dashboard
https://partners.shopify.com/2129821664/apps
(SaltAI Agent for Shopify — create new app, link to Railway service)
