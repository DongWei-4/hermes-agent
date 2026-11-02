# Hermes Agent v0.22.5-tangbao.1

Tangbao custom release based on official Hermes Agent `v0.22.5`.

- **Release-prep date:** 2026-11-02
- **Base version:** official `v0.22.5` (`bf0203a794a07aac3ff764972ab9087baa4e4f4e`)
- **Planned Git tag:** `v0.22.5-tangbao.1`
- **Release line:** 糖包定制版

## Versioning note

Upstream has moved from date-shaped tags to SemVer tags. This release preserves the established Tangbao rule semantically: keep the exact official base tag and append only the downstream sequence suffix. The official base is `v0.22.5`, so the first Tangbao custom release on this base is `v0.22.5-tangbao.1`; the current calendar date is not substituted into the tag.

## Summary

This is a zero-patch official-equivalent downstream release. The source tree is intentionally equivalent to upstream official `v0.22.5`. No household functional patches are included.

The preceding production release, `v2026.9.7-tangbao.1`, also has zero functional delta from its official `v2026.9.7` / `v0.21.1` base: its only downstream commit adds release metadata. There is therefore no household patch to replay or adapt on the `v0.22.5` line.

## Official release highlights

The runtime and source changes in this release come directly from official Hermes Agent `v0.22.5`:

- Stable SemVer release channels and the transaction-style `hermes pm` package manager.
- One multiplexed Gateway and Desktop backend per machine for multiple isolated profiles.
- The reviewed Plugin Catalog, with memory providers and Home Assistant delivered as plugins.
- Managed local-model support through llama.cpp, plus unified Connectors and MCP setup.
- Live subagent transcripts with steer/stop controls and pluggable delegation backends.
- Agent liveness, context compaction, session/state database, provider recovery, and browser-credential reliability improvements.
- `hermes import-agent`, OpenClaw migration, and plugin-delivered `hermes mcp serve` support.
- Gateway reboot recovery and long local-model turn fixes from the `v0.22.x` patch line.

The official release was published on 2026-10-23 UTC. At release preparation time, `v0.22.5` remains the latest non-draft, non-prerelease GitHub release.

## Downstream patch assessment

| Patch / customization | Previous status | v0.22.5 decision | Reason |
| --- | --- | --- | --- |
| Retired downstream rendering patch | Already dropped | **Drop** | It remains retired and is not part of the current production tree. |
| Other downstream functional patches | None | **None to replay** | `v2026.9.7-tangbao.1` differs from official `v2026.9.7` only by release-note metadata. |

Patch queue result: `keep=0`, `adapt=0`, `drop=1` (already retired), with no functional downstream commits to carry forward.

## Validation

- Official annotated tag `v0.22.5^{}` resolves to `bf0203a794a07aac3ff764972ab9087baa4e4f4e`.
- Protected household branch `master-v0.22.5` resolves to the same commit.
- `git diff --name-only v0.22.5^{}..origin/master-v0.22.5` returns zero files.
- Planned tag `v0.22.5-tangbao.1` was absent locally and from the household repository when this branch was created.
- The preceding Tangbao release audit finds one release-note file and zero functional source changes.
- On the ARM64 ROCK 5T development host, official PM installed and hash-verified Python 3.14.9, Node, npm, FFmpeg, ripgrep, the test environment, agent-browser, and cua-driver.
- `hermes --version` reports `Hermes Agent v0.22.5` under Python 3.14.9 from this worktree.
- Targeted version and Feishu validation via `scripts/run_tests.sh`: **160 passed, 7 skipped, 0 failed** across 8 test files, including Feishu approval buttons, setup, WebSocket multiplex isolation, and profile-scope hops.
- The release-prep PR is required to contain only this release-note file and to have a clean merge tree against `master-v0.22.5`.

## Deployment gates and known risks

- This release changes the runtime from official `v0.21.1` to `v0.22.5` and includes large Gateway, profile, package-management, plugin, session, Desktop, and tool-loading changes. Production deployment remains a separate operation.
- The multiplexed one-Gateway-per-machine architecture must be staged against household Feishu routing, home channels, cron delivery, pairing, and profile access controls before deployment.
- Managed installs use Python 3.14; KittenTTS is unavailable on that runtime. The ARM64 installation path passed in the development worktree, but production configuration and service migration still require staging.
- Home Assistant and several memory providers moved from core to plugins. Existing data/config migration and per-profile plugin isolation must be verified before production promotion.
- `v0.22.1` was built from the wrong commit and is intentionally skipped; this release targets the corrected and later `v0.22.5` line directly.
- This repository release does not change the production symlink, restart Gateway services, or alter production profiles.

## Contributors

- Brownie — upstream verification, ARM64 compatibility validation, baseline preparation, and release publication

## Post-merge tagging

After this PR is reviewed and merged into `master-v0.22.5`, create the annotated tag:

```bash
git fetch origin master-v0.22.5 --tags
git checkout master-v0.22.5
git pull --ff-only origin master-v0.22.5
git tag -a v0.22.5-tangbao.1 -m "Hermes Agent v0.22.5-tangbao.1"
git push origin v0.22.5-tangbao.1
```

Publish the GitHub Release from the merged `master-v0.22.5` commit using this release note. Production deployment remains a separate, explicitly authorized lifecycle step.
