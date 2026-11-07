# BA-SEC-DEP-001 — baobab-scf dependency and security maintenance audit

**Review date:** 7 November 2026. **Repository:** `baobab-platform/baobab-scf`. **Status:** SOURCE_COMMITTED only.

## Observed manifests and Dependabot coverage

GitHub Actions / ONLY. The source tree contains no Cargo.toml, pom.xml, go.mod, pyproject.toml, package.json, Dockerfile or Compose manifest.

## Actual dependency/security surveillance

Existing pinned Action enforcement and reusable secret scanning remain relevant. No application package audit can be performed against an application lockfile because no application package manifest exists. Never create an empty, fake npm/Python/Rust ecosystem or claim there are vulnerability-free SCF dependencies.

## Findings, unqualified boundaries and exception custody

SCF is a template/scaffold with a repository.yaml.example, not a registered native runtime or activated service. Before implementation, introduce owned manifests, lockfiles, manifest-aware Dependabot coverage, actual SAST/dependency scans, IAM/CP governance and native positive/negative evidence. Current repo is a maintenance policy template only.

**Current disposition:** Actions source-policy coverage only; SCF native security/finance provider qualification not applicable/not yet demonstrated.

## Governance and evidence levels

Governing authority: [approved BA-SEC-DEP-001 v1.0, Shared #429](https://github.com/baobab-platform/shared/pull/429). The proposed stricter Shared validator is NOT released; this branch uses the SHA-pinned accepted Shared v2.3.0 structural validator and an owner-local positive/negative source suite. Weekly Monday Africa/Johannesburg version updates are distinct from advisory-driven security response. Routine groups apply only to minor/patch version-updates; major or security updates require independent review.

Evidence state **SOURCE_COMMITTED** means source reviewed/committed, not executed. **CI_VERIFIED**, **SECURITY_ALERT_PATH_VERIFIED**, **CONSUMER_QUALIFIED**, **STAGING_VERIFIED**, and **PRODUCTION_VERIFIED** are NOT claimed. Findings are **BLOCKED** pending an independently approved remediation or expiring exception. **WAIVED is not PASSED**. Security CODEOWNERS review applies to manifest and exception changes.

## Exit criteria

1. Run the actual final-head policy regression and accepted Shared validator; inspect live GitHub Dependabot job logs, dependency graph, Alerts, automatic security update settings and actual dependency scan results.
2. Repair reported security vulnerabilities promptly using real installed package/image provenance; independent time-bounded evidence and Security approval are required for any unpatched exceptions. Do not permit scanner errors, no-ops or ignored advisories to appear as success.
3. Re-run owner-specific native runtime, IAM/CP authority, supplier/consumer isolation, replay, migration and negative acceptance tests after a major or security update. No synthetic PSP, carrier, rule or issuer success counts as a real integration.
4. Obtain separate staging/production approvals and deployment evidence. No protected environment, credential, provider or release is activated by this policy adoption.

No CI run or monitoring, PR merge or protected business approval has been claimed.
