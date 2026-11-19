# AI-D0N-C — GroqCloud free-tier public verification (read-only)

Date checked: 2026-11-19
Project scope: NOVA v0.7 AI-assisted **development research**, separate disposable FreeLLMAPI gateway.
Status: **PUBLIC-DOCS PASS / ACCOUNT-ACCESS PENDING / LIVE INFERENCE NO-GO**.

## Public facts verified from official Groq documentation

- GroqCloud has a **Free Plan**; paid **Developer** upgrade requires a payment method and results in usage billing. https://console.groq.com/docs/billing-faqs
- For `openai/gpt-oss-120b`, the published Free Plan limits are **30 RPM, 1,000 RPD, 8,000 TPM, 200,000 TPD**, subject to organization-specific exceptions. https://console.groq.com/docs/rate-limits
- Rate limits apply to organizations and can include separate input/output token caps; exact applicable figures appear in the signed-in organization Limits page. https://console.groq.com/docs/rate-limits
- Free-tier request overages receive `429` according to rate-limit docs; confirm the actual account tier instead of assuming it. https://console.groq.com/docs/rate-limits
- Projects can have independent API keys and *lower*, never higher, model limits; organization-level ceiling still applies. https://console.groq.com/docs/projects
- Input/output retention for inference can be **up to 30 days** for reliability/abuse monitoring unless Zero Data Retention (ZDR) is enabled; ZDR is offered to customers in Data Controls. https://console.groq.com/docs/your-data
- Free Plan does not expose paid-tier spend limits; avoiding paid-tier upgrade is therefore essential rather than relying on a $0 spend-limit control. https://console.groq.com/docs/spend-limits
- Official settings locations: https://console.groq.com/settings/limits and https://console.groq.com/settings/data-controls

## User-account checks still required

1. User signs in to https://console.groq.com in their own Android Chrome browser.
2. Confirm account **Free Plan**; do not upgrade and do not enter payment details.
3. Inspect signed-in Organization Limits and verify model `openai/gpt-oss-120b` availability, actual RPM/RPD/TPM/TPD and any separate ITPM/OTPM.
4. In Data Controls, enable ZDR if user wishes to send proprietary synthetic code; initially use only non-sensitive synthetic prompts regardless.
5. No API key created or shared in public repositories, chat, logs, or screenshots; if a key is later needed, it must only be stored in a secure secret store or private execution environment, with explicit approval for credential creation.
6. Geographic/account eligibility must be confirmed in account UI; not established by public documentation.

## Safety gates before AI-D0M

- Provider account Free Plan confirmed, billing/credit-card absent, no auto-upgrade or paid fallback configured.
- Explicit user approval for exactly one live synthetic inference call **after** the checks.
- Enforce one request, model allowlist, small max tokens, no tools, no external web, no background health-probes, no retries, no provider-fallback, no arbitrary code execution, abort on unexpected billing/rate-limit/auth errors.
- No NOVA production/development Worker integration and no main-branch modification.
- Prior AI-D0N-B offline local gateway success: https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44572299947 ; 26/26 tests passed. This DOES NOT verify an external AI provider or real account allowance.

## Decision

GroqCloud is a good **conditional** first provider candidate. **No live call, no quota consumption, and no account-specific allowance validation has occurred.**
