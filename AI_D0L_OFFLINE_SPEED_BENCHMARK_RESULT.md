# AI-D0L — Offline Development Speed Benchmark Readiness (2026-11-19)

**Verdict:** AI-D0L synthetic offline benchmark **built and validated**. **No FreeLLMAPI/LLM inference occurred and no real speedup has been measured. NOVA integration remains NO-GO.**

## Deliverable

The accompanying ChatGPT research conversation contains the fully runnable downloadable archive **`NOVA_AI_D0L_Offline_Development_Benchmark_Kit.zip`**. Zip SHA-256:
`bfa57afbb33545bb396c5554ae923eddaf84257beaf9d6248602bd087bcc9f9e`.

Archive inspection verified **35 files**: task specifications, starter and reference modules, 75 hidden JavaScript tests, Python evaluator and paired-trial scorer, privacy/policy rules, trial CSV template, self-check, and research reports. The report is also included in the zip. The zip is downloadable from the conversation but is not itself committed to this GitHub repository.

### Verified local tests (Node.js v22.16.0 / Python 3.13.5; no npm installation)

| Synthetic task | Holdout tests | Reference solution passed | Intentionally flawed starter passed |
|---|---:|---:|---:|
| Closed candle integrity/time ordering/staleness | 15 | 15 | 0 |
| Three independent evidence categories plus mandatory risk | 15 | 15 | 1 |
| Two-source time/price reconciliation | 15 | 15 | 2 |
| Paper-only event lifecycle | 16 | 16 | 0 |
| Quota/overflow reservation | 14 | 14 | 1 |
| **Total** | **75** | **75** | **4** |

- **46/46 independent harness self-checks passed**, including syntax/source presence, unsafe-operation-pattern checks, output validation and trial scorer controls.
- All local `.mjs` files passed `node --check`.
- Fake data used *only temporarily* to verify that the scorer accepts a valid synthetic ratio and rejects a policy violation. **No fake trial records are included as actual observations.**
- Empty real-trials file gives the expected fail-closed result: **“2x speed-up cannot be measured without real timed, matched trials.”**

**Reference 75/75 is not a FreeLLMAPI AI test. Starter 4/75 is not an AI model score.**

## Pre-registered real benchmark (NOT EXECUTED)

Five public/synthetic tasks × three matched rounds × two workflows (current direct coding-assistant process and FreeLLMAPI-served model) = 30 timed attempts / 15 matched pairs. Same input spec, hidden tests, reviewer safety criteria, no secret or private NOVA code in prompts. Record full end-to-end time including preparation, response waiting, human review, fixes and retests, plus actual model, quota errors, retries and provider cost.

`measuredSpeedup = median(direct net minutes) / median(gateway net minutes)`.

The user’s **2× goal** requires ratio >= 2, all appropriate hidden tests passing, zero security/privacy/risk violations, at least three complete rounds, and no hidden billing or unreviewed provider fallback. Model differences confound the workflow comparison and must be reported.

## Provider research (official public documentation; no account queries)

- [Groq Your Data](https://console.groq.com/docs/your-data): Zero Data Retention option; account activation, model terms, free limits not verified.
- [Cloudflare Workers AI data policy](https://developers.cloudflare.com/workers-ai/platform/data-usage/): customer content not used to train models. [Pricing](https://developers.cloudflare.com/workers-ai/platform/pricing/): 10,000 daily free Neurons; some models paid-only.
- [OpenRouter pricing](https://openrouter.ai/pricing): free access listed at 50 requests/day and 20 requests/minute; privacy depends on downstream providers, and failed requests can count.

Do not infer unlimited free coding AI, performance, automatic privacy, or approved use from these provider descriptions.

## Safety and next gate

All code/data generated for this benchmark is fictional and separate from NOVA. No NOVA source code, developer credentials, private GitHub repos, trading execution, production Cloudflare Worker, external inference, network calls, new workflows or scheduled tasks were used. The reference/judge code should **not** be sent to a provider.

**Next stage proposal:** separately approve a **one-provider, one-task, one-call synthetic pilot** after confirming the chosen model’s terms, privacy and zero-cost availability. Evaluate model output in a network-disabled, non-root, no-secrets sandbox. Only if successful and explicitly approved consider full 30-attempt paired benchmark.

**NOVA v0.7 production/main, Early Signal MVP, paper-only rules and mandatory risk controls remain unchanged.**
