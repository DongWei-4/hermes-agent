# Hermes Agent v2026.5.16-tangbao.2

**Release type:** 糖包定制版  
**Base version:** v0.14.0 (official 2026.5.16 line)  
**Release date:** 2026-05-27  
**Planned Git tag:** `v2026.5.16-tangbao.2`

> Versioning note: the leading date follows the upstream official base version date. This is the second Tangbao custom release based on the official `2026.5.16` line.

## Summary

Minimal hotfix release cherry-picking 3 upstream Codex authentication fixes from `upstream/main` (post v2026.5.16). This resolves a Codex auth regression that appeared in the last 24 hours. No other functional changes are included.

## Hotfix: Codex Authentication (upstream backport)

Cherry-picked and squash-merged as PR [#23](https://github.com/DongWei-4/hermes-agent/pull/23):

| Upstream commit | Description |
|-----------------|-------------|
| `69dfcdcc1` | fix(auth): codex chat path falls back to credential_pool when singleton is empty |
| `f1422ffd7` | fix(gateway): classify Codex 429 quota as rate-limit, not missing credentials |
| `2bbd53493` | fix(cli): sync credential_pool on Codex re-auth |

**Files changed:**
- `hermes_cli/auth.py` (+162 / -2)
- `gateway/run.py` (+155 / -6)
- `tests/hermes_cli/test_auth_codex_provider.py` (+110)

## Tangbao Changes since v2026.5.16-tangbao.1

- Cherry-picked 3 upstream Codex auth fixes (#23).
- No household 二开 patches are included in this release.

## Validation

- 23/23 Codex auth tests pass.
- Cherry-pick applied cleanly (no conflicts).
- Planned tag `v2026.5.16-tangbao.2` is absent locally and on `origin`.

## Contributors

- Brownie
- Tangbao

## Post-merge tag command

After this release note is merged, create the annotated tag from the updated Tangbao version integration branch:

```bash
git fetch origin master-v2026.5.16 --tags
git checkout master-v2026.5.16
git pull --ff-only origin master-v2026.5.16
git tag -a v2026.5.16-tangbao.2 -m "Hermes Agent v2026.5.16-tangbao.2"
git push origin v2026.5.16-tangbao.2
```
