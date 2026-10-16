# Cloudflare Void: how it works and how to implement it

Captured 2026-10-16. Audience: developers who need to understand Void before building on it or standing up a platform for their company.

> **Source caveat.** This note is built from search-result summaries of the sources below. The sandbox could not open the pages directly, and the Gemini share link (`https://share.gemini.google/xVCdBEL7peBk`) could not be read at all. Anything marked *(verify)* should be checked against the official docs at <https://void.cloud> before relying on it.

## 1. Why Void exists

Employees now "vibe-code" their own internal apps with AI tools. That is the new shadow IT: apps get deployed with no owner, no access control and no visibility. Cloudflare open-sourced Void (October 2026, after acquiring VoidZero, the company behind Vite, in June) so that:

- non-engineers and developers can build and deploy full-stack apps quickly, and
- IT keeps a central place to decide who can deploy, who can access what, and how many resources each app may use.

Void began as a commercial VoidZero product and became open source once Cloudflare no longer needed it to earn independent revenue.

## 2. The three parts

All three ship together as one npm package and Vite plugin (`npm install void`).

| Part | What it is | Who uses it |
|---|---|---|
| **Void SDK** | JS SDK for full-stack Vite apps: file-based API routing, pre-rendering, caching, end-to-end type safety from database to UI, automatic provisioning of bindings (D1, AI). | App developers |
| **Void CLI** | Provisioning and deployment tool. `void deploy` builds with Vite, applies DB migrations, provisions Cloudflare resources (e.g. D1) and deploys. Can target Cloudflare directly or a self-hosted Void Platform. | App developers |
| **Void Platform** | Self-hostable shared deployment platform built on **Workers for Platforms**. Gives IT admins control over users, project access, deployments and resource limits. | IT / platform admins |

## 3. How it fits together

```
Developer / employee
   │  npm install void   (SDK + Vite plugin)
   │  void connect <platform-url>
   │  void deploy
   ▼
Void CLI ── vite build ─► apply migrations ─► provision resources (D1, AI bindings)
   │
   ├──► Cloudflare account directly   (single-team use), or
   └──► Void Platform (self-hosted)   (company-wide, governed)
            ├─ Workers for Platforms  (each app is a user Worker)
            ├─ R2                     (storage)
            ├─ Analytics Engine       (usage/monitoring)  (verify)
            ├─ PostgreSQL             (platform's own state, not app data)
            └─ Login provider         (Cloudflare Access, Google, GitHub or OIDC)
```

Key idea: the person deploying never needs full Cloudflare account access. They authenticate to the Platform, and the Platform deploys on their behalf inside Workers for Platforms, where IT sets limits.

## 4. Developer workflow (building an app)

1. Create or open a Vite project and `npm install void`; add the Void Vite plugin to the Vite config.
2. Write the app using the SDK conventions: file-based API routes, pre-rendering and caching options, typed DB access. Resource usage (D1, AI) is detected from the code and bindings are created for you.
3. If your company runs a Void Platform: `void connect <platform URL>`, then sign in through the configured login provider.
4. `void deploy`. The CLI builds, migrates, provisions and deploys.

Exact SDK APIs, config file names and CLI flags were not available from the sources. Read the SDK guide on <https://void.cloud> *(verify)* before writing code against it.

## 5. Platform admin workflow (standing up Void Platform)

Prerequisites reported by the docs and coverage:

- Cloudflare account on a **paid Workers for Platforms** plan (from about $25/month plus usage)
- **R2** enabled
- **Analytics Engine** *(verify)*
- An empty hosted **PostgreSQL** database
- An account with your chosen **login provider** (Cloudflare Access, Google, GitHub or OIDC)
- A **domain** for the deployed apps

Then follow <https://void.cloud/guide/self-hosted-platform>. Admins manage users, project access and resource limits.

Not part of the core install, per the install guide: the user dashboard, GitHub builds and webhooks, build Containers, and custom project domains *(verify current status)*.

## 6. Governance gaps to plan around

- **No mandatory security review** before deployment. Add your own review step (for example a CI gate or a manual approval process) if you need one.
- **App owners can make their apps public.** If that is not acceptable, you need a policy or an admin-side check, since Void does not enforce it.
- Running the Platform is real infrastructure with real cost: a paid plan, R2, a database and a login provider all have to be maintained.

## 7. Open questions to resolve before adoption

- Does the core install include the admin dashboard, or only the user-facing pieces listed above as excluded? *(sources conflict)*
- How are per-app resource limits configured and enforced?
- How do the SDK's D1 and AI bindings behave under Workers for Platforms limits?
- What is the upgrade path between deploying straight to Cloudflare and deploying to a Platform?

## 8. Sources

- [Employees are vibe-coding their own apps. Cloudflare open-sources Void to give IT a handle on them (The New Stack)](https://thenewstack.io/cloudflare-voidzero-void-vibe-coding/)
- [Deploy with Void (Cloudflare blog)](https://blog.cloudflare.com/deploy-with-void/)
- [Install a Void Platform](https://void.cloud/guide/self-hosted-platform)
- [Void Platform prerequisites](https://void.cloud/guide/platform/installation/prerequisites)
- [Void Cloud](https://void.cloud/)
- [Void on Cloudflare (integration docs)](https://void.cloud/integrations/cloudflare)
- Not readable from this environment: Google share link to the New Stack article, and the Gemini share `https://share.gemini.google/xVCdBEL7peBk`.
