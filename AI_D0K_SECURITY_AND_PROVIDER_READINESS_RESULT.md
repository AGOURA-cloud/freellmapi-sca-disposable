# AI-D0K — Security and Provider Readiness (2026-11-19)

**Decision: AI-D0K offline security research and selected Linux regression suite COMPLETE/PASS; FreeLLMAPI external inference or NOVA integration remains NO-GO.**

## Verified execution evidence

- [Successful isolated security run 44536852155](https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44536852155)
- [Raw evidence artifact 14535294668](https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44536852155/artifacts/14535294668) (7-day retention).
- Exact upstream source: `tashfeenahmed/freellmapi` v0.16.34 commit `9d2b4b407b12e489ecd35320cd630d5ff97cd666`.
- Audited candidate lockfile SHA256: `dc7799e8134eb428d38857b6392b9cb499784416cebd0e9b6fe179091138da36`.
- Pinned test image digest: `node@sha256:d862bb5d082c5ae0210ac07e8c0344cb8cf7bef6d78e9b8c50c179da923108ea`.
- Full fresh npm audit: **0 affected package entries**.
- Public package install using **`npm ci --ignore-scripts`**, then local `better-sqlite3` rebuild in **network-disabled** sandbox: **PASS**.
- 21 targeted test files: **221 tests passed, 0 failed, 4 skipped**. The four skips are **Windows-specific SQLite database-permission/ACL tests**, not applicable to the Linux test runner.
- Breakdown: authentication/session **53**, MCP/rate **41**, Host/CORS/CSP **35**, SSRF/credentials/crypto/logging/database **92**.
- Independent downloaded-evidence verification: **28/28 checks passed**.
- Zero real model calls, credentials, deployments, or changes to NOVA production/Main/MVP/risk rules.
- Temporary `.github/workflows/ai-d0k-one-shot.yml` and `AI_D0K_LAUNCH.txt` removed after completed run; the original manual dependency audit remains.

## Source-verified security observations

1. **Network surface: deployment hardening needed.** [config](https://github.com/tashfeenahmed/freellmapi/blob/9d2b4b407b12e489ecd35320cd630d5ff97cd666/server/src/lib/config.ts) defaults `HOST=::` (dual-stack); Docker Compose's default published port is loopback but deployments vary. [Host-header DNS-rebinding guard](https://github.com/tashfeenahmed/freellmapi/blob/9d2b4b407b12e489ecd35320cd630d5ff97cd666/server/src/middleware/allowed-hosts.ts) is opt-in when `ALLOWED_HOSTS` is unset; `*` disables it. Configure `HOST=127.0.0.1`, `ALLOWED_HOSTS=localhost,127.0.0.1` and `TRUST_PROXY=false` for a future loopback-only sandbox.
2. **MCP auth and authority.** [MCP router](https://github.com/tashfeenahmed/freellmapi/blob/9d2b4b407b12e489ecd35320cd630d5ff97cd666/server/src/routes/mcp.ts) is disabled on fresh installations and requires a unified bearer key when enabled. Its tool list includes `set_routing_strategy` and `clear_cache` (state changing), and `ask_freellmapi` and `web_search` (potential external quota usage). **Do not assume a unified MCP key is read-only.**
3. **Admin auth.** [Express application mounts](https://github.com/tashfeenahmed/freellmapi/blob/9d2b4b407b12e489ecd35320cd630d5ff97cd666/server/src/app.ts) gate administrative routes behind dashboard sessions separately from `/v1` bearer authentication. Initial setup requires local request or one-time setup code remotely.
4. **SSRF:** [Custom-provider URL guard](https://github.com/tashfeenahmed/freellmapi/blob/9d2b4b407b12e489ecd35320cd630d5ff97cd666/server/src/lib/url-guard.ts) blocks cloud metadata/link-local destinations but loopback/private addresses remain allowed unless `FREEAPI_BLOCK_PRIVATE_PROVIDER_URLS=true`. Source acknowledges a residual DNS re-resolution timing window; restrict egress for any hosted service.
5. **Local content and credentials:** Provider API keys support AES-256-GCM encryption with a persistent `ENCRYPTION_KEY`. Saved dashboard conversations persist JSON messages in SQLite; [response cache](https://github.com/tashfeenahmed/freellmapi/blob/9d2b4b407b12e489ecd35320cd630d5ff97cd666/server/src/services/cache.ts) is opt-in but can persist plaintext model responses, and request analytics/logs have retention periods. Keep caching and unnecessary transcripts off.
6. **Deployment architecture:** FreeLLMAPI is Node.js/Express with native SQLite; it is **not a drop-in Cloudflare Worker**. NOVA must not be modified merely to host it.

## Official provider findings (no account used or provider called)

- [Gemini API pricing/data-use](https://ai.google.dev/gemini-api/docs/pricing): free-tier content may be used to improve Google's products. No sensitive NOVA code in free-tier prompts.
- [Groq data controls](https://console.groq.com/docs/your-data): limited default inference retention; potential reliability/abuse logs up to 30 days and opt-in Zero Data Retention available. [Limits](https://console.groq.com/docs/rate-limits) vary by model/account.
- [OpenRouter free collection](https://openrouter.ai/collections/free-models): typical free routes limited to **50 requests/day and 20 requests/minute** without qualifying credit purchase; [provider-specific privacy policies vary](https://openrouter.ai/privacy).
- [Cloudflare Workers AI data policy](https://developers.cloudflare.com/workers-ai/platform/data-usage/): Cloudflare does not use Workers AI customer content for model training. [Pricing](https://developers.cloudflare.com/workers-ai/platform/pricing/): **10,000 free Neurons/day**, some models paid-only.
- FreeLLMAPI's [own provider quota documentation](https://github.com/tashfeenahmed/freellmapi/blob/9d2b4b407b12e489ecd35320cd630d5ff97cd666/docs/en/providers/02-quotas-and-cooldowns.md) notes that some validation probes can consume actual metered quota. Free tier is not unlimited.

## Remaining required gates

**NO-GO** for paid/free-provider inference, connecting any AI account, unreviewed prompt transfers, storing NOVA source, making changes to NOVA, always-on service deployment or autonomous trading. Still unverified: actual provider terms/account controls, full security penetration testing, all application tests, current runtime-image audit, reliability and measured development speed.

**Proposed next AI-D0L:** design a measured **synthetic/public-code-only** benchmark for developer help; review one eligible privacy-compatible provider's account settings/ToS; request separate approval **before** any real provider call or credentials. Judge speed by net review-and-fix time, tests, and risk, not the claimed 2× target.

**Status:** Isolated source-and-test security research COMPLETE; real-model and NOVA integration **NOT APPROVED**.