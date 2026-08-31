# Terraform boundary

These modules are optional platform building blocks. They are deliberately separate from `enterprise-databrick-framework` so a company with an existing Databricks platform can adopt the framework package without adopting this IaC.

The modules assume the Databricks provider is configured by the calling stack. Cloud-specific workspace, network, storage and metastore bootstrap remain outside these generic modules because AWS, Azure and GCP differ materially.

## Modules

- `unity_catalog_environment`: isolated environment catalog, stable base schemas and runtime grants.
- `workspace_binding`: workspace-to-catalog/external-location/storage-credential isolation.
- `github_oidc_service_principal`: Databricks-managed service principal plus GitHub Actions workload identity federation.

Platform teams compose these modules into their own state stacks. Dataset engineers should not edit them during normal onboarding.
