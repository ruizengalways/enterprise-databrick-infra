# Project Context — Enterprise Databricks Infra

This is the **human-readable** platform context. Machine-readable ownership and implementation state live in `project/repository.yml` and `project/capabilities.yml`.

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

Normal data engineers should not edit Terraform merely to onboard a new dataset. Dataset semantics, source adapters, expected outputs, and domain transformations belong in workload/customer repos.

## Platform reference decisions

1. Terraform owns long-lived platform/admin resources that this reference implementation provisions.
2. Workload deployment belongs in the consuming workload repo; this repo may provide templates.
3. GitHub automation should prefer workload identity federation/OIDC over long-lived PATs or client secrets.
4. Deployment and runtime identities are distinct security concepts and should be least privilege.
5. Workspace-catalog bindings may be used to isolate production catalogs from non-production workspaces.
6. Environment/catalog names in this repo are examples, not framework API requirements.
7. No infrastructure state or environment-specific identifier belongs in `enterprise-databrick-framework`.

Fast-changing Databricks product details such as Bundle deployment behavior, authentication, provider resource support, serverless/runtime features, and workspace bindings must be checked against current official documentation before material changes.

## Certification boundary

`enterprise-databrick-customer` owns independent framework certification evidence. This repo may supply a reference environment and deployment template for real Databricks C3-C5 runs, but Terraform validation does not itself certify framework runtime correctness.

## Human vs machine documentation

```text
Human narrative
  README.md
  docs/**/*.md

Machine repository/capability state
  project/**/*.yml
```

Automation must read `project/capabilities.yml` instead of parsing prose for current implementation state.

## Resume checklist

For a human:

1. read this file and `docs/OWNERSHIP.md`;
2. inspect relevant ADRs/runbooks;
3. verify current Databricks platform guidance before implementation changes.

For an automated agent/new conversation:

1. read `project/repository.yml` and `project/capabilities.yml`;
2. read the customer repo's `project/context.yml` and `project/state.yml`;
3. inspect current `main`, open PRs, and CI;
4. do not move workload semantics into Terraform.

Current SHAs and dynamic certification status are intentionally not duplicated in this Markdown.
