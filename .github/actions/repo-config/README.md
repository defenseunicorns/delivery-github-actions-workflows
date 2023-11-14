# repo-config

This action uses github's rest API to configure repo settings and branch protection in a standardized way.

<!-- action-docs-description -->
## Description

Configure a repository and branch protection with a github app
<!-- action-docs-description -->

<!-- action-docs-inputs -->
## Inputs

| parameter | description | required | default |
| --- | --- | --- | --- |
| token | The GitHub token to use for authentication | `false` |  |
| application_id | The GitHub App ID | `false` |  |
| application_private_key | The GitHub App private key | `false` |  |
| branch | Branch to configure | `false` | main |
| checks | YAML formatted required status checks | `false` | checks:   - context: 'e2e-tests'   - context: 'pre-commit-checks' |
| require_code_owner_reviews | Require code owner reviews | `false` | true |
<!-- action-docs-inputs -->

<!-- action-docs-outputs -->

<!-- action-docs-outputs -->

<!-- action-docs-runs -->
## Runs

This action is a `composite` action.
<!-- action-docs-runs -->
