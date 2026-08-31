# ADR-001: Separate framework package from platform infrastructure

Status: Accepted

## Decision

`enterprise-databrick-framework` owns reusable data-engineering semantics and runtime code. `enterprise-databrick-infra` owns environment/platform provisioning and deployment identity concerns.

A consuming company must be able to use the framework without this infra repository when equivalent infrastructure already exists.

## Consequences

- Normal dataset engineers do not need Terraform access.
- Platform changes have a separate review/ownership path.
- The framework package cannot assume catalog names, workspace IDs, GitHub Environments or OIDC identities.
- Infra templates may reference the framework conceptually, but the two repos must not form a mandatory runtime dependency cycle.
