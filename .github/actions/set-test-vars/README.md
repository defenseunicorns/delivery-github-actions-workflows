# set-test-vars

This action extracts make targets out of the caller repo's makefile in regards to end to end testing and pre-commit hooks.

Syntax for the caller repo's makefile must be in this pattern:

```makefile
# terratest make targets

# will be extracted and added to e2e-test-matrix by this action when merging to main or when it is a release-branch
.PHONY: test-ci-complete-insecure
test-ci-complete-insecure: ## Run one test (TestExamplesCompleteInsecure). Requires access to an AWS account. Costs real money.
    $(eval export TF_VAR_region := $(or $(REGION),$(TF_VAR_region),us-east-2))
    $(MAKE) _test-all EXTRA_TEST_ARGS="-timeout 3h -run TestExamplesCompleteInsecure"

# will be extracted and added to e2e-test-matrix by this action if it is a release-branch
.PHONY: test-release-complete-secure
test-release-complete-secure: ## Run one test (TestExamplesCompleteSecure). Requires access to an AWS account. Costs real money.
    $(eval export TF_VAR_region := $(or $(REGION),$(TF_VAR_region),us-gov-west-1))
    $(MAKE) _test-all EXTRA_TEST_ARGS="-timeout 3h -run TestExamplesCompleteSecure"

# pre commit targets

# will be excluded from extraction by this action
.PHONY: pre-commit-all
pre-commit-all: ## Run all pre-commit hooks. Returns nonzero exit code if any hooks fail. Uses Docker for maximum compatibility
    $(MAKE) _runhooks HOOK="" SKIP=""

# will be extracted and added to pre-commit-test-matrix
.PHONY: pre-commit-terraform
pre-commit-terraform: ## Run the terraform pre-commit hooks. Returns nonzero exit code if any hooks fail. Uses Docker for maximum compatibility
    $(MAKE) _runhooks HOOK="" SKIP="check-added-large-files,check-merge-conflict,detect-aws-credentials,detect-private-key,end-of-file-fixer,fix-byte-order-marker,trailing-whitespace,check-yaml,fix-smartquotes,go-fmt,golangci-lint,renovate-config-validator"

# will be extracted and added to pre-commit-test-matrix
.PHONY: pre-commit-golang
pre-commit-golang: ## Run the golang pre-commit hooks. Returns nonzero exit code if any hooks fail. Uses Docker for maximum compatibility
    $(MAKE) _runhooks HOOK="" SKIP="check-added-large-files,check-merge-conflict,detect-aws-credentials,detect-private-key,end-of-file-fixer,fix-byte-order-marker,trailing-whitespace,check-yaml,fix-smartquotes,terraform_fmt,terraform_docs,terraform_checkov,terraform_tflint,renovate-config-validator"

# will be extracted and added to pre-commit-test-matrix
.PHONY: pre-commit-renovate
pre-commit-renovate: ## Run the renovate pre-commit hooks. Returns nonzero exit code if any hooks fail. Uses Docker for maximum compatibility
    $(MAKE) _runhooks HOOK="renovate-config-validator" SKIP=""

# will be extracted and added to pre-commit-test-matrix
.PHONY: pre-commit-common
pre-commit-common: ## Run the common pre-commit hooks. Returns nonzero exit code if any hooks fail. Uses Docker for maximum compatibility
    $(MAKE) _runhooks HOOK="" SKIP="go-fmt,golangci-lint,terraform_fmt,terraform_docs,terraform_checkov,terraform_tflint,renovate-config-validator"
```

<!-- action-docs-description -->
## Description

This action sets the environment variables needed to run the e2e tests
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
| e2e-test-matrix | The matrix of e2e tests to run |
| pre-commit-test-matrix | The matrix of pre-commit tests to run |
<!-- action-docs-outputs -->

<!-- action-docs-runs -->
## Runs

This action is a `composite` action.
<!-- action-docs-runs -->
