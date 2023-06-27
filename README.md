# Commit Hash Verifier Action

[![Build Status](https://img.shields.io/endpoint.svg?url=https%3A%2F%2Factions-badge.atrox.dev%2FshiftEscape%2Fcommit-hash-verifier-action%2Fbadge%3Fref%3Dmain&style=flat)](https://actions-badge.atrox.dev/shiftEscape/commit-hash-verifier-action/goto?ref=main)
[![License](https://img.shields.io/github/license/shiftEscape/commit-hash-verifier-action)](https://github.com/shiftEscape/commit-hash-verifier-action/blob/main/LICENSE)
[![Issues](https://img.shields.io/github/issues/shiftEscape/commit-hash-verifier-action)](https://github.com/shiftEscape/commit-hash-verifier-action/issues)
[![Stars](https://img.shields.io/github/stars/shiftEscape/commit-hash-verifier-action)](https://github.com/shiftEscape/commit-hash-verifier-action/stargazers)

Verifies the validity of a commit hash from a specific branch!

## Usage

To use this action in your workflow, you can include the following step or reference your custom inputs from there.

```yaml
steps:
  - name: Verify Commit
    uses: shiftEscape/commit-hash-verifier-action@<version>
    with:
      commit-hash: ${{ github.sha }} # defaults to HEAD
      branch-name: ${{ github.ref }} # defaults to current branch (triggered by event)
```

## Inputs

- `commit-hash` (required): The commit hash to verify.
- `branch-name` (required): The name of the branch to check the commit against.

## Outputs

- `valid-commit`: Indicates if the commit hash is valid.

## Example

Here's an example of a workflow that uses the `Commit Hash Verifier` Action:

```yaml
name: Commit Validation Workflow

on:
  push:
    branches:
      - main

jobs:
  verify-commit-job:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Verify Commit Hash
        uses: shiftEscape/commit-hash-verifier-action@main # specify version using `@`
        with:
          commit-hash: ${{ github.sha }} # reference your commit hash here
          branch-name: ${{ github.ref }} # reference you branch name here

      # Exit on invalid commit hash
      - name: Stop job if commit is invalid
        if: steps.verify-commit.outputs.valid-commit == 'false'
        run: exit 1

      # Continue with other steps if commit is valid
      - name: Continue with remaining steps
        if: steps.verify-commit.outputs.valid-commit == 'true'
        run: |
          # Continue with your desired workflow logic here
          echo "Commit hash is valid and from the correct branch"
```
