<!-- markdownlint-disable -->

# Hardening Report: bcomnes--deploy-to-neocities/v3.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **bcomnes--deploy-to-neocities/v3.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tags or branch names instead of immutable 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag or branch is moved to point to malicious code.

.github/workflows/neocities.yml: `actions/checkout@v4`, `actions/setup-node@v4`, `bcomnes/deploy-to-neocities@master` (especially dangerous — `@master` is a mutable branch).

.github/workflows/release.yml: `actions/checkout@v4`, `actions/setup-node@v4`, `bcomnes/npm-bump@v2.2.1`.

.github/workflows/test.yml: `actions/checkout@v4`, `actions/setup-node@v4`, `fastify/github-action-merge-dependabot@v3`.

Locations:

- `.github/workflows/neocities.yml:19`
- `.github/workflows/neocities.yml:22`
- `.github/workflows/neocities.yml:28`
- `.github/workflows/release.yml:19`
- `.github/workflows/release.yml:22`
- `.github/workflows/release.yml:29`
- `.github/workflows/test.yml:14`
- `.github/workflows/test.yml:16`
- `.github/workflows/test.yml:27`

### missing-permissions (severity: medium)

Three workflow files lack a top-level `permissions:` block and have at least one job without a job-level `permissions:` block, meaning those jobs run with the default (broad) token permissions.

- `.github/workflows/neocities.yml`: No top-level permissions and no job-level permissions on the `deploy` job.
- `.github/workflows/release.yml`: No top-level permissions and no job-level permissions on the `version_and_release` job.
- `.github/workflows/test.yml`: No top-level permissions; the `test` job has no job-level permissions (only the `automerge` job does).

Locations:

- `.github/workflows/neocities.yml:1`
- `.github/workflows/release.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 3 workflow files:

**unpinned-uses** (9 locations):
- actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262 # v4
- actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020 # v4
- bcomnes/deploy-to-neocities@master → @7d963e5cb1f9e98e6106799a2d631d63b80dd870 # master
- bcomnes/npm-bump@v2.2.1 → @b57939f35cd2f4c813f8c79239961755023df57a # v2.2.1
- fastify/github-action-merge-dependabot@v3 → @73ec4cbb5e56df5591eae286972d5b2201ffe90f # v3

**missing-permissions** (3 workflow files):
- Added `permissions: {}` at top level to all 3 files (deny-all default)
- neocities.yml deploy job: `contents: read`
- release.yml version_and_release job: `contents: write` (needed for git operations/tagging)
- test.yml test job: `contents: read`; automerge job already had explicit permissions

