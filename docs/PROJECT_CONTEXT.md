# Project Context — Enterprise Databricks Infra

Last architecture/documentation audit: **2026-08-31**.

Read this file first when resuming platform work in a new conversation.

## Repository role

This repository is an **optional platform/IaC reference implementation**. It is intentionally separate from reusable framework runtime code and from customer/domain workload code.

| Repository | Responsibility |
|---|---|
| `data-engineering-cheetsheet` | technology-neutral data pipeline semantics |
| `enterprise-databrick-framework` | reusable data-engineering package |
| `enterprise-databrick-customer` | reference workload + learning/certification evidence |
| `enterprise-databrick-infra` | Terraform/platform identity/environment/deployment templates |

## Non-negotiable boundary

A company with an existing Databricks platform must be able to ignore this repo and still use the framework package.

Normal data engineers should not edit Terraform merely to onboard a new dataset. Dataset semantics, source adapters, expected outputs and domain transformations belong in workload/customer repos.

## Current platform reference decisions

1. Terraform owns long-lived platform/admin resources such as catalog foundations, workspace bindings and automation identities where this reference implementation provisions them.
2. Workload resource deployment should use Declarative Automation Bundles in the consuming workload repo; this infra repo provides templates, not the workload itself.
3. Current Bundles use the direct deployment engine; do not make the deprecated Terraform Bundle engine a new dependency.
4. GitHub automation should use Databricks workload identity federation/OIDC when possible; avoid long-lived PAT/client-secret storage.
5. Deployment identities and runtime identities are conceptually distinct and should receive least privilege.
6. Workspace-catalog bindings may be used to isolate production catalogs from non-production workspaces.
7. Environment/catalog names in this repo are examples/reference defaults, not framework API requirements.
8. No infrastructure state or environment-specific identifier belongs in `enterprise-databrick-framework`.

## Current implementation state

At this audit point this repo contains:

- `unity_catalog_environment` Terraform module;
- `workspace_binding` Terraform module;
- `github_oidc_service_principal` Terraform module;
- DEV/CI/UAT/PROD example environment configuration;
- Declarative Automation Bundle target and GitHub Actions templates;
- Terraform validation CI.

The Terraform validation workflow has passed on GitHub Actions. This does **not** mean the modules have been applied to every cloud/account; it proves syntax/provider validation for this reference code.

## How this repo interacts with certification

`enterprise-databrick-customer` owns independent framework certification evidence. When real Databricks C3-C5 certification is executed, this repo may supply an optional reference environment, identity and deployment template. Certification must still record exact framework/customer SHAs and cannot be inferred from Terraform validation.

## Resume checklist

Before changing platform behavior:

1. read this file and `docs/OWNERSHIP.md`;
2. inspect current `main`, open PRs and Terraform CI;
3. check current Databricks official documentation for features that change quickly (Bundles, authentication, provider/resource support);
4. do not move customer/workload semantics into Terraform;
5. update this file if a platform boundary or major reference decision changes.
