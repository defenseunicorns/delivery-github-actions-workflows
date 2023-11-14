# Get Buildharness Software Versions

This action will get the buildharness software versions of the caller repo. Used to determine the versions of buildharness container, golang, terraform, and zarf used by the caller repo for github cache purposes.

<!-- action-docs-description -->
## Description

This action gets the versions of the tools used by buildharness and outputs them as environment variables.
<!-- action-docs-description -->

<!-- action-docs-inputs -->
## Inputs

| parameter | description | required | default |
| --- | --- | --- | --- |
| token | The GitHub token to use for authentication | `false` |  |
| application_id | The GitHub App ID | `false` |  |
| application_private_key | The GitHub App private key | `false` |  |
<!-- action-docs-inputs -->

<!-- action-docs-outputs -->
## Outputs

| parameter | description |
| --- | --- |
| buildharness_version | The version of buildharness used by the caller repo |
| golang_version | The version of golang used by buildharness |
| terraform_version | The version of terraform used by buildharness |
| zarf_version | The version of zarf used by buildharness |
<!-- action-docs-outputs -->

<!-- action-docs-runs -->
## Runs

This action is a `composite` action.
<!-- action-docs-runs -->
