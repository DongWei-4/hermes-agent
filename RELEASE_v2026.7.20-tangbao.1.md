# Hermes Agent v2026.7.20-tangbao.1

Tangbao custom release based on official Hermes Agent `v2026.7.20` / `v0.19.0` (The Quicksilver Release).

- **Release-prep date:** 2026-07-29
- **Base version:** official `v2026.7.20` (`3ef6bbd20`)
- **Planned Git tag:** `v2026.7.20-tangbao.1`
- **Release line:** 糖包定制版

## Summary

This is a zero-patch official-equivalent downstream release. The source tree is intentionally equivalent to upstream official `v2026.7.20`. No downstream functional patches are included.

The preceding downstream release was also official-equivalent and carried no functional patch queue, so there is nothing to replay or adapt for this upgrade.

## Official release highlights

The runtime and source changes in this release come directly from official Hermes Agent `v2026.7.20`:

- Approximately 80% faster cold first-turn startup (`~4.3s` to `~0.9s`).
- Live reasoning and per-token response rendering improvements.
- Desktop and TUI performance improvements, including lower streaming-Markdown CPU use and faster large-diff/session rendering.
- Smart approvals enabled by default, with persistent deny rules.
- Bitwarden and 1Password secret-source support.
- Durable delegated-result and message-delivery handling across restarts.
- Per-server, channel, and thread routing to isolated Hermes profiles.
- New provider/model support and broad credential, webhook, browser, media, local-file, and subprocess security hardening.

Official `v2026.7.1 → v2026.7.20` delta: **2,399 commits**, **2,489 files changed** (+300,745 / −36,282 lines).

## Downstream patch assessment

| Patch / customization | Previous status | v2026.7.20 decision | Reason |
| --- | --- | --- | --- |
| Retired downstream rendering patch | Already dropped | **Drop** | It remains retired and is not part of this release. |
| Other downstream functional patches | None | **None to replay** | The preceding downstream release carried no functional code differences from its official base. |

## Validation

- Official tag `v2026.7.20^{}` resolves to `3ef6bbd201263d354fd83ec55b3c306ded2eb72a`.
- Protected branch `master-v2026.7.20` resolves to the same commit.
- `git diff --name-only v2026.7.20^{}..origin/master-v2026.7.20` returns zero files.
- The preceding downstream baseline audit found only release metadata and zero functional changes.
- Planned tag `v2026.7.20-tangbao.1` was unused when this release-prep branch was created.
- The release-prep PR contains only this release-note file.

## Contributors

- Brownie — compatibility audit and release preparation

## Post-merge tagging

After this PR is reviewed and merged into `master-v2026.7.20`, an authorized release operator may create the annotated tag:

```bash
git fetch origin master-v2026.7.20 --tags
git checkout master-v2026.7.20
git pull --ff-only origin master-v2026.7.20
git tag -a v2026.7.20-tangbao.1 -m "Hermes Agent v2026.7.20-tangbao.1"
git push origin v2026.7.20-tangbao.1
```

GitHub Release publication and deployment are outside the scope of this preparation PR.
