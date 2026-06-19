# agents.md

Guidance for AI coding agents working on **sveltekit-production-starter** — a production-ready SvelteKit template built around shift-left quality practices.

## Stack

- **Svelte 5** (Runes API: `$state`, `$derived`, `$effect`, `$props`)
- **TypeScript** strict (no `any`; use `satisfies` and utility types)
- **Tailwind CSS v4** + `tailwind-merge` / `tailwind-variants`
- **shadcn-svelte (Bits UI)** — `Button`, `Input`, `Label`, `FormField`
- **sveltekit-superforms + Zod** — type-safe forms with server validation
- **Axios** — configured HTTP client
- **Sentry** — error/perf monitoring (production-only, in both hooks)

## Quality pipeline

1. **Pre-commit** — `lint-staged` runs Prettier + ESLint via Husky
2. **Pre-push** — Full Playwright E2E suite with V8 coverage
3. **CI** — lint, type-check, E2E, and build on every push/PR to `main`
4. **Runtime** — Sentry

## Routes

- `/` — home
- `/protected` — server-guarded (`+page.server.ts` auth)
- `/pokemon/[id]` — dynamic scaffold
- `/images` — scaffold

## Key libraries

| Category      | Library                                                                   |
| ------------- | ------------------------------------------------------------------------- |
| UI            | `bits-ui`, shadcn-svelte patterns                                         |
| Forms         | `sveltekit-superforms`, `zod`                                             |
| HTTP          | `axios`                                                                   |
| Styling       | `tailwindcss` v4, `tailwind-merge`, `tailwind-variants`, `tw-animate-css` |
| Monitoring    | `@sentry/sveltekit`                                                       |
| Testing       | `@playwright/test`, `monocart-reporter` (V8 coverage)                     |
| Code quality   | `eslint`, `prettier`, `husky`, `lint-staged`                              |

## Architecture

- `src/routes/` — file-based routing (`+page.svelte`, `+layout.svelte`)
- `src/lib/components/ui/` — reusable shadcn-svelte components
- `src/lib/server/` — server-only code (auth, etc.)
- `src/hooks.client.ts` / `src/hooks.server.ts` — SvelteKit hooks with Sentry
- `e2e/` — Playwright E2E tests with V8 coverage

Forms use **sveltekit-superforms** with **Zod** schemas; form components in `$components/ui/form-field/` handle error display.

## Coding standards

Follow Svelte 5, SOLID principles, and TypeScript strict conventions. Prefer `satisfies`, type guards, and explicit boundaries between domain and use-case layers (`src/domain/**`, `src/use-cases/**`).