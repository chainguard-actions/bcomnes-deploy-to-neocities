<!-- markdownlint-disable -->

# Hardening Report: bcomnes--deploy-to-neocities/v3.0.5

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **bcomnes--deploy-to-neocities/v3.0.5** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references across the workflow files are pinned to mutable tags, version strings, or branch names rather than immutable 40-character commit SHAs. This exposes the workflows to supply-chain attacks where a compromised or force-pushed tag could silently execute malicious code.

.github/workflows/neocities.yml:
  - uses: actions/checkout@v6 (line 22)
  - uses: actions/setup-node@v6 (line 25)
  - uses: bcomnes/deploy-to-neocities@master (line 32) — especially dangerous: 'master' is a mutable branch

.github/workflows/release.yml:
  - uses: actions/checkout@v6 (line 22)
  - uses: actions/setup-node@v6 (line 26)
  - uses: bcomnes/npm-bump@v2.2.1 (line 32)

.github/workflows/test.yml:
  - uses: actions/checkout@v6 (line 17)
  - uses: actions/setup-node@v6 (line 19)
  - uses: fastify/github-action-merge-dependabot@v3 (line 31)

All should be pinned to full SHA digests, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/neocities.yml:22`
- `.github/workflows/neocities.yml:25`
- `.github/workflows/neocities.yml:32`
- `.github/workflows/release.yml:22`
- `.github/workflows/release.yml:26`
- `.github/workflows/release.yml:32`
- `.github/workflows/test.yml:17`
- `.github/workflows/test.yml:19`
- `.github/workflows/test.yml:31`

### missing-permissions (severity: medium)

Three workflow files lack explicit `permissions:` blocks, meaning they run with the default (often broad) GITHUB_TOKEN permissions.

- `.github/workflows/neocities.yml`: No top-level `permissions:` key and the single `deploy` job has no job-level `permissions:` block.
- `.github/workflows/release.yml`: No top-level `permissions:` key and the single `version_and_release` job has no job-level `permissions:` block.
- `.github/workflows/test.yml`: No top-level `permissions:` key; the `automerge` job has a job-level `permissions:` block, but the `test` job does not — so not every job is covered.

Add a top-level `permissions: {}` (or minimal specific scopes) to each workflow, and grant only the permissions actually required by each job.

Locations:

- `.github/workflows/neocities.yml:1`
- `.github/workflows/release.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 3 workflow files:

**unpinned-uses** (9 references pinned):
- actions/checkout@v6 → @d23441a48e516b6c34aea4fa41551a30e30af803 # v6 (in all 3 files)
- actions/setup-node@v6 → @249970729cb0ef3589644e2896645e5dc5ba9c38 # v6 (in all 3 files)
- bcomnes/deploy-to-neocities@master → @7d963e5cb1f9e98e6106799a2d631d63b80dd870 # master (neocities.yml)
- bcomnes/npm-bump@v2.2.1 → @b57939f35cd2f4c813f8c79239961755023df57a # v2.2.1 (release.yml)
- fastify/github-action-merge-dependabot@v3 → @73ec4cbb5e56df5591eae286972d5b2201ffe90f # v3 (test.yml)

**missing-permissions** (3 files updated):
- neocities.yml: Added top-level `permissions: {}` and job-level `contents: read`
- release.yml: Added top-level `permissions: {}` and job-level `contents: write` (needed for git push during version bump)
- test.yml: Added top-level `permissions: {}` and job-level `contents: read` for the test job (automerge job already had its permissions)

