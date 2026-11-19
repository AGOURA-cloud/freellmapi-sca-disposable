# AI-D0M — private GitHub Actions environment preparation checkpoint

Date: 2026-11-19
Status: **PREPARED OFFLINE / PRIVATE REPOSITORY NEEDED / LIVE INFERENCE NO-GO**

## User-verified Groq account restrictions
- Account screenshot: GroqCloud **Free $0 current plan**.
- User enabled **Inference APIs Zero Data Retention**.
- User created separate project `NOVA-AI-D0-TEST`.
- Only model **`openai/gpt-oss-120b`** allowed in that project.
- Project limits confirmed in screenshot: **1 RPM; 5 RPD; 2,000 TPM; 10,000 TPD**.

## Offline gateway evidence
- Disposable branch `experiment/ai-d0n-b-offline-gateway-fix` 26/26 passing offline verification:
  https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44572299947
- FreeLLMAPI source commit pinned to `9d2b4b407b12e489ecd35320cd630d5ff97cd666`.
- Preservation of a previously audited dependency lockfile with SHA256 `dc7799e8134eb428d38857b6392b9cb499784416cebd0e9b6fe179091138da36`; original GitHub Actions evidence artifact ID `14529460580`.
- The artifact is also preserved in the AI-D0M private preparation ZIP supplied within the ChatGPT conversation.

## Prepared local kit (not uploaded to any private repo yet)
- `.github/workflows/ai-d0m-private-preflight.yml` — **manual-only**, no inference or secrets access, GitHub read permission.
- `scripts/preflight.mjs` — checks invariants and preserved lockfile hash.
- `policy.json` — live permission **false**, retries **0**, no paid fallback, no background health probes allowed, model pinned.
- `evidence/pinned-audited-package-lock.json` — SHA-256 verified.
- `docs/AI-D0M-LIVE-REVIEW-CHECKLIST.md` — future security review gates.

Local static Node preflight and syntax validation both passed. That does not mean a private GitHub Actions run or a live gateway has passed.

## Critical access and safety constraints
- Connected GitHub tooling can read/write files and branches but **cannot create a new repository or create Actions secrets**. Existing private NOVA production repo must remain unchanged.
- Next user action: create a dedicated **PRIVATE** GitHub repository (suggested `AGOURA-cloud/nova-ai-d0m-private-test`), then give the GitHub connector access so ChatGPT can upload preflight files without Android unzip/copy-paste work.
- **Do not generate or share a Groq API key yet**. No Actions secret is configured, no live provider calls were made.
- Pin all source and dependencies; audit all FreeLLMAPI background networking and possible secret-exfiltration paths **before** exposing a credential to the runtime.
- Never let routine GitHub workflow events auto-execute a live request. Future live workflow requires separate explicit user authorization for exactly one synthetic call through FreeLLMAPI.
- No modifications to NOVA production, Early Signal MVP, or risk rules.

Sources:
- Official GitHub repository secret instructions: https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets?tool=webui
- Official GitHub Actions secure-use guidelines: https://docs.github.com/en/actions/reference/security/secure-use
