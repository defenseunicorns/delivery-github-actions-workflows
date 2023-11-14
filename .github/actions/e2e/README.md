# e2e test

This action runs e2e tests inside the buildharness container

<!-- action-docs-description -->
## Description

Run E2E Tests
<!-- action-docs-description -->

<!-- action-docs-inputs -->
## Inputs

| parameter | description | required | default |
| --- | --- | --- | --- |
| token | The GitHub token to use for authentication | `false` |  |
| application_id | The GitHub App ID | `false` |  |
| application_private_key | The GitHub App private key | `false` |  |
| region | The AWS region to deploy to | `true` |  |
| role-to-assume | The AWS IAM Role to assume in the target account | `true` |  |
| send-status | Whether to send status updates to GitHub | `false` | true |
| github-context | The GitHub Status Context to use when updating the status | `false` | default e2e test context |
| make-target | The make target to run | `true` |  |
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
