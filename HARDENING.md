<!-- markdownlint-disable -->

# Hardening Report: bcomnes--deploy-to-neocities/v3.0.4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **bcomnes--deploy-to-neocities/v3.0.4** was hardened automatically. 6 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All uses: references in neocities.yml use mutable tag/branch refs instead of pinned 40-character SHA commits, making the workflow vulnerable to supply-chain attacks. Failing references: `actions/checkout@v4`, `actions/setup-node@v4`, `bcomnes/deploy-to-neocities@master` (branch ref — especially dangerous).

Locations:

- `.github/workflows/neocities.yml:19`
- `.github/workflows/neocities.yml:22`
- `.github/workflows/neocities.yml:29`

### unpinned-uses (severity: high)

All uses: references in release.yml use mutable tag/version refs instead of pinned 40-character SHA commits. Failing references: `actions/checkout@v4`, `actions/setup-node@v4`, `bcomnes/npm-bump@v2.2.1`.

Locations:

- `.github/workflows/release.yml:20`
- `.github/workflows/release.yml:23`
- `.github/workflows/release.yml:30`

### unpinned-uses (severity: high)

All uses: references in test.yml use mutable tag/version refs instead of pinned 40-character SHA commits. Failing references: `actions/checkout@v4`, `actions/setup-node@v4`, `fastify/github-action-merge-dependabot@v3`.

Locations:

- `.github/workflows/test.yml:14`
- `.github/workflows/test.yml:16`
- `.github/workflows/test.yml:30`

### missing-permissions (severity: medium)

neocities.yml has no top-level `permissions:` key and the single `deploy` job also has no `permissions:` key. Without explicit permissions, the workflow inherits the default (potentially broad) repository permissions.

Locations:

- `.github/workflows/neocities.yml:1`

### missing-permissions (severity: medium)

release.yml has no top-level `permissions:` key and the single `version_and_release` job also has no `permissions:` key. Without explicit permissions, the workflow inherits the default (potentially broad) repository permissions.

Locations:

- `.github/workflows/release.yml:1`

### missing-permissions (severity: medium)

test.yml has no top-level `permissions:` key, and the `test` job has no job-level `permissions:` key (only the `automerge` job defines permissions). The `test` job therefore runs with default repository permissions.

Locations:

- `.github/workflows/test.yml:8`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 6 findings across 3 workflow files:

**neocities.yml**: Pinned actions/checkout@v4 → SHA 34e114876b0b11c390a56381ad16ebd13914f8d5, actions/setup-node@v4 → SHA 49933ea5288caeca8642d1e84afbd3f7d6820020, bcomnes/deploy-to-neocities@master → SHA 7d963e5cb1f9e98e6106799a2d631d63b80dd870. Added top-level `permissions: {}` and job-level `permissions: contents: read`.

**release.yml**: Pinned actions/checkout@v4 → SHA 34e114876b0b11c390a56381ad16ebd13914f8d5, actions/setup-node@v4 → SHA 49933ea5288caeca8642d1e84afbd3f7d6820020, bcomnes/npm-bump@v2.2.1 → SHA b57939f35cd2f4c813f8c79239961755023df57a. Added top-level `permissions: {}` and job-level `permissions: contents: write` (required for git operations during version bump/release).

**test.yml**: Pinned actions/checkout@v4 → SHA 34e114876b0b11c390a56381ad16ebd13914f8d5, actions/setup-node@v4 → SHA 49933ea5288caeca8642d1e84afbd3f7d6820020, fastify/github-action-merge-dependabot@v3 → SHA 73ec4cbb5e56df5591eae286972d5b2201ffe90f. Added top-level `permissions: {}` and job-level `permissions: contents: read` for the test job (automerge job already had explicit permissions).

