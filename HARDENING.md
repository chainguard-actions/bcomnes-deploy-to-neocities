<!-- markdownlint-disable -->

# Hardening Report: bcomnes--deploy-to-neocities/v3.0.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **bcomnes--deploy-to-neocities/v3.0.3** was hardened automatically. 6 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in neocities.yml use mutable tag or branch refs instead of pinned 40-character commit SHAs, making the workflow vulnerable to supply-chain attacks if the referenced action is compromised or its tag is moved. Failing references: `actions/checkout@v4`, `actions/setup-node@v4`, `bcomnes/deploy-to-neocities@master`.

Locations:

- `.github/workflows/neocities.yml:19`
- `.github/workflows/neocities.yml:22`
- `.github/workflows/neocities.yml:28`

### unpinned-uses (severity: high)

All `uses:` references in release.yml use mutable tag refs instead of pinned 40-character commit SHAs. Failing references: `actions/checkout@v4`, `actions/setup-node@v4`, `bcomnes/npm-bump@v2.2.1`.

Locations:

- `.github/workflows/release.yml:19`
- `.github/workflows/release.yml:23`
- `.github/workflows/release.yml:29`

### unpinned-uses (severity: high)

All `uses:` references in test.yml use mutable tag refs instead of pinned 40-character commit SHAs. Failing references: `actions/checkout@v4`, `actions/setup-node@v4`, `fastify/github-action-merge-dependabot@v3`.

Locations:

- `.github/workflows/test.yml:13`
- `.github/workflows/test.yml:15`
- `.github/workflows/test.yml:26`

### missing-permissions (severity: medium)

neocities.yml has no top-level `permissions:` key and the single `deploy` job also has no `permissions:` key. Without explicit permissions, the workflow inherits the default (potentially broad) token permissions.

Locations:

- `.github/workflows/neocities.yml:1`

### missing-permissions (severity: medium)

release.yml has no top-level `permissions:` key and the single `version_and_release` job also has no `permissions:` key. Without explicit permissions, the workflow inherits the default (potentially broad) token permissions.

Locations:

- `.github/workflows/release.yml:1`

### missing-permissions (severity: medium)

test.yml has no top-level `permissions:` key and the `test` job has no job-level `permissions:` key (only the `automerge` job defines permissions). The `test` job therefore runs with default, potentially broad token permissions.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 6 findings across 3 workflow files:

1. neocities.yml: Pinned actions/checkout@v4 → SHA 34e114876b0b11c390a56381ad16ebd13914f8d5, actions/setup-node@v4 → SHA 49933ea5288caeca8642d1e84afbd3f7d6820020, bcomnes/deploy-to-neocities@master → SHA 9db23a2f6149b53a63ec046c41d16063b87afc33. Added top-level `permissions: {}`.

2. release.yml: Pinned actions/checkout@v4 → SHA 34e114876b0b11c390a56381ad16ebd13914f8d5, actions/setup-node@v4 → SHA 49933ea5288caeca8642d1e84afbd3f7d6820020, bcomnes/npm-bump@v2.2.1 → SHA b57939f35cd2f4c813f8c79239961755023df57a. Added top-level `permissions: {}` and job-level `permissions: contents: write` (needed for git tag/push operations by npm-bump).

3. test.yml: Pinned actions/checkout@v4 → SHA 34e114876b0b11c390a56381ad16ebd13914f8d5, actions/setup-node@v4 → SHA 49933ea5288caeca8642d1e84afbd3f7d6820020, fastify/github-action-merge-dependabot@v3 → SHA 73ec4cbb5e56df5591eae286972d5b2201ffe90f. Added top-level `permissions: {}` and explicit `permissions: {}` on the test job. The automerge job already had its own permissions block (pull-requests: write, contents: write) which was preserved.

