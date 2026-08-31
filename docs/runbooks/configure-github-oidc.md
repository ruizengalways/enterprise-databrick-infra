# Configure GitHub Workload Identity Federation for Databricks

Use this as a reference platform runbook. Apply organisation-specific security review and least-privilege policy before production use.

## Why

Databricks recommends workload identity federation for automated workloads where possible because it exchanges a GitHub OIDC token for short-lived Databricks OAuth credentials instead of storing long-lived Databricks secrets.

## Federation policy

For a GitHub environment-scoped policy, the typical issuer is:

```text
https://token.actions.githubusercontent.com
```

A typical subject is:

```text
repo:<owner>/<repository>:environment:<environment>
```

Databricks recommends using a GitHub Environment for this type of deployment boundary. The federation policy subject must exactly match the token subject.

For `audiences`, current Databricks guidance recommends the Databricks account ID; if omitted, the account ID is the default in the federation API. The Terraform module in this repo requires the account ID explicitly so the trust boundary is visible.

## GitHub workflow authentication

A GitHub Actions job using federation normally has:

```yaml
permissions:
  id-token: write
  contents: read

env:
  DATABRICKS_AUTH_TYPE: github-oidc
  DATABRICKS_HOST: ${{ vars.DATABRICKS_HOST }}
  DATABRICKS_CLIENT_ID: ${{ vars.DATABRICKS_CLIENT_ID }}
```

`DATABRICKS_CLIENT_ID` is the Databricks service-principal application ID. The Databricks CLI/SDK can then obtain the GitHub workload identity token and exchange it for a Databricks OAuth token.

## Suggested bootstrap sequence

1. Create or identify the Databricks account/workspace/metastore using the organisation's approved admin bootstrap process.
2. Create a dedicated Databricks service principal for deployment automation.
3. Create a GitHub federation policy scoped to the intended repository/environment or approved reusable workflow.
4. Grant only the workspace/account privileges required for deployment.
5. Create a separate runtime service principal where separation of deployment/runtime duties is required.
6. Configure GitHub Environment protection/approvals according to policy.
7. Store non-secret environment identifiers as GitHub variables where appropriate.
8. Test federation with a minimal command such as `databricks current-user me` before allowing deployment.
9. Validate and deploy the workload Bundle using the exact reviewed source/artifact.
10. Record release/certification evidence outside Terraform state.

## Reusable workflows

Databricks also supports federation using the `job_workflow_ref` subject claim for a trusted reusable GitHub workflow. Use that model when centralising deployment logic across many repositories; do not copy a broad federation policy to every repository merely for convenience.

## Incident response

If a CI/CD identity or trust policy is suspected to be compromised:

1. disable/remove the federation policy;
2. disable the service principal if necessary;
3. review workspace/account/Unity Catalog grants and audit logs;
4. inspect recent GitHub workflow/deployment history;
5. recreate a least-privilege policy/identity as required;
6. revalidate a known-good immutable release before resuming promotion.

OIDC removes a long-lived Databricks secret, but it does not remove the need to protect GitHub environments, workflows, repository write access and Databricks privileges.
