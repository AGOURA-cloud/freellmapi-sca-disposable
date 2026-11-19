# FreeLLMAPI — first independent npm dependency audit

**Run date:** 2026-11-19 (UTC). **Decision:** NO-GO: do not install or integrate with NOVA.

## Verification

- [Actual GitHub Actions run](https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44508422963)
- [Original scan evidence](https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44508422963/artifacts/14519773859) (7-day artifact retention configured)
- Upstream source commit: `9d2b4b407b12e489ecd35320cd630d5ff97cd666` (FreeLLMAPI `v0.16.34`)
- Lockfile Git blob verified: `1ee6ad8e90aa6288ccc4e03402c63f11e5f7a537`
- Executed `npm audit --json --audit-level=low --ignore-scripts --registry=https://registry.npmjs.org`.
- NPM advisory query completed and evidence uploaded successfully. The job deliberately failed at the final safety gate because vulnerabilities were found.

## NPM audit result

| Severity | Affected npm audit package entries |
|---|---:|
| Critical | 2 |
| High | 13 |
| Moderate | 15 |
| Low | 0 |
| **Total** | **30** |

**Important:** These are npm audit affected-package counts, not 30 unique confirmed exploitable issues. Some packages are dev tooling, and some parent dependencies inherit advisory flags through transitive children.

- Critical names reported: `concurrently` (direct; flagged through `shell-quote`) and `shell-quote` (transitive).
- High-severity direct package names include `undici`, `sharp`, `compression`, `multer`, `shadcn` and `vite`.
- Exact exploitability, remediation compatibility and production reachability have **not** been established.
- This scan did **not** run `npm install`, `npm ci`, FreeLLMAPI, provider calls or NOVA code.

## Next gate

Require a corrected pinned upstream release, repeat a full security audit, triage reachable findings and test isolated hosting; verify a single provider's terms and privacy. Any model inference or NOVA integration requires separate approval.

The temporary push-only scan trigger and marker were deleted after this run. The reusable manual-only audit workflow remains. NOVA's repository, production and trading rules were not changed.
