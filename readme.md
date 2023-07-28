# Defense Unicorns Delivery Consolidated GitHub Actions Repository

This repository centralizes Delivery's commonly used GitHub Actions, allowing for easier maintenance and consistency across our organization.

## Overview

GitHub Actions enable automation of software workflows. They can help to streamline software development workflows by providing CI/CD capabilities directly within your repository. However, having actions scattered across multiple repositories can lead to inconsistent results, redundant work, and difficulties in maintenance.

Our Consolidated GitHub Actions repository addresses these issues by housing commonly used actions in a single location. By doing so, updates or changes made to an action only need to be made once, and those changes will then be reflected in all repositories referencing that action.

## Usage

To use an action from this repository, reference it in the `uses` field of your workflow file.
