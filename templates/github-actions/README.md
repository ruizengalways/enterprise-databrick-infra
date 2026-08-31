# GitHub Actions templates

These are **copy/adapt templates** for a consuming Databricks workload repository. They are intentionally not active workflows in this infra repository.

They demonstrate the platform contract for:

- GitHub OIDC authentication
- exact-SHA deployment
- shared DEV deployment
- UAT/PROD promotion
- isolated PR Bundle namespaces and cleanup

A consuming repo replaces validation/test commands with its own commands and supplies its own `databricks.yml`. The reusable framework wheel may be one dependency, but these templates do not require it.
