<!-- markdownlint-disable -->

# Hardening Report: bcomnes--deploy-to-neocities/v3.0.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **bcomnes--deploy-to-neocities/v3.0.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tags or branch names instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks where a tag or branch could be silently updated to point to malicious code.

neocities.yml:
  - uses: actions/checkout@v4  (tag)
  - uses: actions/setup-node@v4  (tag)
  - uses: bcomnes/deploy-to-neocities@master  (branch — especially dangerous)

release.yml:
  - uses: actions/checkout@v4  (tag)
  - uses: actions/setup-node@v4  (tag)
  - uses: bcomnes/npm-bump@v2.2.1  (tag)

test.yml:
  - uses: actions/checkout@v4  (tag)
  - uses: actions/setup-node@v4  (tag)
  - uses: fastify/github-action-merge-dependabot@v3  (tag)

All references should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/neocities.yml:18`
- `.github/workflows/neocities.yml:21`
- `.github/workflows/neocities.yml:28`
- `.github/workflows/release.yml:18`
- `.github/workflows/release.yml:22`
- `.github/workflows/release.yml:27`
- `.github/workflows/test.yml:14`
- `.github/workflows/test.yml:16`
- `.github/workflows/test.yml:27`

### missing-permissions (severity: medium)

Three workflow files lack a top-level `permissions:` block, and not every job within them defines its own `permissions:` block. Without explicit permissions, workflows run with the default token permissions (which may be read/write depending on repository settings), violating the principle of least privilege.

- neocities.yml: No top-level permissions, no job-level permissions on the `deploy` job.
- release.yml: No top-level permissions, no job-level permissions on the `version_and_release` job.
- test.yml: No top-level permissions; the `test` job has no permissions block (only `automerge` does).

Each workflow should declare a top-level `permissions: {}` (or minimal specific scopes) and grant additional permissions only at the job level where needed.

Locations:

- `.github/workflows/neocities.yml:1`
- `.github/workflows/release.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 3 workflow files:

1. **unpinned-uses**: Pinned all 9 action references to full 40-character commit SHAs with tag comments for readability:
   - actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262 # v4 (used in all 3 files)
   - actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020 # v4 (used in all 3 files)
   - bcomnes/deploy-to-neocities@master → @7d963e5cb1f9e98e6106799a2d631d63b80dd870 # master (neocities.yml)
   - bcomnes/npm-bump@v2.2.1 → @b57939f35cd2f4c813f8c79239961755023df57a # v2.2.1 (release.yml)
   - fastify/github-action-merge-dependabot@v3 → @73ec4cbb5e56df5591eae286972d5b2201ffe90f # v3 (test.yml)

2. **missing-permissions**: Added top-level `permissions: {}` to all 3 workflow files. Added job-level `permissions: contents: write` to the version_and_release job in release.yml (needed to push commits/tags). The automerge job in test.yml already had its own permissions block which was preserved.

