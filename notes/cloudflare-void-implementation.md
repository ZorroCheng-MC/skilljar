# Cloudflare Void: how it works and how to implement it

Captured 2026-10-16. Audience: developers who need to understand Void before building apps on it, and IT admins who will run the platform behind it.

Primary source: [Employees are vibe-coding their own apps. Cloudflare open-sources Void to give IT a handle on them (The New Stack)](https://thenewstack.io/cloudflare-voidzero-void-vibe-coding/)

> **Source caveat.** The New Stack page could not be opened from this sandbox, so the background sections (1–3, 8–9) come from search-result summaries of it and of Cloudflare's "Deploy with Void" post. The step-by-step walkthrough (section 4) was supplied by the note's requester and has **not** been checked against the official Void docs. Exact package, import and command names (`voidPlugin`, `void/db`, `void init`, `void login`) should be confirmed at <https://void.cloud> before you build on them. A Gemini share link was also provided but could not be read.

## 1. Why Void exists

Employees now "vibe-code" their own internal apps with AI tools. That is the new shadow IT: apps get deployed with no owner, no access control and no visibility. Cloudflare open-sourced Void in October 2026 (after acquiring VoidZero, the company behind Vite, in June) so that:

- non-engineers and developers can build and deploy full-stack apps quickly, and
- IT keeps one place to decide who can deploy, who can access what, and how many resources each app may use.

Void began as a commercial VoidZero product and became open source once Cloudflare no longer needed it to earn independent revenue.

## 2. The three parts

All three ship together as one npm package and Vite plugin.

| Part | What it is | Who uses it |
|---|---|---|
| **Void SDK** | JS SDK for full-stack Vite apps: file-based API routing, pre-rendering, caching, end-to-end type safety from database to UI, automatic provisioning of bindings (D1, AI). | App developers |
| **Void CLI** | Provisioning and deployment tool. `void deploy` builds with Vite, applies DB migrations, provisions Cloudflare resources and deploys, either to Cloudflare directly or to a self-hosted Void Platform. | App developers |
| **Void Platform** | Self-hostable shared deployment platform ("control plane") built on **Workers for Platforms**. Gives IT admins control over users, project access, deployments and resource limits. | IT / platform admins |

## 3. Core idea: resource inference

Void infers the resources an app needs from its code. Developers do not hand-write Wrangler bindings and do not ask IT for Cloudflare credentials.

- The Vite plugin intercepts backend resource imports at build time and packages the app for the Workers runtime.
- Importing `void/db` tells Void the app needs a Cloudflare D1 database. It is created and bound automatically at deploy time.
- Locally, Void simulates D1 and the Workers runtime, so `npm run dev` needs no Cloudflare account.
- The developer only talks to the company's Void Platform, which holds the real Cloudflare access.

```
Developer / employee
   │  npm install -D void        (SDK + Vite plugin)
   │  void connect <platform-url>
   │  void login                 (company SSO)
   │  void deploy
   ▼
Void CLI ── vite build ─► detect void/db import ─► create D1 + run migrations
   │
   ├──► Cloudflare account directly   (single-team use), or
   └──► Void Platform (self-hosted)   (company-wide, governed)
            ├─ Workers for Platforms  (each app is an isolated user Worker)
            ├─ R2                     (storage)
            ├─ Analytics Engine       (usage/monitoring)   (verify)
            ├─ PostgreSQL             (platform's own state, not app data)
            └─ Login provider         (Cloudflare Access, Google, GitHub or OIDC)
```

## 4. Implementation walkthrough (example: internal request tracker)

*Supplied walkthrough, not independently verified. See the source caveat.*

### 4.1 Initialise and install

```bash
# New project (templates for React, Vue, Svelte, Solid, ...)
npx void init my-internal-tool
cd my-internal-tool
npm install
```

To add Void to an existing Vite project instead:

```bash
npm install -D void
```

### 4.2 Configure the Vite plugin (`vite.config.ts`)

```ts
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import { voidPlugin } from 'void';

export default defineConfig({
  plugins: [
    react(),
    voidPlugin() // intercepts backend resource imports at build time and bundles for the Workers runtime
  ]
});
```

### 4.3 Define the database schema (`src/db/schema.ts`)

Void follows Drizzle conventions, which keeps types end-to-end from database to UI.

```ts
import { sqliteTable, text, integer } from 'drizzle-orm/sqlite-core';

export const requests = sqliteTable('requests', {
  id: integer('id').primaryKey({ autoIncrement: true }),
  title: text('title').notNull(),
  status: text('status').default('pending').notNull(),
  createdAt: integer('created_at', { mode: 'timestamp' }).$defaultFn(() => new Date())
});
```

### 4.4 Implement the backend API (`src/routes/api/requests.ts`)

Void runs the backend on Hono internally, with file-based routing. Importing `void/db` is what makes Void infer that D1 is needed and provision and bind it on deploy.

```ts
import { db } from 'void/db';
import { requests } from '../../db/schema';

export async function GET() {
  // Local SQLite in development, Cloudflare D1 in production
  const allRequests = await db.select().from(requests);
  return Response.json(allRequests);
}

export async function POST(req: Request) {
  const body = await req.json();
  const [created] = await db.insert(requests).values({
    title: body.title
  }).returning();

  return Response.json(created, { status: 201 });
}
```

### 4.5 Call it from the frontend (`src/App.tsx`)

The frontend stays a normal React/Vite app that calls the API with `fetch`.

```tsx
import { useEffect, useState } from 'react';

interface RequestItem {
  id: number;
  title: string;
  status: string;
}

export default function App() {
  const [list, setList] = useState<RequestItem[]>([]);
  const [title, setTitle] = useState('');

  const fetchItems = () => {
    fetch('/api/requests')
      .then(res => res.json())
      .then(setList);
  };

  useEffect(() => {
    fetchItems();
  }, []);

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    if (!title) return;
    await fetch('/api/requests', {
      method: 'POST',
      body: JSON.stringify({ title }),
      headers: { 'Content-Type': 'application/json' }
    });
    setTitle('');
    fetchItems();
  };

  return (
    <div style={{ padding: 24, fontFamily: 'sans-serif' }}>
      <h2>Internal Request Tracker</h2>
      <form onSubmit={handleSubmit}>
        <input
          value={title}
          onChange={e => setTitle(e.target.value)}
          placeholder="Enter a request..."
        />
        <button type="submit">Submit</button>
      </form>
      <ul>
        {list.map(item => (
          <li key={item.id}>{item.title} ({item.status})</li>
        ))}
      </ul>
    </div>
  );
}
```

### 4.6 Develop locally and deploy to the managed platform

**Local development.** Void emulates the D1 database and the Workers runtime locally:

```bash
npm run dev
```

**Connect and deploy.** Developers need no Cloudflare API token or dashboard access, only the URL of the company's Void Platform:

```bash
# 1. Connect to the company's Void control plane (hosted by IT)
npx void connect https://void.internal.company.com

# 2. Authenticate (redirects to company SSO: Google, GitHub, OIDC or Cloudflare Access)
npx void login

# 3. Deploy
npx void deploy
```

**What happens behind the scenes on `void deploy`:**

1. The CLI scans the project, detects the `void/db` import, and creates a dedicated D1 database in IT's Workers for Platforms tenant, then runs the schema migration.
2. Frontend static assets and backend code are packaged into an isolated Worker.
3. The company's default quotas (CPU time, memory, allowed access domains) are applied, and a controlled preview or production URL is returned.

## 5. Platform admin workflow (standing up Void Platform)

Prerequisites reported by the docs and press coverage:

- Cloudflare account on a **paid Workers for Platforms** plan (from about $25/month plus usage)
- **R2** enabled
- **Analytics Engine** *(verify)*
- An empty hosted **PostgreSQL** database
- An account with your chosen **login provider** (Cloudflare Access, Google, GitHub or OIDC)
- A **domain** for the deployed apps

Follow <https://void.cloud/guide/self-hosted-platform>. Admins manage users, project access and resource limits.

Reported as **not** part of the core install: the user dashboard, GitHub builds and webhooks, build Containers, and custom project domains *(verify current status)*.

## 6. Governance gaps to plan around

- **No mandatory security review** before deployment. Add your own gate (a CI check or manual approval) if you need one.
- **App owners can make their apps public.** If that is not acceptable, you need a policy or an admin-side check, since Void does not enforce it.
- Running the Platform is real infrastructure with real cost: a paid plan, R2, a database and a login provider all have to be maintained.

## 7. Open questions to resolve before adoption

- Does the core install include the admin dashboard, or only the pieces listed above as excluded? *(sources conflict)*
- How are the default per-app quotas configured and enforced?
- How do D1 and AI bindings behave under Workers for Platforms limits?
- What is the upgrade path between deploying straight to Cloudflare and deploying to a Platform?
- Do the names in section 4 (`voidPlugin`, `void/db`, `void init`, `void login`, file-based routes under `src/routes/api/`) match the released SDK?

## 8. Sources

- [Employees are vibe-coding their own apps. Cloudflare open-sources Void to give IT a handle on them (The New Stack)](https://thenewstack.io/cloudflare-voidzero-void-vibe-coding/)
- [Deploy with Void (Cloudflare blog)](https://blog.cloudflare.com/deploy-with-void/)
- [Install a Void Platform](https://void.cloud/guide/self-hosted-platform)
- [Void Platform prerequisites](https://void.cloud/guide/platform/installation/prerequisites)
- [Void Cloud](https://void.cloud/)
- [Void on Cloudflare (integration docs)](https://void.cloud/integrations/cloudflare)
- Section 4 walkthrough: supplied by the requester, unverified.
- Not readable from this environment: the Gemini share `https://share.gemini.google/xVCdBEL7peBk`.
