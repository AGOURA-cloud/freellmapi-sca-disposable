# AI-D0G — metadata-only patched dependency security experiment

Date: 2026-11-19. Verdict: **NO-GO for FreeLLMAPI installation, external inference and NOVA integration.**

## Evidence

- [Actual GitHub Actions run](https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44517952229)
- [Original patched-lockfile and npm-audit artifact](https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44517952229/artifacts/14525659737) (7-day retention).
- FreeLLMAPI v0.16.34 public source commit: 9d2b4b407b12e489ecd35320cd630d5ff97cd666.
- Original Git lockfile blob SHA: 1ee6ad8e90aa6288ccc4e03402c63f11e5f7a537.
- Candidate patched lockfile SHA-256: 7899f3a635988d65dff9cae750fc86d95060f492534d0f5e3b3802a5cbe6db0b.

## Actual metadata-only patch

| Server dependency | Original resolved | Candidate resolved |
|---|---:|---:|
| undici | 6.28.0 | 6.29.1 |
| multer | 2.2.0 | 2.5.0 |
| compression | 1.8.1 | 1.8.2 |
| sharp | 0.35.3 | 0.35.5 |

The four corresponding named affected packages disappeared from the updated full npm audit. No FreeLLMAPI code was executed or installed. Runtime compatibility was NOT tested.

## Registry-backed npm-audit results

| Severity | Original full | Patched full | Patched --omit=dev |
|---|---:|---:|---:|
| Critical | 2 | 2 | 0 |
| High | 13 | 9 | 9 |
| Moderate | 15 | 15 | 9 |
| **Total** | **30** | **26** | **18** |

Note: the original --omit=dev baseline was not measured. Omit-dev includes non-dev packages across the monorepo, not necessarily the deployed server binary. Counts represent affected package entries, not unique CVEs or known exploits.

## Remaining 26 affected package names

@hono/node-server, @modelcontextprotocol/sdk, @ts-morph/common, @typescript-eslint/eslint-plugin, @typescript-eslint/parser, @typescript-eslint/type-utils, @typescript-eslint/typescript-estree, @typescript-eslint/utils, baseline-browser-mapping, body-parser, brace-expansion, browserslist, concurrently, fast-uri, hono, ip-address, minimatch, postcss-selector-parser, qs, shadcn, shell-quote, source-map-js, ts-morph, typescript-eslint, vite, zod.

## Security

Isolated GitHub runner copied ONLY public package manifests and npm lockfile from immutable FreeLLMAPI source, regenerated lockfile using npm install --package-lock-only --ignore-scripts --no-audit, then audited full and --omit=dev views. No npm package installation, app execution, AI calls, keys, NOVA code or deployment. The temporary push trigger and start marker have both been deleted after the run; manual-only audit workflow remains.

**Integration remains NO-GO. Next AI-D0H:** Triage all remaining findings; plan parent-level metadata remediation; obtain separate approval before any third-party installation/build/run or external model call. The 2x acceleration goal remains unmeasured.
