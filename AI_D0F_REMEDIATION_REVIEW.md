# NOVA AI-D0F — FreeLLMAPI dependency remediation review
Date: 2026-11-19. **Decision: NO-GO** for FreeLLMAPI installation, external AI inference, or NOVA integration.

## Evidence independently verified
- [Initial npm audit run](https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44508422963)
- [Second manually launched audit run](https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44510911183)
- FreeLLMAPI release v0.16.34, fixed public source commit `9d2b4b407b12e489ecd35320cd630d5ff97cd666`
- Verified `package-lock.json` blob SHA `1ee6ad8e90aa6288ccc4e03402c63f11e5f7a537`
- The **two raw npm audit reports are byte-identical**, SHA-256 `56828fc6a8032bd70d711f37fc33e2bb2ad3e040abd9668c3faf7806919e45c7`
- Both show 30 affected package entries: **2 critical, 13 high, 15 moderate**. These are NOT unique CVEs or confirmed application exploits. All remain unremediated in the scanned lockfile.

## What matters
The **two critical** entries, `concurrently` and `shell-quote`, are marked **dev-only** in the pinned lockfile. This makes production exposure less direct, but does not make build/dev security irrelevant.

The following **direct server dependencies** have high-severity advisories, with source usage verified:
| Package | Pinned | Candidate fixed minimum | Pinned source references |
|---|---|---|---|
| `undici` | 6.28.0 | 6.29.1+ compatible 6.x | `server/src/lib/proxy.ts` lazy ProxyAgent / WebSocket |
| `multer` | 2.2.0 | 2.5.0+ | `server/src/routes/keys.ts`, `server/src/routes/proxy.ts` uploads |
| `compression` | 1.8.1 | 1.8.2+ | `server/src/app.ts` Express middleware |
| `sharp` | 0.35.3 | 0.35.5+ | `server/src/lib/image-normalize.ts` optional image normalization |

The candidate versions are from advisory ranges and **have NOT been installed or tested**. Additional transitive issues remain, including `body-parser`, `qs`, `fast-uri`, MCP SDK, UI/build tooling and other affected packages. `shell-quote` requires 1.11.0+ in an appropriate compatible parent tree.

## Remediation sequence
1. Prepare an isolated source copy and resolve a proposed corrected dependency lockfile (do not change NOVA).
2. Repeat full `npm audit` and add a *separate* `npm audit --omit=dev` comparison. Verify lockfile hashes and every critical/high finding.
3. With separately approved isolated execution, run build, test, lint and regressions for proxy/TLS/WebSocket, uploads, compression and image processing.
4. Verify final runtime artifact and dependency tree; no waived or hidden advisories.
5. Evaluate a single permitted AI provider's terms, quota and privacy before any external model call. 2× acceleration target is unmeasured.

## What actually occurred in AI-D0F
A read-only GitHub source review, two-report comparison, and **15/15 offline triage-validator tests**. The tests validate our *report parser*, not FreeLLMAPI's fixes. No new workflow runs, installs, inference calls, credentials, NOVA changes, deployments, or trading changes.

**Conclusion:** NO-GO remains. The next gate is a corrected source tree and real follow-up dependency/build audit, not blind upgrades.

The downloadable detailed checkpoint, 30-entry CSV, source ZIPs and Python test harness were generated separately in the ChatGPT conversation.
