# Enterprise Databricks Infra

Reusable **platform/infrastructure** companion to [`enterprise-databrick-framework`](https://github.com/ruizengalways/enterprise-databrick-framework).

This repository owns Databricks platform concerns that a normal data engineer should not need to touch when an organisation already has its platform in place.

## Repository boundary

This repo owns:

- Databricks account/workspace/Unity Catalog infrastructure modules
- workspace/catalog bindings
- deployment and runtime service principals
- GitHub OIDC / workload identity federation
- environment topology and platform configuration
- Declarative Automation Bundle environment targets used for platform acceptance/deployment
- DEV/UAT/PROD promotion and PR-environment cleanup automation
- platform acceptance harnesses that prove the reusable framework works on real Databricks infrastructure

This repo does **not** own:

- pipeline semantic models and P01-P14 contracts
- reusable ingestion/SCD/CDC runtime handlers
- metadata validation
- reconciliation/repair business semantics
- source/domain-specific transformations

Those belong to `enterprise-databrick-framework` or to the consuming company's workload repositories.

## Consumption model

A company that already has Databricks infrastructure can ignore this repository completely and consume only the framework package.

```text
company workload repo
        |
        +--> enterprise-databrick-framework wheel/package
        |
        +--> existing company Databricks platform
```

A company that needs a platform baseline can selectively reuse modules from this repo:

```text
enterprise-databrick-infra
        |
        +--> Terraform modules
        +--> identity/OIDC
        +--> environment/catalog boundaries
        +--> deployment/promotion examples
```

The two repositories are deliberately **not runtime-coupled**. Infrastructure may exist before the framework is adopted, and the framework must remain usable when this repo is absent.

## Directory map

```text
.
├── terraform/
│   └── modules/
├── config/environments/
├── bundles/
│   ├── platform/
│   └── acceptance/
├── acceptance/
├── docs/
│   ├── adr/
│   └── runbooks/
└── .github/workflows/
```

Platform teams own this repository. Normal dataset onboarding should happen elsewhere.
