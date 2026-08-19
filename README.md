# Ascend GTM — Client Template

Skeleton repository for onboarding a new Ascend GTM client.
Use GitHub's **"Use this template"** button to create the client repo, then fill in
the placeholders described below.

## What's inside

```
.
├── config/
│   └── .env.example      # Environment-variable placeholders (copy to .env)
├── docs/                  # Client-specific runbooks, SOPs, meeting notes
├── scripts/               # One-off or recurring automation scripts
└── workflows/             # Workflow definitions (n8n exports, API sequences, etc.)
```

## Getting started

1. **Create a repo from this template** — click *Use this template → Create a new repository*.
   Name it `<client-slug>-ops` (e.g. `kahuna-ops`).

2. **Copy and fill environment variables**

   ```bash
   cp config/.env.example config/.env
   ```

   Edit `config/.env` with the values for this client's tenant.
   See the comments in `.env.example` for what each variable controls.

3. **Set the tenant slug** — update `ASCEND_TENANT_SLUG` in your `.env` so
   gateway calls and memZERO memory are scoped to this client.

4. **Add client docs** — drop runbooks, onboarding checklists, and SOPs into `docs/`.

5. **Add workflows** — export workflow JSON (e.g. from n8n) into `workflows/`;
   put helper scripts in `scripts/`.

## Architecture overview

Ascend GTM operations run through the **Ascend GTM Platform gateway**, which
proxies client API calls (HubSpot, Salesforce, Google Ads, GA4, etc.) with
secrets held server-side. Operators never handle raw API keys directly — the
gateway manages authentication and enforces dry-run-by-default for mutations.

Key components:

| Component | Purpose |
|---|---|
| **Gateway** (`api_proxy` / `nango_proxy`) | Proxied access to client SaaS APIs; secrets stay server-side |
| **memZERO** | Cross-session memory (semantic, episodic, procedural) scoped per tenant |
| **Tenant routing** | Work is scoped by `tenant:<slug>` — each client gets isolated config and memory |
| **Connectors** | GitHub, Gmail, Calendar, Slack, Vercel, Apollo, QuickBooks, Cloudflare, etc. |

## Repo conventions

- **No secrets in the repo.** Use `config/.env` (git-ignored) or the gateway's
  server-side secret store.
- **Tenant isolation.** One repo per client. Cross-client resources live in
  internal repos (e.g. `ascend-ops`).
- **Docs over tribal knowledge.** If you learn something about a client's setup,
  write it down in `docs/`.

## License

Private — Ascend GTM internal use only.
