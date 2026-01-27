# Project Index: tanstack-starter

Generated: 2026-01-19

## Overview

A full-stack TanStack Start application demonstrating modern web development patterns including SSR, server functions, TanStack Query, authentication, i18n, and form handling.

## Project Structure

```
tanstack-starter/
├── src/
│   ├── routes/           # File-based routing (TanStack Router)
│   │   ├── __root.tsx    # Root layout with Header, DevTools
│   │   ├── index.tsx     # Home page
│   │   ├── api/          # API routes
│   │   │   └── auth/     # Better Auth handler
│   │   └── demo/         # Demo feature routes
│   │       ├── api.*.ts          # API endpoints
│   │       ├── form.*.tsx        # Form demos
│   │       ├── start.*.tsx       # TanStack Start demos
│   │       ├── tanstack-query.tsx
│   │       ├── better-auth.tsx
│   │       ├── table.tsx
│   │       └── ...
│   ├── components/       # React components
│   │   ├── ui/           # Shadcn UI components
│   │   ├── Header.tsx    # Main navigation
│   │   └── LocaleSwitcher.tsx
│   ├── integrations/     # Third-party integrations
│   │   ├── tanstack-query/
│   │   │   ├── root-provider.tsx  # QueryClient setup
│   │   │   └── devtools.tsx
│   │   └── better-auth/
│   │       └── header-user.tsx
│   ├── lib/              # Utility libraries
│   │   ├── auth.ts       # Better Auth server config
│   │   ├── auth-client.ts
│   │   └── utils.ts      # cn() for class merging
│   ├── hooks/            # Custom React hooks
│   │   ├── demo.form.ts
│   │   └── demo.form-context.ts
│   ├── data/             # Demo data
│   ├── paraglide/        # Generated i18n (Paraglide)
│   ├── router.tsx        # Router configuration
│   ├── server.ts         # Server entry with Paraglide middleware
│   └── routeTree.gen.ts  # Auto-generated route tree
├── messages/             # i18n message files (en.json, de.json)
├── project.inlang/       # Paraglide i18n config
├── public/               # Static assets
├── vite.config.ts        # Vite + TanStack Start config
├── tsconfig.json
├── package.json
├── components.json       # Shadcn UI config
└── biome.json            # Linter/formatter config
```

## Entry Points

| Entry Point | Path | Purpose |
|-------------|------|---------|
| Server | `src/server.ts` | SSR entry with Paraglide middleware |
| Router | `src/router.tsx` | TanStack Router + Query integration |
| Root Layout | `src/routes/__root.tsx` | App shell with Header, DevTools |
| Home | `src/routes/index.tsx` | Landing page |

## Core Technologies

| Technology | Version | Purpose |
|------------|---------|---------|
| TanStack Start | ^1.153.0 | Full-stack React framework |
| TanStack Router | ^1.151.6 | File-based routing with SSR |
| TanStack Query | ^5.90.19 | Server state management |
| TanStack Form | ^1.27.7 | Form state management |
| TanStack Table | ^8.21.3 | Data table component |
| Better Auth | ^1.4.15 | Authentication |
| Paraglide JS | ^2.9.0 | i18n with URL-based localization |
| Tailwind CSS | ^4.1.18 | Styling |
| Shadcn UI | (new-york) | UI component library |
| React | ^19.2.3 | UI framework |
| Vite | ^7.3.1 | Build tool |
| Nitro | nightly | Server runtime |

## Routes Reference

### Main Routes

- `/` - Home page (landing)
- `/demo/i18n` - Internationalization demo

### Demo: TanStack Start

- `/demo/start/server-funcs` - Server functions with file-based todos
- `/demo/start/api-request` - API request patterns
- `/demo/start/ssr/` - SSR demos index
- `/demo/start/ssr/spa-mode` - SPA mode rendering
- `/demo/start/ssr/full-ssr` - Full SSR rendering
- `/demo/start/ssr/data-only` - Data-only SSR

### Demo: Features

- `/demo/tanstack-query` - TanStack Query with todos
- `/demo/better-auth` - Authentication demo
- `/demo/table` - TanStack Table demo
- `/demo/form/simple` - Simple form demo
- `/demo/form/address` - Address form demo

### API Routes

- `/api/auth/$` - Better Auth catch-all handler
- `/demo/api/tq-todos` - Todos API for TanStack Query demo
- `/demo/api/names` - Names API endpoint

## Key Modules

### Router (`src/router.tsx`)

- Creates TanStack Router with SSR Query integration
- Configures Paraglide URL rewriting for i18n
- Exports `getRouter()` factory function

### Server (`src/server.ts`)

- Applies Paraglide middleware for server-side i18n
- Delegates to TanStack Start server handler

### Auth (`src/lib/auth.ts`)

- Better Auth configuration
- Email/password authentication enabled
- Stateless by default (add database for persistence)

### TanStack Query (`src/integrations/tanstack-query/`)

- `root-provider.tsx`: QueryClient factory and Provider
- `devtools.tsx`: React Query DevTools plugin

### UI Components (`src/components/ui/`)

- Button, Input, Label, Select, Slider, Switch, Textarea
- Uses Shadcn UI (new-york style)
- Class variance authority for variants

## Configuration

### Path Aliases

```
@/* -> ./src/*
```

### Scripts

```bash
pnpm dev      # Start dev server (port 3000)
pnpm build    # Production build
pnpm preview  # Preview production build
pnpm test     # Run Vitest tests
pnpm lint     # Biome linting
pnpm format   # Biome formatting
pnpm check    # Biome check
```

### i18n Locales

- `en` (English)
- `de` (German)
- Messages in `messages/*.json`
- URL-based strategy (e.g., `/de/demo/i18n`)

## Quick Start

```bash
# Install dependencies
pnpm install

# Set up Better Auth (optional)
npx @better-auth/cli secret  # Generate BETTER_AUTH_SECRET

# Start development server
pnpm dev
```

## Development Notes

- Demo files (prefixed with `demo`) can be safely deleted
- Route tree auto-generated in `src/routeTree.gen.ts`
- Paraglide outputs auto-generated in `src/paraglide/`
- DevTools available in development (bottom-right corner)
