<div align="center">

# Issue Closed Labeler (GitHub Action)

Conditionally add or remove labels of issues when closed via a PR.

[![CI](https://github.com/RebeccaStevens/issue-closed-labeler-action/actions/workflows/ci.yml/badge.svg)](https://github.com/RebeccaStevens/issue-closed-labeler-action/actions/workflows/ci.yml)
[![Coverage Status](https://codecov.io/gh/RebeccaStevens/issue-closed-labeler-action/branch/main/graph/badge.svg?token=MVpR1oAbIT)](https://codecov.io/gh/RebeccaStevens/issue-closed-labeler-action)
[![code style: prettier](https://img.shields.io/badge/code_style-prettier-ff69b4.svg?style=flat-square)](https://github.com/prettier/prettier)
[![GitHub Discussions](https://img.shields.io/github/discussions/RebeccaStevens/issue-closed-labeler-action?style=flat-square)](https://github.com/RebeccaStevens/issue-closed-labeler-action/discussions)
[![BSD 3 Clause license](https://img.shields.io/github/license/RebeccaStevens/issue-closed-labeler-action.svg?style=flat-square)](https://opensource.org/licenses/BSD-3-Clause)
[![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg?style=flat-square)](https://commitizen.github.io/cz-cli/)
[![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg?style=flat-square)](https://github.com/semantic-release/semantic-release)

</div>

## Inputs

### `rules`

A JSON encoded array of conditions that need to be met and what should happen when they are.

Examples of unencoded rules.

```js,
[
  {
    // Only apply this rule if the issue already has the "bug" label.
    condition: "bug",
    add: "fixed",     // Add the "fixed" label.
  },
  {
    condition: "bug",
    add: [
      "fixed",        // Add the "fixed" label.
      "merged",       // Also add the "merged" label.
    ],
    remove: "wip"     // Remove the "wip" label if present.
  },
  {
    // Only apply this rule if the issue already has either the "feature" or "enhancement" label.
    condition: [
      "some",         // Other options include "all" and "none".
      ["feature", "enhancement"]
    ],
    add: "added",     // Add the "added" label.
  },
  {
    // Same as the rule above but the labels must be on the PR itself.
    condition: [
      "pull request", // The other option is "issue" which is the default.
      "some",
      ["feature", "enhancement"]
    ],
    add: "added",     // Add the "added" label.
  },
  {
    // More complex conditions can be built up using operators.
    // Only apply this rule if the issue already has the "bug" label and does not have the "do not merge" label.
    condition: [
      [
        "all",
        ["bug"]
      ],
      "and",          // Other options include "or" and brackets "(", ")". You can also use "&&" and "||" if you prefer.
      [
        "none",
        ["do not merge"]
      ],
    ],
    add: "fix",
  }
]
```

### `token`

A github token used for creating an octoclient for making API calls. **Default: `${{ github.token }}`**.

## Debug Logging

This action supports [debug logging](https://docs.github.com/en/actions/managing-workflow-runs/enabling-debug-logging#enabling-step-debug-logging). When enabled, it will dump the output of the
api call for creating the tags/branches.
This is useful for testing and should be included when reporting bugs.

# Sample Usage

`versioning.yml`

```yaml
name: PR close - update issues

on:
  pull_request:
    types:
      - closed

jobs:
  run:
    name: "Test"
    runs-on: ubuntu-latest
    steps:
      - name: Update Labels
        uses: RebeccaStevens/issue-closed-labeler-action@latest
        with:
          rules: '[{"condition": "Type: Bug", "add": "Resolution: Fixed"}]'
```
