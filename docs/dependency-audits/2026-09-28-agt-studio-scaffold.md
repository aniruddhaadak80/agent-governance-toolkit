---
title: AGT Studio initial dependency lockfile
last_reviewed: 2026-09-28
owner: Ricky-G
---

# AGT Studio initial dependency lockfile

## Which dependencies changed and why

This change introduces `agent-governance-studio/web/package-lock.json` for the
new, private Studio frontend. There is no previous Studio lockfile or existing
Studio dependency graph to migrate.

| Direct packages | Pinned versions | Purpose |
|---|---|---|
| `react`, `react-dom` | 18.3.1 | Render the browser entry point using the architecture's React 18 choice. |
| `@tanstack/react-query` | 5.83.0 | Establish the chosen query-provider toolchain without adding network calls. |
| `vite`, `@vitejs/plugin-react`, `typescript` | 7.3.6, 4.7.0, 5.9.2 | Compile and bundle the TypeScript React entry point. |
| `tailwindcss`, `@tailwindcss/vite` | 4.1.13 | Compile the starter stylesheet. |
| `vitest`, `jsdom`, `@testing-library/react`, `@testing-library/dom` | 4.1.11, 26.1.0, 16.3.0, 10.4.1 | Execute DOM rendering and bootstrap tests. |
| `eslint`, `@eslint/js`, `typescript-eslint`, `globals` | 9.32.0, 9.32.0, 8.39.1, 16.3.0 | Enforce a non-no-op frontend lint gate. |
| `@types/react`, `@types/react-dom`, `@types/node` | 18.3.27, 18.3.7, 22.18.6 | Typecheck the browser and build configuration. |

The committed lockfile records transitive versions and integrity hashes for
reproducible `npm ci` installation. The Python Studio package introduces no
runtime dependencies.

## Security advisory relevance

The initial Vite and Vitest selections reported npm advisories, so they were
replaced with patched `vite@7.3.6` and `vitest@4.1.11` before committing the
lockfile. The full `npm audit` and dependency-review checks reported zero
vulnerable packages for the committed tree. The release-age and upstream
lockfile-integrity checks passed on the PR. Install scripts must still pass
the repository's separate registry-backed audit; this document does not
replace that check.

## Breaking change risk assessment

This is an additive package that does not change any existing runtime. The
frontend is not published or launched by existing AGT commands. The targeted
Python and frontend tests, both builds, and an isolated Python wheel import
passed locally. The only new executable frontend behavior is rendering the
minimal Studio heading; sidecar and product features are deferred.
