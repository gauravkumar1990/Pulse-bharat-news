# Pulse Bharat — Vue + Supabase + Hono News Platform

Responsive news product inspired by modern Indian and international newsroom UX, without copying a specific brand. Includes public news homepage, Shorts, polls, login, subscription-ready data model, analytics event API and an editorial/admin dashboard.

## Stack
- Vue 3 + Vue Router + Pinia + Vite
- Hono TypeScript API
- Supabase Auth, Postgres and RLS

## Live demo
The Vue frontend is configured for GitHub Pages using the included GitHub Actions workflow. The public demo works with sample content without environment variables.

## Local development
1. cd apps/web && npm install && npm run dev
2. cd apps/api && npm install && npm run dev
3. Configure Supabase environment variables when ready.

The Hono API requires a server runtime and is not hosted by GitHub Pages.