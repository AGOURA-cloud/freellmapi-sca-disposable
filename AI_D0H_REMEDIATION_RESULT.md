# AI-D0H — verified compatible lockfile remediation (2026-11-19)

**Verdict: NO-GO for FreeLLMAPI install, deployment, inference, or NOVA integration.**

## Real GitHub audit evidence

- [Experiment run](https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44520037926)
- [Full raw audit and lockfile evidence artifact](https://github.com/AGOURA-cloud/freellmapi-sca-disposable/actions/runs/44520037926/artifacts/14527492803) (7-day retention)
- Original pinned FreeLLMAPI public commit: `9d2b4b407b12e489ecd35320cd630d5ff97cd666`; original Git lockfile blob `1ee6ad8e90aa6288ccc4e03402c63f11e5f7a537`.
- Candidate SHA256 of full patched lockfile: `386b0d7944a331b2931befb37fdfb8faae84629ff5a21255d101c71297050ada`.

## npm audit affected-package entries

| Severity | Original | Four-fix (AI-D0G) | Compatible updates (AI-D0H) |
|---|---:|---:|---:|
| Critical | 2 | 2 | **0** |
| High | 13 | 9 | **0** |
| Moderate | 15 | 15 | **4** |
| Total | **30** | **26** | **4** |

After `npm audit fix --package-lock-only --ignore-scripts` without `--force`, 22 of the prior 26 affected names disappeared; zero new affected names appeared. Both the full and `--omit=dev` audits now show 4 moderate entries. **All five package manifests remained identical**. The lockfile changed 108 resolved package entries. No `node_modules` directory was created and no upstream packages or scripts were executed. There is no runtime compatibility validation.

## Exactly four remaining flagged entries

- `shadcn` 4.23.0 — direct client workspace dependency
- `ts-morph` 26.0.0 — transitive through `shadcn`
- `@ts-morph/common` 0.27.0 — transitive through `ts-morph`
- `minimatch` 10.2.6 — transitive under `@ts-morph/common`, DoS advisory GHSA-g3hv-75wg-m249

npm suggests `shadcn@1.0.0` to resolve the chain, but that is a major downgrade relative to declared `^4.2.0`. The pinned client stylesheet imports `shadcn/tailwind.css`, so removing or downgrading `shadcn` blindly could break the client build. **No forced or breaking upgrade was attempted.**

## Scope and decision

The job's last safety step intentionally failed because four advisories remain. Both the temporary path-specific workflow and launch marker have been deleted. The original manual-only audit workflow and both earlier experiment records remain.

**Next AI-D0I:** assess a supported fix or isolation for the client-side `shadcn` chain, separate server-only SBOM/reachability, and request separate approval before any third-party install/build or AI model call.

Independent local evidence checks: **22/22** passed. These prove artifact/parsing coherence, not application security. NOVA production, MVP, trading rules, and risk rules remain unchanged. Experimental 2x development acceleration remains unmeasured.
