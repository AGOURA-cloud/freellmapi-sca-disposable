# AI-D0K — Security & Provider Readiness (verified, 2026-11-19)

**Decision:** Selected offline security regression gate **PASS**; **external AI inference / FreeLLMAPI-to-NOVA integration remains NO-GO without separate approval.**

## Reproducible evidence

- [Actual AI-D0K GitHub Actions run](https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44533366364)
- [Actual raw logs and npm audit evidence](https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44533366364/artifacts/14535381412) (7-day artifact retention)
- Artifact ZIP SHA256 `e538a45bf5412b6690c91f80eb950d0b45ef149eeb92a7b2bad9dc82d8e85940`
- Source commit `9d2b4b407b12e489ecd35320cd630d5ff97cd666`; patched lockfile SHA256 `dc7799e8134eb428d38857b6392b9cb499784416cebd0e9b6fe179091138da36`.

## Actual test results

| Group | Tests | Result |
|---|---:|---|
| MCP HTTP endpoint, unified key, disabled-by-default, MCP settings | 32 | PASS |
| Dashboard auth, login lockout, initial setup, desktop-session protection | 31 | PASS |
| Host-header guard, CORS, CSP protections | 30 | PASS |
| Administrative/proxy rate limits, error sanitation, URL restrictions | 40 | PASS |
| **Total** | **133** | **133 passed, 0 failed** |

Native SQLite rebuild and in-memory query: PASS. Dependency install with scripts disabled in separate ephemeral Docker; all test execution under `--network none`, nonroot, read-only rootfs and dropped capabilities. Public npm audit: **0 reported affected package entries**. No private credentials or external AI inference; NOVA production and Github main unchanged.

**Important GitHub run-report correction:** GitHub marks the run red only because its final guard compared against the obsolete AI-D0J success string (`SELECTED_ISOLATED_TESTS_PASSED_NOT_INTEGRATION_APPROVAL`) instead of the new AI-D0K value (`SELECTED_OFFLINE_SECURITY_TESTS_PASSED_NOT_INTEGRATION_APPROVAL`). The recorded `summary.json`, all 6 underlying zero exit codes and the 133 actual passing tests were independently inspected. This is a CI *reporting* defect, not an application test failure. There is no blanket FreeLLMAPI security certification.

## Source security architecture and deployment risks

- Dashboard `/api/*` protected by opaque session tokens; separate unified API key authenticates `/v1` and `/mcp`.
- MCP disabled by default but when enabled offers inference and state-changing tools (e.g. routing strategy updates, cache clear), not a read-only data interface. Never give external agents its unrestricted unified key.
- The pinned standalone Node server defaults `HOST=::` (not guaranteed loopback only), whereas Docker Compose's host publishing defaults `HOST_BIND=127.0.0.1`.
- `ALLOWED_HOSTS` is **not enforced if unset**; an explicit loopback-only host allowlist should be mandatory for any isolated startup test.
- Password/session and threat boundaries need further manual review. A clean unit test suite does not prove public deployment safety.

## Provider privacy and cost readiness (terms only; no inference calls)

- Google Gemini unpaid: Unpaid prompts and generated responses can be used for product improvement and undergo human review; only synthetic/nonconfidential prompts. https://ai.google.dev/gemini-api/terms
- GroqCloud: inference data normally not retained except limited operational circumstances; offers opt-in Zero Data Retention for eligible customers, settings/model terms must be verified for exact account. https://console.groq.com/docs/your-data
- OpenRouter: providers have different logging/retention policies and free model quotas. Enforce account/provider privacy preferences and treat routing as a separate privacy boundary. https://openrouter.ai/docs/guides/privacy/provider-logging
- Cloudflare Workers AI: published 10,000 free Neurons/day; Cloudflare says it does not use Workers AI customer content to train or improve products without explicit consent. Third-party licenses still apply. https://developers.cloudflare.com/workers-ai/platform/pricing/ and https://developers.cloudflare.com/workers-ai/platform/data-usage/

## Decision and proposed next gate

**Local test pass. No actual server service was deployed or exposed; no FreeLLMAPI connection was configured.** Next proposed AI-D0L: isolated loopback-only HTTP/MCP startup exercise with no secrets; restricted read-only gateway design; synthetic AI-task quality/latency benchmarking only after separate external-provider consent. NOVA remains paper-only and unchanged. 2x speed claim unmeasured.
