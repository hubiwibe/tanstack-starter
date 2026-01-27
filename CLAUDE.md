# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
pnpm dev          # Start dev server on port 3000
pnpm build        # Build for production
pnpm test         # Run Vitest tests
pnpm lint         # Lint with Biome
pnpm format       # Format with Biome
pnpm check        # Run both lint and format checks
```

Add Shadcn components:

```bash
pnpm dlx shadcn@latest add <component-name>
```

## Tech Stack

- **Framework**: TanStack Start (full-stack React with SSR and server functions)
- **React 19** with TypeScript 5.9 (strict mode)
- **Routing**: TanStack Router (file-based in `src/routes/`)
- **Data Fetching**: TanStack Query + route loaders
- **Forms**: TanStack React Form
- **Tables**: TanStack React Table
- **Auth**: Better Auth (email/password, extendable with database)
- **i18n**: ParaglideJS (URL-based localization, messages in `project.inlang/messages`)
- **Styling**: Tailwind CSS 4 + Shadcn UI (new-york style, zinc base)
- **Linting/Formatting**: Biome (tabs, double quotes)
- **Testing**: Vitest + Testing Library

## Architecture

### File-Based Routing

Routes are defined in `src/routes/`. The route tree is auto-generated to `src/routeTree.gen.ts` (read-only, do not edit).

- `__root.tsx` - Root layout with devtools
- `api/` - Server-side API routes
- Route files export a `Route` object created with `createFileRoute()`

### Server Functions

Use `createServerFn()` from `@tanstack/react-start` for type-safe server-side functions:

```tsx
import { createServerFn } from "@tanstack/react-start";

const serverFn = createServerFn({ method: "GET" })
  .validator(schema)
  .handler(async ({ data }) => { ... });
```

### Key Directories

- `src/components/ui/` - Shadcn UI components (excluded from Biome)
- `src/integrations/` - Framework integrations (TanStack Query provider, Better Auth)
- `src/lib/` - Utilities (`auth.ts`, `auth-client.ts`, `utils.ts` with `cn()`)
- `src/paraglide/` - Generated i18n runtime (excluded from Biome)

### Demo Files

Files prefixed with `demo` are examples and can be safely deleted.

## Code Style

- Biome handles formatting (tabs, double quotes for JS/TS)
- Path alias: `@/*` maps to `src/*`
- Use `cn()` from `@/lib/utils` for merging Tailwind classes
