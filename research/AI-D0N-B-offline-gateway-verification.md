# AI-D0N-B — Offline FreeLLMAPI Gateway Verification Checkpoint

Date: 2026-11-19
Scope: disposable GitHub repository only. No NOVA production changes.

## Verified evidence

- Successful GitHub Actions run: https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44572299947
- Successful artifact ID: `14555555684` (GitHub artifact retention is temporary).
- Pinned third-party source commit: `9d2b4b407b12e489ecd35320cd630d5ff97cd666`.
- Pinned candidate package-lock SHA-256: `dc7799e8134eb428d38857b6392b9cb499784416cebd0e9b6fe179091138da36`.
- npm audit advisories: 0 reported (info/low/moderate/high/critical all zero).
- Executed local HTTP/MCP gateway assertions: **26/26 PASS**, individually verified in artifact `offline-gateway.log`.
- Offline gateway exit code: `0`. GitHub workflow conclusion: `success`.

## Controlled iteration and root causes

1. The original AI-D0N run https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44545887545 failed before executing checks due to an incorrectly indented shell heredoc terminator.
2. The corrected first AI-D0N-B run https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44571620722 completed 5 checks and stopped at the hostile Host-header check. The probe used Node `fetch()`, whose Undici client rewrites the Host header. This is also explicitly documented in the upstream source test `server/src/__tests__/routes/allowed-hosts.test.ts` at the pinned commit.
3. The final fix changed only the **test probe** to send a raw Host header using `node:http`. The original upstream host-header guard was **not changed or bypassed**. The subsequent run passed all 26 checks.

## Boundaries

- Gateway ran on localhost in an ephemeral container with outbound network disabled during gateway tests.
- No upstream AI provider keys used, no live inference requests made, no live free-tier quota validated.
- No NOVA integration, production deployment, trading changes, or scheduled scanning authorized or performed.
- Passing these local functional/security checks is **not** proof of production safety, provider free allowance, or live API reliability.

## Next gates

1. Read-only verification of a legitimate free inference provider, terms, actual quota, privacy handling, and billing safeguards.
2. Explicit approval for a single synthetic **AI-D0M** live inference request once those checks are satisfactory.
3. Only thereafter, independent **AI-D0O** coding benchmark against our prepared synthetic tests.
