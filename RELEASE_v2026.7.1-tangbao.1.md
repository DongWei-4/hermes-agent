# Hermes Agent v2026.7.1-tangbao.1

Tangbao custom release based on official Hermes Agent `v2026.7.1` / `v0.18.0` (The Judgment Release).

- **Release date:** 2026-07-03
- **Base version:** official `v2026.7.1` (`7c1a02955`)
- **Planned Git tag:** `v2026.7.1-tangbao.1`
- **Release line:** 糖包定制版

## Summary

This is a zero-patch official-equivalent release. The source tree is intentionally equivalent to upstream official `v2026.7.1`. No household 二开 patches are included in this release.

The Feishu reply Markdown rendering fix from `v2026.5.28-tangbao.1` is intentionally **dropped** for this release.

## Changes since official `v2026.7.1`

None. This release adopts the official `v2026.7.1` tree as-is.

## Dropped patches from previous Tangbao release

| Patch | Previous release | Status | Reason |
| --- | --- | --- | --- |
| Feishu reply Markdown rendering fix | v2026.5.28-tangbao.1 | **Drop** | Not needed for this upgrade |

## Upgrade from v2026.5.28 to v2026.7.1

Official delta: **4,356 commits**, **3,899 files changed** (+655,765 / −90,184 lines).

Key official releases between the baselines:

- **v2026.6.5** — The Surface Release (desktop app, browser admin panel, remote gateway)
- **v2026.7.1** — The Judgment Release (300+ issues closed, 1,693 files changed)

## Validation

- `git diff --name-only v2026.7.1..origin/master-v2026.7.1` → 0 (pure official baseline)
- Branch `master-v2026.7.1` protected ✓

## Contributors

- Brownie — release prep

## Post-merge tagging

After this PR is merged into `master-v2026.7.1`, create the annotated tag:

```bash
git fetch origin master-v2026.7.1 --tags
git checkout master-v2026.7.1
git pull --ff-only origin master-v2026.7.1
git tag -a v2026.7.1-tangbao.1 -m "Hermes Agent v2026.7.1-tangbao.1"
git push origin v2026.7.1-tangbao.1
```
