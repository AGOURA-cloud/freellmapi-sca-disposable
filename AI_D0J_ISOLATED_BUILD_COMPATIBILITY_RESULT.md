# AI-D0J — verified restricted FreeLLMAPI build and compatibility experiment

**Date:** 2026-11-19. **Status:** Selected isolated checks PASSED. **NOVA integration remains NO-GO / not approved.**

## Final real GitHub evidence

- [Successful corrected run #44531244819](https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44531244819)
- [Actual uploaded test logs, exit statuses, pinned image digest, npm audit result](https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44531244819/artifacts/14533427153) (7-day retention)
- Pinned public source: `9d2b4b407b12e489ecd35320cd630d5ff97cd666` (FreeLLMAPI v0.16.34)
- Pinned security-candidate package-lock SHA-256: `dc7799e8134eb428d38857b6392b9cb499784416cebd0e9b6fe179091138da36`
- Container image digest: `node@sha256:d862bb5d082c5ae0210ac07e8c0344cb8cf7bef6d78e9b8c50c179da923108ea`

## Actual test outcomes (all zero exit codes)

| Check | Outcome |
|---|---|
| Fresh npm audit against pinned patched lockfile | 0 affected entries |
| `npm ci --ignore-scripts` in disposable Docker | PASS |
| Scoped minimatch glob, ts-morph in-memory AST, shadcn package version | PASS |
| Server TypeScript build | PASS |
| Client/dashboard TypeScript + Vite build, including CSS resolution | PASS |
| CLI build | PASS |
| Focused server tests: URL security guard (29), image processing (12) | **41/41 PASS** |
| Focused client tests: app shell | **3/3 PASS** |
| Targeted offline `better-sqlite3` rebuild | PASS |
| Native SQLite in-memory query | PASS |

**Total functional checks: 8/8 groups passed**, **focused unit tests 44/44 passed**. A separate local check validated the preserved evidence: **27/27 integrity checks**.

## Safety boundaries

- Original upstream revision verified by commit and original Git lockfile blob.
- Exact clean candidate reused from [AI-D0I-B](https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44522639358); did not accept an automatically different lockfile.
- All application builds and tests ran in Docker with `--network none --read-only --cap-drop ALL --security-opt no-new-privileges`, a non-root user, memory/CPU/process limits, and no app credentials.
- Only initial npm registry dependency download allowed networking; lifecycle scripts disabled during this install.
- `better-sqlite3` rebuild happened later only in network-disabled, no-secrets Docker.
- No external AI inference, active FreeLLMAPI service, NOVA repo code changes, Cloudflare changes, deployment or trading.
- Temporary one-shot GitHub workflow and all AI-D0J launch markers have been removed.

## Research corrections

- [First AI-D0J run #44530313687](https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44530313687) stopped before installing code due a fresh-resolver lockfile hash mismatch (safety gate worked).
- [Second run #44530683136](https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44530683136) passed builds, 44 tests, SQLite but failed a *research smoke test* that incorrectly expected a literal shadcn CSS file path.
- Final run #44531244819 corrected the false assumption; real Vite client CSS build still passed, and the entire selected gate passed.

## Still NOT verified

Full source audit, all application tests, server startup and HTTP/MCP authentication, provider ToS/privacy/security, runtime image SBOM, rate/quota reliability, real free model inference or any measured speed-up for NOVA.

**Next proposed AI-D0K:** independently assess local-only server/HTTP/MCP authentication and exposure in a disposable no-secrets sandbox plus provider privacy/terms without contacting paid/private AI services. A separate approval will be needed before connecting any external AI account or feeding NOVA code.

### Conclusion

AI-D0J isolated build and selected compatibility **PASS**, but live FreeLLMAPI use or NOVA integration is **not authorized**. The hypothesis of doubling development speed remains untested.
