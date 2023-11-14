# Autoformat

This action will run the buildharness that will run pre-commit and commit the changes to the repo using graphql,

<!-- action-docs-description -->
## Description

Autoformat code using pre-commit
<!-- action-docs-description -->

<!-- action-docs-inputs -->
## Inputs

| parameter | description | required | default |
| --- | --- | --- | --- |
| token | The GitHub token to use for authentication | `false` |  |
| application_id | The GitHub App ID | `false` |  |
| application_private_key | The GitHub App private key | `false` |  |
| github-context | The GitHub Status Context to use when updating the status | `true` |  |
| buildharness_version | The version of buildharness used by the caller repo | `true` |  |
| golang_version | The version of golang used by the buildharness release from the caller repo | `true` |  |
| terraform_version | The version of terraform used by the buildharness release from the caller repo | `true` |  |
| zarf_version | The version of zarf used by the buildharness release from the caller repo | `true` |  |
<!-- action-docs-inputs -->

<!-- action-docs-outputs -->

<!-- action-docs-outputs -->

<!-- action-docs-runs -->
## Runs

This action is a `composite` action.
<!-- action-docs-runs -->
