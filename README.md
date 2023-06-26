# Commit Hash Verifier Action

This GitHub Action verifies the validity of a commit hash from a specific branch.

## Usage

To use this action in your workflow, you can include the following step or reference your custom inputs from there.

```yaml
steps:
  - name: Verify Commit
    uses: shiftEscape/commit-hash-verifier-action@v1
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
        uses: shiftEscape/commit-hash-verifier-action@v1
        with:
          commit-hash: ${{ github.sha }}
          branch-name: ${{ github.ref }}

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
