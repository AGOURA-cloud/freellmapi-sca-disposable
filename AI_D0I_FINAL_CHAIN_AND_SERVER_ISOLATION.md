# AI-D0I — verified final advisory chain and isolated server metadata

**2026-11-19 | Research result: dependency metadata scans clean in the tested candidate; FreeLLMAPI runtime/integration still NO-GO.**

## Evidence and exact original source
- FreeLLMAPI source commit: `9d2b4b407b12e489ecd35320cd630d5ff97cd666` (v0.16.34).
- Original lockfile Git blob: `1ee6ad8e90aa6288ccc4e03402c63f11e5f7a537`.
- [AI-D0I initial locked-source and server-only execution](https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44521983228) — [raw artifact](https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44521983228/artifacts/14526949871) (7-day retention).
- [AI-D0I-B fresh lockfile resolver execution](https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44522639358) — [raw artifact](https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44522639358/artifacts/14529460580) (7-day retention).

## Result
| Scope | npm audit affected package entries |
|---|---:|
| Original full pinned source (AI-D0E) | 30 |
| 4 server fixes (AI-D0G) | 26 |
| Compatible lockfile fixes (AI-D0H) | 4 |
| AI-D0I first incremental scoped override | 4 |
| AI-D0I-B fresh full lockfile without scoped override | 4 |
| **AI-D0I-B fresh full lockfile with scoped override** | **0** |
| **AI-D0I isolated Server+Shared --omit=dev audit** | **0** |

Four inherited moderate findings stemmed from `shadcn → ts-morph → @ts-morph/common → minimatch`, with `minimatch 10.2.6` affected by GHSA-g3hv-75wg-m249. Fresh npm resolution with ONLY a root-scoped override made the nested `@ts-morph/common` instance of `minimatch` resolve to `11.0.1`. `shadcn` remained 4.23.0. The only changed package **version** between the two freshly resolved candidate lockfiles was that one nested minimatch instance.

### Research-only root-level override
```json
{ "overrides": { "@ts-morph/common": { "minimatch": "11.0.1" } } }
```

**Mandatory compatibility warning:** This is a forced `minimatch 10.x → 11.x` major upgrade beyond @ts-morph/common's declared dependency range. No third-party code was installed, loaded, compiled or executed. A clean registry metadata audit is not proof of application compatibility, a secure deployment, or provider reliability.

## Next gate (not authorized by this experiment)
Separately authorize a disposable, no-secrets build/test sandbox; test server, client CSS, shadcn/ts-morph/minimatch compatibility, native addon installation, source behavior and deployment SBOM. Review provider privacy/ToS independently and authorize any external inference separately. Experimental 2× speed target is still unmeasured.

Independent local artifact checks: **18/18 passed** for raw evidence integrity and comparison logic; they are not FreeLLMAPI functional tests. Temporary one-shot workflows and start markers from both AI-D0I runs were deleted after completion. Original manual-only security audit workflow retained.

**NOVA production, GitHub main, Early Signal MVP, three-confirmation rule, risk safeguards and paper-only policy unchanged.**
