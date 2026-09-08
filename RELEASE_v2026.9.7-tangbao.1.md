# Hermes Agent v2026.9.7-tangbao.1

Tangbao custom release based on official Hermes Agent `v2026.9.7` / `v0.21.1`.

- **Release-prep date:** 2026-09-08
- **Base version:** official `v2026.9.7` (`2237be355906fbe6065ce1815711eee52b2d646e`)
- **Planned Git tag:** `v2026.9.7-tangbao.1`
- **Release line:** 糖包定制版

## Summary

This is a zero-patch official-equivalent downstream release. The source tree is intentionally equivalent to upstream official `v2026.9.7`. No household functional patches are included.

The current production release, `v2026.7.20-tangbao.1`, also has zero functional delta from its official base: its only downstream commit adds release metadata. There is therefore no household patch to replay or adapt on the `v2026.9.7` line.

## Official release highlights

The runtime and source changes in this release come directly from official Hermes Agent `v2026.9.7`:

- Official version `0.21.1`, published on 2026-09-07.
- Patch rollup of the current official line since `v0.21.0`; the upstream release reports 5,139 non-merge commits, 4,364 changed files, and 632 merged PRs in that release window.
- Broad codebase modularization and file-operation/startup performance work.
- Provider and model updates.
- Desktop session controls and browser annotations.
- MCP authorization improvements.
- Cron scheduling/delivery fixes and delegation reliability improvements.

At release preparation time, official `main` is 28 commits ahead of this tag. `v2026.9.7` remains the latest published stable release and is intentionally used as the downstream baseline.

## Downstream patch assessment

| Patch / customization | Previous status | v2026.9.7 decision | Reason |
| --- | --- | --- | --- |
| Retired downstream rendering patch | Already dropped | **Drop** | It remains retired and is not part of the current production tree. |
| Other downstream functional patches | None | **None to replay** | `v2026.7.20-tangbao.1` differs from official `v2026.7.20` only by this project's release-note metadata. |

Patch queue result: `keep=0`, `adapt=0`, `drop=1` (already retired), with no functional downstream commits to carry forward.

## Validation

- Official tag `v2026.9.7^{}` resolves to `2237be355906fbe6065ce1815711eee52b2d646e`.
- Protected branch `master-v2026.9.7` resolves to the same commit.
- `git diff --name-only v2026.9.7^{}..master-v2026.9.7` returns zero files.
- `pyproject.toml`, `hermes_cli.__version__`, and `hermes --version` report `0.21.1` / `2026.9.7`.
- The preceding Tangbao release audit found one release-note commit and zero functional changes.
- Planned tag `v2026.9.7-tangbao.1` was absent from the household repository when this branch was created.
- `python -m compileall -q hermes_constants.py run_agent.py agent hermes_cli`: **PASS**.
- `pytest tests/test_hermes_constants.py tests/agent/test_image_routing.py tests/cli/test_version_command.py -q -o 'addopts='`: **120 passed, 5 skipped**.
- The release-prep PR is required to contain only this release-note file and must have a clean merge tree against `master-v2026.9.7`.

## Deployment gates and known risks

- This upgrade moves production from official `v0.19.0` (`v2026.7.20`) to `v0.21.1` (`v2026.9.7`). The upstream delta spans 16,134 commits and 10,287 changed files, including extensive modularization. A separate staging/deployment verification is required before changing the production release symlink or restarting the gateway.
- In `v0.21.1`, `agent.image_input_mode: auto` routes images through an explicitly configured `auxiliary.vision` backend even when the main model supports native vision. Profiles that must preserve direct main-model image reading should set `agent.image_input_mode: native` before deployment and verify the effective route.
- This preparation PR does not modify profile configuration, deploy production, change release symlinks, restart gateways, publish tags, or create a GitHub Release.

## Contributors

- Brownie — compatibility audit, baseline preparation, and release verification

## Post-merge tagging

After this PR is reviewed and merged into `master-v2026.9.7`, an authorized release operator may create the annotated tag:

```bash
git fetch origin master-v2026.9.7 --tags
git checkout master-v2026.9.7
git pull --ff-only origin master-v2026.9.7
git tag -a v2026.9.7-tangbao.1 -m "Hermes Agent v2026.9.7-tangbao.1"
git push origin v2026.9.7-tangbao.1
```

GitHub Release publication and production deployment remain separate, explicitly authorized steps.
