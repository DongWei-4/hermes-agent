# Hermes Agent v2026.5.7-tangbao.1

**Release type:** 糖包定制版
**Base version:** v0.13.0 (official 2026.5.7 line)
**Release date:** 2026-05-14
**Planned Git tag:** `v2026.5.7-tangbao.1`

> Versioning note: the leading date follows the upstream official base version date. This is the first Tangbao custom release based on the official `2026.5.7` line.

## Summary

This release packages the Tangbao custom patch set replayed on top of the official `v2026.5.7` / `v0.13.0` baseline.

## Highlights

- Added Tangbao fork/update management documentation for the versioned integration-branch workflow.
- Fixed Discord tool schema compatibility for Gemini by normalizing schema types expected by affected providers.
- Normalized empty assistant tool-call content for affected chat APIs to prevent provider-side request failures.
- Updated CI workflow branch filters so PR checks run on Tangbao version integration branches such as `master-v2026.5.7` and future `master-v*` lines.

## Changes Since Official Base

Relative to official tag `v2026.5.7`, this Tangbao custom release includes:

- `fix(tools): fix discord tool schema type mismatch for Gemini (#1)`
- `fix: normalize empty tool-call content for affected chat APIs (#7)`
- `docs(tangbao): define custom fork management policy (#14)`
- `ci: run PR checks on Tangbao version branches (#17)`

## Validation

- PR #17 CI after workflow-only skip adjustment:
  - `Contributor Attribution Check / check-attribution`: success
  - `Lint (ruff + ty) / ruff + ty diff`: success
  - `Nix / nix (ubuntu-latest)`: success
  - `Nix / nix (macos-latest)`: success
  - `OSV-Scanner / Scan lockfiles`: success
- Release-prep validation:
  - planned tag `v2026.5.7-tangbao.1` absent locally and on `origin`
  - release note diff contains only `RELEASE_v2026.5.7-tangbao.1.md`
  - merge-tree check against `origin/master-v2026.5.7` completed without conflicts

Known validation note: enabling the full source `Tests` workflow on `master-v*` exposed existing source-suite failures on the target line unrelated to the workflow-only CI branch-filter change. Source-code PRs should continue to run that suite; workflow-only PRs skip it to avoid being blocked by unrelated source-suite debt.

## Contributors

- Brownie
- Dirty-bun
- Tangbao

## Post-merge tag command

After this release note is merged, create the annotated tag from the updated Tangbao version integration branch:

```bash
git fetch origin master-v2026.5.7 --tags
git checkout master-v2026.5.7
git pull --ff-only origin master-v2026.5.7
git tag -a v2026.5.7-tangbao.1 -m "Hermes Agent v2026.5.7-tangbao.1"
git push origin v2026.5.7-tangbao.1
```
