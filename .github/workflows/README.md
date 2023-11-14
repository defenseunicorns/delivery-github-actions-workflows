- [E2E Test Workflow](#e2e-test-workflow)
  - [Workflow Configuration](#workflow-configuration)
    - [Secrets](#secrets)
    - [Inputs](#inputs)
  - [Jobs](#jobs)
    - [`set-test-vars`](#set-test-vars)
    - [`comment`](#comment)
    - [`get-buildharness-software-versions`](#get-buildharness-software-versions)
    - [`ping`](#ping)
    - [`determine-matrix`](#determine-matrix)
    - [`matrix_debug`](#matrix_debug)
    - [`e2e-test`](#e2e-test)
    - [`report-e2e-test-status`](#report-e2e-test-status)
  - [Usage](#usage)
- [PR Merge Group Test Workflow](#pr-merge-group-test-workflow)
  - [Workflow Configuration](#workflow-configuration-1)
    - [Secrets](#secrets-1)
    - [Inputs](#inputs-1)
  - [Jobs](#jobs-1)
    - [`report-success-status`](#report-success-status)
    - [`get-changes`](#get-changes)
    - [`get-branch-name`](#get-branch-name)
    - [`auto-e2e-test-report-success`](#auto-e2e-test-report-success)
    - [`common-e2e-test`](#common-e2e-test)
    - [`release-e2e-test`](#release-e2e-test)
  - [Usage](#usage-1)
- [Pre-commit Workflow](#pre-commit-workflow)
  - [Workflow Configuration](#workflow-configuration-2)
    - [Secrets](#secrets-2)
    - [Inputs](#inputs-2)
  - [Jobs](#jobs-2)
    - [`set-test-vars`](#set-test-vars-1)
    - [`get-buildharness-software-versions`](#get-buildharness-software-versions-1)
    - [`pre-commit-checks`](#pre-commit-checks)
    - [`report-pre-commit-test-status`](#report-pre-commit-test-status)
  - [Usage](#usage-2)
- [Release-Please Workflow](#release-please-workflow)
  - [Workflow Configuration](#workflow-configuration-3)
    - [Secrets](#secrets-3)
  - [Jobs](#jobs-3)
    - [`release-please`](#release-please)
  - [Usage](#usage-3)
- [Renovate Test Workflow](#renovate-test-workflow)
  - [Workflow Configuration](#workflow-configuration-4)
    - [Secrets](#secrets-4)
    - [Permissions](#permissions)
    - [Defaults](#defaults)
  - [Jobs](#jobs-4)
    - [`renovate-test`](#renovate-test)
  - [Usage](#usage-4)
- [Repo-Config Workflow](#repo-config-workflow)
  - [Workflow Configuration](#workflow-configuration-5)
    - [Secrets](#secrets-5)
    - [Inputs](#inputs-3)
    - [Jobs](#jobs-5)
      - [`repo-config`](#repo-config)
    - [Usage](#usage-5)
- [Report-Status Workflow](#report-status-workflow)
  - [Workflow Configuration](#workflow-configuration-6)
    - [Secrets](#secrets-6)
    - [Inputs](#inputs-4)
    - [Jobs](#jobs-6)
      - [`report-status-checks`](#report-status-checks)
    - [Usage](#usage-6)
- [Secure Test with ChatOps Workflow](#secure-test-with-chatops-workflow)
  - [Workflow Configuration](#workflow-configuration-7)
    - [Secrets](#secrets-7)
    - [Jobs](#jobs-7)
      - [`get-buildharness-software-versions`](#get-buildharness-software-versions-2)
      - [`e2e-govcloud-secure`](#e2e-govcloud-secure)
      - [`send-slack-message`](#send-slack-message)
    - [Usage](#usage-7)
- [Update Workflow](#update-workflow)
  - [Workflow Configuration](#workflow-configuration-8)
    - [Secrets](#secrets-8)
    - [Jobs](#jobs-8)
      - [`parse`](#parse)
      - [`comment`](#comment-1)
      - [`ping`](#ping-1)
      - [`autoformat`](#autoformat)
    - [Usage](#usage-8)


# E2E Test Workflow

This reusable workflow is designed for end-to-end (E2E) testing on GitHub Actions. It includes various jobs to set test variables, update comments, determine test matrices, and run E2E tests in a matrix of regions and make targets.

## Workflow Configuration

This workflow is triggered on a `workflow_call` event.

### Secrets

| Secret Name                   | Description                           | Required |
| ----------------------------- | ------------------------------------- | -------- |
| `TOKEN`                       | GitHub App Token                      | No       |
| `APPLICATION_ID`              | The GitHub App ID                     | No       |
| `APPLICATION_PRIVATE_KEY`     | The GitHub App Private Key            | No       |
| `AWS_COMMERCIAL_ROLE_TO_ASSUME` | AWS IAM Role to assume              | Yes      |
| `AWS_GOVCLOUD_ROLE_TO_ASSUME` | AWS GovCloud IAM Role to assume       | Yes      |

### Inputs

| Input Name                    | Description                                                   | Type    | Required | Default          |
| ----------------------------- | ------------------------------------------------------------- | ------- | -------- | ---------------- |
| `e2e-test-matrix`             | Make target and region to run e2e tests on, must be JSON formatted | string | No       |                  |
| `e2e-required-status-check`   | Status check to report when e2e tests are complete            | string | No       | `["e2e-tests"]`  |
| `release-branch`              | If this is a release branch, determines the e2e test matrix to use | boolean | No       | `false`          |
| `debug`                       | Debug mode                                                     | boolean | No       | `false`          |

## Jobs

### `set-test-vars`
Builds e2e test matrix from available make targets in the repository.

### `comment`
Updates the comment that triggered the `/test` command to show the run URL.

### `get-buildharness-software-versions`
Gets software versions for various tools used in the workflow.

### `ping`
Conducts a simple ping/pong status update.

### `determine-matrix`
Determines the e2e test matrix based on inputs or defaults.

### `matrix_debug`
Checks the matrix setup in debug mode.

### `e2e-test`
Runs the E2E tests in the determined matrix of regions and make targets.

### `report-e2e-test-status`
Updates the required status check based on the E2E test results.

## Usage

To use this workflow in your repository, reference it in your `.github/workflows` directory with the required secrets and inputs.

---

# PR Merge Group Test Workflow

This reusable workflow is specifically designed for handling pull requests amd merge group tests. It includes jobs for reporting success status, checking for relevant changes, determining branch names, and running E2E tests based on specific conditions.

## Workflow Configuration

This workflow is triggered on a `workflow_call` event.

### Secrets

| Secret Name                   | Description                           | Required |
| ----------------------------- | ------------------------------------- | -------- |
| `TOKEN`                       | GitHub App Token                      | No       |
| `APPLICATION_ID`              | The GitHub App ID                     | No       |
| `APPLICATION_PRIVATE_KEY`     | The GitHub App Private Key            | No       |
| `AWS_COMMERCIAL_ROLE_TO_ASSUME` | AWS IAM Role to assume              | Yes      |
| `AWS_GOVCLOUD_ROLE_TO_ASSUME` | AWS Govcloud IAM Role to assume       | Yes      |

### Inputs

| Input Name                     | Description                                                  | Type    | Required | Default         |
| ------------------------------ | ------------------------------------------------------------ | ------- | -------- | --------------- |
| `common-ci-e2e-test-matrix`    | Make target and region for E2E tests, JSON formatted. Runs in merge queue for main and release-please branches | string  | No       |                 |
| `release-e2e-test-matrix`      | Make target and region for E2E tests, JSON formatted. Runs for release-please branches merging into main | string  | No       |                 |
| `e2e-required-status-check`    | Status check to report when E2E tests are complete           | string  | No       | `["e2e-tests"]` |

## Jobs

### `report-success-status`
Reports success for required status checks on pull_request events.

### `get-changes`
Checks for relevant changes requiring tests in the merge queue.

### `get-branch-name`
Determines the source PR branch name from merge-group's generated branch name.

### `auto-e2e-test-report-success`
Automatically reports success if no relevant changes are found in the merge queue, except for release-please branches targeting main.

### `common-e2e-test`
Runs E2E tests for merge group events with relevant file changes, excluding release-please branches targeting main.

### `release-e2e-test`
Runs E2E tests for merge group events with relevant file changes, specifically for release-please branches targeting main.

## Usage

To use this workflow in your repository, reference it in your `.github/workflows` directory with the required secrets and inputs.

---

# Pre-commit Workflow

This reusable workflow is designed for pre-commit checks in GitHub Actions. It includes jobs to set test variables, fetch software versions, run pre-commit checks, and report the status of these checks.

## Workflow Configuration

This workflow is triggered on a `workflow_call` event.

### Secrets

| Secret Name                   | Description                           | Required |
| ----------------------------- | ------------------------------------- | -------- |
| `TOKEN`                       | GitHub App Token                      | No       |
| `APPLICATION_ID`              | The GitHub App ID                     | No       |
| `APPLICATION_PRIVATE_KEY`     | The GitHub App Private Key            | No       |

### Inputs

| Input Name                          | Description                                       | Type    | Required | Default                |
| ----------------------------------- | ------------------------------------------------- | ------- | -------- | ---------------------- |
| `pre-commit-required-status-check`  | The status check that must pass before            | string  | No       | `["pre-commit-checks"]`|

## Jobs

### `set-test-vars`
Sets the test matrix for pre-commit checks, fetching targets from the makefile.

### `get-buildharness-software-versions`
Fetches software versions for various tools used in the workflow.

### `pre-commit-checks`
Runs the pre-commit checks according to the set matrix.

### `report-pre-commit-test-status`
Reports the status of the pre-commit checks based on if the job matrix finished successfully.

## Usage

To use this workflow in your repository, reference it in your `.github/workflows` directory with the required secrets and inputs.

---

# Release-Please Workflow

This reusable workflow facilitates the automatic release process using `release-please` on every push to the main branch.

## Workflow Configuration

This workflow is triggered on a `workflow_call` event.

### Secrets

| Secret Name                   | Description                           | Required |
| ----------------------------- | ------------------------------------- | -------- |
| `TOKEN`                       | GitHub App Token                      | No       |
| `APPLICATION_ID`              | The GitHub App ID                     | No       |
| `APPLICATION_PRIVATE_KEY`     | The GitHub App Private Key            | No       |

## Jobs

### `release-please`
Runs the `release-please` process for automated release management. This job operates on Ubuntu latest runners and involves steps to acquire the necessary token and execute the `release-please` command.

## Usage

To use this workflow in your repository, reference it in your `.github/workflows` directory with the required secrets. This workflow simplifies the release process by automatically handling versioning and changelog updates.

---

# Renovate Test Workflow

This reusable workflow is designed to autoformat code using pre-commit and then triggers an automatic test run if and only if Renovate is the author and the PR was just opened, as outlined in ADR #0008.

## Workflow Configuration

This workflow is triggered on a `workflow_call` event.

### Secrets

| Secret Name                   | Description                           | Required |
| ----------------------------- | ------------------------------------- | -------- |
| `TOKEN`                       | GitHub App Token                      | No       |
| `APPLICATION_ID`              | The GitHub App ID                     | No       |
| `APPLICATION_PRIVATE_KEY`     | The GitHub App Private Key            | No       |
| `AWS_COMMERCIAL_ROLE_TO_ASSUME` | AWS IAM Role to assume              | Yes      |
| `AWS_GOVCLOUD_ROLE_TO_ASSUME` | AWS GovCloud IAM Role to assume       | Yes      |


### Permissions

- `id-token`: write
- `contents`: write

### Defaults

Runs use bash shell with `-e -o pipefail` for consistency with GitHub Actions' default behavior.

## Jobs

### `renovate-test`
This job checks if the actor is `renovate[bot]` and then performs the following steps:
- Fetches build harness software versions.
- Autoformats the code.
- Automatically approves and merges the pull request.

## Usage

To use this workflow in your repository, reference it in your `.github/workflows` directory with the required secrets and inputs. This workflow is specifically tailored for scenarios involving Renovate bot pull requests.

---

# Repo-Config Workflow

This workflow is designed to configure repository settings and branch protection rules, specifically when the caller reference is the main branch.

## Workflow Configuration

This workflow is triggered on a `workflow_call` event.

### Secrets

| Secret Name                   | Description                           | Required |
| ----------------------------- | ------------------------------------- | -------- |
| `TOKEN`                       | GitHub App Token                      | No       |
| `APPLICATION_ID`              | The GitHub App ID                     | No       |
| `APPLICATION_PRIVATE_KEY`     | The GitHub App Private Key            | No       |

### Inputs

| Input Name  | Description                                   | Type    | Required | Default              |
| ----------- | --------------------------------------------- | ------- | -------- | -------------------- |
| `branch`    | Branch to configure                           | string  | No       | `main`               |
| `checks`    | YAML formatted required status checks         | string  | No       | *see default*        |

### Jobs

#### `repo-config`
Configures repository settings and branch protection rules for the specified branch. This job runs on Ubuntu latest and is conditional on the caller reference being the main branch. It includes the following steps:
- Configuring the repository according to the provided settings.
- Setting up branch protection rules as defined in the inputs.

### Usage

To use this workflow in your repository, reference it in your `.github/workflows` directory with the required secrets and inputs. This workflow is particularly useful for automating the setup of repository configurations and branch protection rules for consistency across projects.

---

# Report-Status Workflow

This reusable workflow is designed to report the status of various checks. It can use either a provided GitHub App Token or a combination of GitHub App ID and Private Key for authentication.

## Workflow Configuration

This workflow is triggered on a `workflow_call` event.

### Secrets

| Secret Name                   | Description                           | Required |
| ----------------------------- | ------------------------------------- | -------- |
| `TOKEN`                       | GitHub App Token                      | No       |
| `APPLICATION_ID`              | The GitHub App ID                     | No       |
| `APPLICATION_PRIVATE_KEY`     | The GitHub App Private Key            | No       |

### Inputs

| Input Name      | Description                               | Type    | Required | Default                                           |
| --------------- | ----------------------------------------- | ------- | -------- | ------------------------------------------------- |
| `status-checks` | List of check types to run                | string  | Yes      |                                                   |
| `status`        | Status to report                          | string  | No       | `success`                                         |
| `description`   | Description to report                     | string  | No       | `"started by @${{ github.actor }}"`               |

### Jobs

#### `report-status-checks`
Runs on Ubuntu latest and involves a strategy to iterate over various status checks provided in the inputs. It includes the following steps:
- Reporting the status for each check in the matrix.
- Utilizing the specified token or app credentials for authentication.

### Usage

To use this workflow in your repository, add it to your `.github/workflows` directory with the necessary secrets and inputs. This workflow is particularly useful for automating the reporting of statuses for various checks in your CI/CD pipeline.

---

# Secure Test with ChatOps Workflow

This reusable workflow conducts secure end-to-end (e2e) tests in a GovCloud environment, reports software versions for various tools, and sends a Slack message if the test fails. It can use either a provided GitHub App Token or a combination of GitHub App ID and Private Key for authentication.

## Workflow Configuration

This workflow is triggered on a `workflow_call` event.

### Secrets

| Secret Name                   | Description                                     | Required |
| ----------------------------- | ----------------------------------------------- | -------- |
| `TOKEN`                       | GitHub App Token                                | No       |
| `APPLICATION_ID`              | The GitHub App ID                               | No       |
| `APPLICATION_PRIVATE_KEY`     | The GitHub App Private Key                      | No       |
| `AWS_GOVCLOUD_ROLE_TO_ASSUME` | AWS Govcloud IAM Role to assume                 | Yes      |
| `SLACK_WEBHOOK_URL`           | Slack webhook URL for posting failure messages  | Yes      |

### Jobs

#### `get-buildharness-software-versions`
- Runs on Ubuntu latest.
- Retrieves software version information, including buildharness, Golang, Terraform, and zarf versions.
- Uses a conditional to skip if the workflow is triggered by a repository dispatch with a 'ping' command.

#### `e2e-govcloud-secure`
- Dependent on the `get-buildharness-software-versions` job.
- Executes secure e2e tests in the GovCloud environment.
- Utilizes versions obtained from the previous job for various tools.
- Runs make targets for secure testing.

#### `send-slack-message`
- Runs if the e2e-govcloud-secure test fails.
- Builds and sends a Slack message to a specified channel, notifying the failure.

### Usage

To implement this workflow in your repository, place it in your `.github/workflows` directory and ensure the necessary secrets are provided. This workflow is particularly useful for organizations that require secure testing in GovCloud environments and wish to integrate ChatOps for real-time notifications of test outcomes.


---

# Update Workflow

This reusable workflow is designed for handling updates triggered by specific commands. It includes parsing commands, updating comments, running ping tests, and autoformatting content.

## Workflow Configuration

This workflow is triggered on a `workflow_call` event.

### Secrets

| Secret Name                   | Description                             | Required |
| ----------------------------- | --------------------------------------- | -------- |
| `TOKEN`                       | GitHub App Token                        | No       |
| `APPLICATION_ID`              | The GitHub App ID                       | No       |
| `APPLICATION_PRIVATE_KEY`     | The GitHub App Private Key              | No       |

### Jobs

#### `parse`
- Runs on Ubuntu latest.
- Parses the command from the workflow trigger.
- Outputs flags for running ping and autoformat based on the parsed command.

#### `comment`
- Updates the comment that triggered the command to show the run URL.

#### `ping`
- Conditional on the output of the `parse` job.
- Executes a simple ping test to validate workflow functionality.

#### `autoformat`
- Conditional on the output of the `parse` job.
- Retrieves build harness software versions.
- Autoformats generated content based on the provided software versions.

### Usage

To use this workflow in your repository, add it to your `.github/workflows` directory with the necessary secrets. This workflow is especially useful for repositories that require automated responses to specific commands, such as updating comments or autoformatting code.
