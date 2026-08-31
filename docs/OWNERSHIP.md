# Ownership model

This repository is owned by platform/infrastructure engineers.

Normal data-engineering work should not require changes here. Dataset onboarding, CDC/SCD semantics, metadata contracts, source fixtures, expected outcomes and domain transformations belong in consuming workload repositories using `enterprise-databrick-framework`.

`enterprise-databrick-customer` is the reference example of such a consuming workload and is also the independent certification/learning repository.

A change belongs here when it concerns long-lived Databricks platform resources or organisation-wide deployment identity/boundaries, for example workspaces, catalogs, base schemas, workspace bindings, service principals, federation policies or shared environment/deployment templates.

If a company already provides those capabilities, this repository is optional.
