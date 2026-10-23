# Workspace

## Overview

pnpm workspace monorepo using TypeScript. Each package manages its own dependencies.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)

## Key Commands

- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- `pnpm --filter @workspace/api-server run dev` — run API server locally

See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details.

## Apps

### nfse-emitter (`/`)
Web app for the workflow "Conferência NFS-e": upload of a Lince financial PDF, extraction of the
"VALORES A RECEBER" section (ignoring "BLOQUEIOS E CANCELAMENTOS" and inadimplência/cancelamento
records), grouping by CPF (sum of valor + concatenated documentos), integrity check against
"Total Geral", and generation of a downloadable Playwright Node.js script for the user to run
locally and automate filling of the Emissão Simplificada form on nfse.gov.br after manual
Gov.br login.

Backend routes (in `artifacts/api-server`):
- `POST /api/nfse/parse` — accepts `{ contentBase64, filename }`, returns valid notes + ignored
  records + integrity result. PDF parsing uses `pdfjs-dist` (legacy build) with positional
  text reconstruction.
- `POST /api/nfse/script` — accepts `{ notes }`, returns a runnable `emitir-nfse.mjs` Playwright
  script + setup instructions.
