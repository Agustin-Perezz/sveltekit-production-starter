# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Development
pnpm dev              # Start dev server
pnpm build            # Production build
pnpm preview          # Preview production build

# Quality checks
pnpm lint             # Prettier + ESLint
pnpm check            # Svelte + TypeScript type checking
pnpm format           # Format with Prettier

# Testing
pnpm test             # Run Playwright E2E tests
pnpm test:show-report # Open Monocart test report
pnpm coverage:show-report # Open V8 coverage report
```

## Architecture

**SvelteKit 2 + Svelte 5** application using file-based routing, static adapter, and modern runes syntax (`$props()`, `$derived()`, `$bindable()`).

### Key Directories

- `src/routes/` - File-based routing; `+page.svelte` for pages, `+layout.svelte` for layouts
- `src/lib/components/ui/` - Reusable UI components following shadcn-svelte patterns
- `src/lib/server/` - Server-only code (auth, etc.)
- `src/hooks.client.ts` / `src/hooks.server.ts` - SvelteKit lifecycle hooks with Sentry integration
- `e2e/` - Playwright E2E tests with V8 code coverage

### Import Aliases

- `$lib` → `src/lib`
- `$components` → `src/lib/components`
- `$schemas` → `src/lib/schemas`

### Component Patterns

Components use **tailwind-variants** for styling with compound component patterns:

```svelte
<script lang="ts" module>
  // Export types at module level
  export type ButtonVariant = VariantProps<typeof buttonVariants>['variant'];
</script>

<script lang="ts">
  // Use Svelte 5 runes
  let {
    children,
    variant = 'default',
    ref = $bindable(null),
    ...restProps
  } = $props();
</script>
```

### Forms

Uses **sveltekit-superforms** with **Zod** schemas for validation. Form components in `$components/ui/form-field/` handle error display automatically.

### Authentication

Cookie-based auth handled in `src/hooks.server.ts`. Protected routes check `locals.user` and redirect to `/` if unauthenticated.

### Error Tracking

Sentry configured in both client and server hooks. Source maps uploaded during build via Vite plugin.

## Git Hooks

- **Pre-commit**: lint-staged runs Prettier + ESLint on staged files
- **Pre-push**: Full E2E test suite runs

## Environment Variables

Copy `.env.dist` to `.env` and configure:

- `VITE_API_BASE_URL` - API endpoint
- `VITE_SENTRY_DSN` - Sentry DSN for client
- `SENTRY_*` - Build-time Sentry config for source maps
