# Enterprise Databricks Infra

Reusable **platform/infrastructure** companion to [`enterprise-databrick-framework`](https://github.com/ruizengalways/enterprise-databrick-framework).

This repository owns Databricks platform concerns that normal data engineers should not need to touch when an organisation already has its platform in place.

## Ecosystem

- [`data-engineering-cheetsheet`](https://github.com/ruizengalways/data-engineering-cheetsheet) — technology-neutral semantic/design source of truth.
- [`enterprise-databrick-framework`](https://github.com/ruizengalways/enterprise-databrick-framework) — reusable package.
- [`enterprise-databrick-customer`](https://github.com/ruizengalways/enterprise-databrick-customer) — reference workload, learning fixtures and certification evidence.
- **`enterprise-databrick-infra`** — optional platform/IaC baseline (this repo).

For a new conversation or returning platform engineer, start with [`docs/PROJECT_CONTEXT.md`](docs/PROJECT_CONTEXT.md).

## Repository boundary

This repo owns:

- Databricks account/workspace/Unity Catalog infrastructure modules
- workspace/catalog bindings
- deployment and runtime service principals
- GitHub OIDC / workload identity federation
- environment topology and platform configuration
- reusable Declarative Automation Bundle target and GitHub Actions deployment templates
- platform acceptance/deployment patterns

This repo does **not** own pipeline semantic models, P01-P14 runtime handlers, metadata validation, reconciliation/repair semantics or source/domain transformations. Those belong to `enterprise-databrick-framework` or a consuming company's workload repositories.

## Consumption model

A company that already has Databricks infrastructure can ignore this repository completely:

```text
company workload repo
        |
        +--> enterprise-databrick-framework wheel/package
        |
        +--> existing company Databricks platform
```

A greenfield company can selectively reuse this repo:

```text
enterprise-databrick-infra
        |
        +--> Terraform modules
        +--> identity/OIDC
        +--> environment/catalog boundaries
        +--> Bundle / CI-CD templates
```

The framework and infra repositories are deliberately **not runtime-coupled**.

## Directory map

```text
.
├── terraform/
│   └── modules/
├── config/environments/
├── templates/
│   ├── databricks-bundle-targets.yml
│   └── github-actions/
├── docs/
│   ├── adr/
│   ├── runbooks/
│   ├── PLATFORM_FOUNDATION.md
│   ├── PROJECT_CONTEXT.md
│   └── OWNERSHIP.md
└── .github/workflows/
    └── validate.yml
```

Only the validation workflow is active here. Workload deployment workflows are templates because each adopting company/repository owns its own application Bundle, tests, approvals and release cadence.

## Current Databricks guidance reflected here

As of the 2026-08-31 documentation audit:

- **Declarative Automation Bundles** is the current name (formerly Databricks Asset Bundles).
- New Bundles use the **direct deployment engine** by default in current Databricks CLI releases; the Terraform deployment engine is being retired.
- GitHub Actions should use **workload identity federation/OIDC** where possible instead of long-lived Databricks secrets.
- Unity Catalog **workspace-catalog binding** can enforce environment isolation beyond catalog grants.

These are platform choices/reference practices. They are not prerequisites for using the framework package when a company already has equivalent controls.

Platform teams own this repository. Normal dataset onboarding should happen elsewhere.
