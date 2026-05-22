# Roof Today

A full-stack web application for Roof Today, Seattle's premier roofing company — featuring a public marketing site with a lead capture form and a password-protected admin dashboard.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 8080)
- `pnpm --filter @workspace/roof-today run dev` — run the frontend (port 25756)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- Frontend: React + Vite, Tailwind CSS, TanStack Query, Wouter
- API: Express 5 + express-session + bcryptjs
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `lib/api-spec/openapi.yaml` — API contract source of truth
- `lib/db/src/schema/leads.ts` — Leads table schema
- `lib/db/src/schema/admin-users.ts` — Admin users table schema
- `artifacts/api-server/src/routes/leads.ts` — Public leads routes
- `artifacts/api-server/src/routes/admin.ts` — Admin routes (auth-protected)
- `artifacts/roof-today/src/` — React frontend

## Architecture decisions

- Session-based auth using `express-session` with httpOnly cookies — the spec explicitly requires username/password admin auth
- PostgreSQL (Drizzle ORM) instead of SQLite — matches the existing Replit workspace stack
- React + Vite SPA with wouter routing — public site at `/`, admin at `/admin`, login at `/admin/login`
- The design subagent handles full frontend: marketing site, admin dashboard, login page

## Product

- **Public site** (`/`): Marketing landing page with hero, services, reviews, about, and a contact/lead capture form
- **Admin** (`/admin`): Password-protected dashboard showing lead stats, full leads table with search/filter/status updates/delete
- **Lead flow**: Visitor submits form → saved to DB → appears in admin dashboard with status tracking (new → contacted → quoted → closed)

## User preferences

- Bold industrial-premium design: dark slate (#0D1117), electric orange (#FF6B2B), Bebas Neue headlines, DM Sans body
- Company: Roof Today, 18323 Bothell Everett Hwy #345, Bothell, WA 98012, (844) 760-2914

## Admin credentials

- Username: `admin`
- Password: `rooftoday2024`

## Gotchas

- After changing `lib/db/src/schema/`, run `pnpm --filter @workspace/db run push` then `pnpm run typecheck:libs`
- After changing `lib/api-spec/openapi.yaml`, run `pnpm --filter @workspace/api-spec run codegen`
- Credentials cookie requires `credentials: 'include'` in all fetch calls (already set in `lib/api-client-react/src/custom-fetch.ts`)

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
