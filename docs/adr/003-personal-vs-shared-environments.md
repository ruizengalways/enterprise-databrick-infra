# ADR-003: Separate personal development from shared promotion environments

Status: Accepted

Personal `dev` is developer-owned and may run as the current user. Shared DEV, UAT and PROD are release environments and run as stable service principals. PR CI is isolated and disposable.

This distinction belongs to the platform/deployment layer, not to the reusable framework package.
