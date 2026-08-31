# Reference Platform Foundation

This document describes an **optional reference topology**, not a requirement imposed by `enterprise-databrick-framework`.

## Example environment model

```text
Personal developer deployment
  target: dev
  example catalog: edp_dev
  run-as: developer

Ephemeral PR integration
  target: ci
  example catalog: edp_ci
  namespace: pr-<number>
  run-as: CI runtime service principal

Stable shared development
  target: shared_dev
  example catalog: edp_dev
  run-as: DEV runtime service principal

UAT
  target: uat
  example catalog: edp_uat
  run-as: UAT runtime service principal

PROD
  target: prod
  example catalog: edp_prod
  run-as: PROD runtime service principal
```

A company may use a different account/workspace/catalog topology. The framework package must not depend on these names.

## Identity separation

For shared environments, keep deployment and runtime responsibilities separable:

```text
GitHub deployer service principal
  -> authenticates using workload identity federation/OIDC
  -> validates/plans/deploys workload resources

Databricks runtime service principal
  -> configured as workload run identity
  -> receives only required data/runtime privileges
```

The deployer should not automatically become the privileged production data-plane identity.

## Unity Catalog

The reference Terraform module creates an isolated environment catalog and base schemas such as Bronze/Silver/Gold/reference/quarantine/platform-control. These are examples, not framework requirements.

Workspace-catalog binding is managed separately so production/data-domain isolation can be changed without coupling it to catalog replacement. Databricks documents that an unbound workspace is denied access to a restricted catalog even when a principal otherwise has catalog grants.

## Declarative Automation Bundles

Use the current term **Declarative Automation Bundles**. Databricks has renamed the previous Databricks Asset Bundles terminology.

Current Databricks CLI releases use the **direct deployment engine** by default for new Bundles. The direct engine does not depend on Terraform and Databricks is retiring the old Terraform Bundle deployment engine. It is valid to set `bundle.engine: direct` explicitly in templates when you want the intent visible.

Terraform in this repo therefore owns platform/admin infrastructure; consuming workload Bundles own Jobs/Pipelines and their deployable application resources. Do not make both mechanisms authoritative for the same resource.

## Promotion invariant

A production-oriented release should promote an immutable artifact/exact Git SHA rather than silently rebuilding different source for each environment:

```text
SHA X / artifact X
 -> validate
 -> shared DEV
 -> UAT
 -> PROD
```

The exact approval topology is company policy and is not enforced by the framework package.

## Reference customer

Use `enterprise-databrick-customer` to prove that a workload can consume the framework independently of this repo. Real runtime certification should deploy the customer workload into an approved Databricks environment and retain evidence tied to exact SHAs.
