# Hermes Agent v2026.5.16-tangbao.1

**Release type:** 糖包定制版
**Base version:** v0.14.0 (official 2026.5.16 line)
**Release date:** 2026-05-20
**Planned Git tag:** `v2026.5.16-tangbao.1`

> Versioning note: the leading date follows the upstream official base version date. This is the first Tangbao custom release based on the official `2026.5.16` line.

## Summary

This release establishes the Tangbao version integration line for the official `v2026.5.16` baseline.

No Tangbao custom functional patches are included in this release. The source tree is intentionally equivalent to upstream official `v2026.5.16`; this release adds only the Tangbao release note and planned tag metadata after the protected `master-v2026.5.16` baseline branch has been created from the official tag.

## Upstream Baseline

- Official tag: `v2026.5.16`
- Official release: Hermes Agent v0.14.0 / v2026.5.16
- Baseline branch: `master-v2026.5.16`
- Baseline verification: `git diff v2026.5.16..master-v2026.5.16` is expected to be empty before this release note PR.

## Tangbao Changes

- Added this release note for `v2026.5.16-tangbao.1`.
- No runtime, gateway, provider, tool, CI, or documentation behavior changes are included.
- No previous Tangbao 二开 patches are replayed in this release.

## Validation

Release-prep validation to perform before merge:

- Planned tag `v2026.5.16-tangbao.1` is absent locally and on `origin` before tagging.
- `origin/master-v2026.5.16` points to the same commit as official `v2026.5.16`.
- `git diff --name-status origin/master-v2026.5.16..HEAD` contains only `RELEASE_v2026.5.16-tangbao.1.md`.
- Merge-tree check against `origin/master-v2026.5.16` completes without conflicts.

## Contributors

- Brownie
- Tangbao

## Post-merge tag command

After this release note is merged, create the annotated tag from the updated Tangbao version integration branch:

```bash
git fetch origin master-v2026.5.16 --tags
git checkout master-v2026.5.16
git pull --ff-only origin master-v2026.5.16
git tag -a v2026.5.16-tangbao.1 -m "Hermes Agent v2026.5.16-tangbao.1"
git push origin v2026.5.16-tangbao.1
```
