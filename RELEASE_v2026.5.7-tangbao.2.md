# Hermes Agent v2026.5.7-tangbao.2

**Release type:** 糖包定制版
**Base version:** v0.13.0 (official 2026.5.7 line)
**Release date:** 2026-05-20
**Planned Git tag:** `v2026.5.7-tangbao.2`

> Versioning note: the leading date follows the upstream official base version date. This is the second Tangbao custom release based on the official `2026.5.7` line. `v2026.5.7-tangbao.1` was already used for the first Tangbao custom release on this base; this release intentionally increments only the Tangbao sequence suffix.

## Summary

This release packages the follow-up Tangbao custom fixes on top of the `v2026.5.7-tangbao.1` line.

## Highlights

- Fixed Feishu/Lark reply rendering so reply messages use native post elements for Markdown-rich content instead of losing formatting.
- Reworked Tangbao version-branch PR tests to be difference-scoped by default, preventing unrelated upstream full-suite baseline failures from blocking focused PRs.
- Preserved full baseline visibility through manual/scheduled workflow runs while keeping ordinary PR checks targeted and actionable.

## Changes Since v2026.5.7-tangbao.1

Relative to the previous Tangbao custom release on the official `2026.5.7` line, this release includes:

- `ci: scope PR tests to changed areas (#20)`
- `fix(feishu): render reply markdown with native post elements (#19)`

## Validation

- PR #20 CI:
  - `Lint (ruff + ty) / ruff + ty diff`: success
  - `Nix / nix (ubuntu-latest)`: success
  - `Nix / nix (macos-latest)`: success
- PR #19 CI after rebasing onto the diff-scoped CI change:
  - `Contributor Attribution Check / check-attribution`: success
  - `Lint (ruff + ty) / ruff + ty diff`: success
  - `Nix / nix (ubuntu-latest)`: success
  - `Nix / nix (macos-latest)`: success
  - `Supply Chain Audit / Scan PR for critical supply chain risks`: success
  - `Tests / diff-scoped tests`: success
  - `Tests / e2e when touched`: success
  - `Tests / full baseline tests`: skipped by design for PR runs
- Release-prep validation:
  - planned tag `v2026.5.7-tangbao.2` absent locally and on `origin`
  - release note diff contains only `RELEASE_v2026.5.7-tangbao.2.md`
  - merge-tree check against `origin/master-v2026.5.7` completed without conflicts

Known validation note: the upstream full source pytest baseline is currently red independently of these Tangbao follow-up changes. Full baseline tracking is retained as a manual/scheduled workflow rather than an ordinary PR blocker until the inherited baseline debt is resolved.

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
git tag -a v2026.5.7-tangbao.2 -m "Hermes Agent v2026.5.7-tangbao.2"
git push origin v2026.5.7-tangbao.2
```
