# Auto Approve and Merge

This action is will have a github app approve and merge a pull request into a branch that has branch protection enabled.
Specific settings that needed workarounds:
- Require CODEOWNER approval
- Require Merge queue

This has hacky logic implemented to get around the fact that apps cannot be added as CODEOWNERS or be added to teams.

<!-- action-docs-description -->
## Description

Auto approve and merge
<!-- action-docs-description -->

<!-- action-docs-inputs -->
## Inputs

| parameter | description | required | default |
| --- | --- | --- | --- |
| token | The GitHub token to use for authentication | `false` |  |
| application_id | The GitHub App ID | `false` |  |
| application_private_key | The GitHub App private key | `false` |  |
| branch | The branch to merge into | `false` | main |
| checks | The checks to wait for before merging | `false` | checks:   - context: 'e2e-tests'   - context: 'pre-commit-checks' |
<!-- action-docs-inputs -->

<!-- action-docs-outputs -->

<!-- action-docs-outputs -->

<!-- action-docs-runs -->
## Runs

This action is a `composite` action.
<!-- action-docs-runs -->
