# ADR-002: Keep Terraform state aligned to platform ownership

Status: Accepted

Terraform owns long-lived platform resources such as catalogs, base schemas, workspace bindings, service principals and federation policies. Workload Jobs/Pipelines and dataset semantics are not Terraform-owned by default.

Cloud-specific workspace/network/storage bootstrap is intentionally outside the generic modules in this repository and should be composed by the adopting organisation.
