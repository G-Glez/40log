# 40log — Agent Guide

## Stack
- Angular 21.2, standalone components (no NgModules), `bootstrapApplication` in `src/main.ts`
- Tailwind CSS 4 via PostCSS (`@tailwindcss/postcss` in `.postcssrc.json`, `@import 'tailwindcss'` in `src/styles.css`)
- Vitest 4 for unit tests (run via `ng test`; Angular CLI `@angular/build:unit-test` builder)
- Prettier for formatting (100 print width, single quotes, `angular` parser for `*.html`)
- TypeScript 5.9 in strict mode, `module: "preserve"`

## Commands
```bash
ng serve          # dev server at localhost:4200 with HMR
ng build          # production build → dist/
ng test           # run Vitest unit tests (no --watch: just run and exit)
npm run watch     # ng build --watch --configuration development
```

## Git
- **Branch**: `dev` is the working branch. Never commit directly to `dev`. Create a feature branch from `dev` (e.g. `feat/nombre`), work there, push, and the user opens the PR manually.
- **Main branch**: protected — no direct pushes. Only merge via PR from `dev` (or feature branches) into `main`.
- **Dev branch**: protected from deletion and force-pushes (admins included).
- **Commit style**: [Conventional Commits](https://www.conventionalcommits.org/). Prefixes: `feat:` for features, `refact:` for refactors, `fix:` for bug fixes. Messages in Spanish or English.

## Conventions
- **Format** with `npx prettier --write .` before committing. Prettier is the only formatter.
- **Tests** live as `.spec.ts` files next to their source. Vitest globals are enabled in `tsconfig.spec.json` (`vitest/globals` types), so `describe`/`it`/`expect` are available without imports.
- **Component template/style** paths use `templateUrl`/`styleUrl` (Angular idioms), not inline.
- **No routing** yet — `src/app/app.routes.ts` has an empty `Routes` array.
- **MCP**: `.vscode/mcp.json` configures `npx @angular/cli mcp` for AI tooling.

## Development
- Angular CLI schematics work: `ng generate component`, etc.
- For debugging in VS Code, use the provided launch configs (`ng serve` / `ng test`).

## Deployment (Cloudflare Workers)
- `wrangler.jsonc` configures Workers with static assets. Angular builds to `dist/40log/browser/` — **not** `dist/`. If `assets.directory` is wrong, you get 404.
- For SPA routing, add a `_routes.json` in `public/` (gets copied to output). Without it, non-root paths will 404.
- Deploy with `npx wrangler deploy`.
