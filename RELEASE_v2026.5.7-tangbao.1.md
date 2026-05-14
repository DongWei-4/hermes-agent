# Hermes Agent v2026.5.7-tangbao.1

糖包定制版 based on official Hermes Agent `v2026.5.7` / `v0.13.0`.

## Summary

This release upgrades the household fork from the `v2026.4.23` baseline to the official `v2026.5.7` baseline while keeping only the remaining low-risk Tangbao custom patches.

## Included Tangbao customizations

- Keep Discord Gemini tool schema compatibility hardening.
- Keep assistant tool-call empty-content normalization for provider compatibility.
- Keep Tangbao fork-management documentation.
- Keep PR quality gate workflow and align contributor attribution checks with the household `master` branch.

## Dropped Tangbao customizations

- Drop `network.safe_intranet_ips` because upstream now supports:

  ```yaml
  security:
    allow_private_urls: true
  ```

  This covers household soft-router/LAN access while preserving upstream's always-blocked cloud metadata floor.

- Drop named custom provider credential precedence patch because upstream fixed the core same-`base_url` credential mix-up in `e38ea3807`.
- Drop Feishu LaTeX normalization because the Gemma-specific need is gone.
- Do not carry historical `v2026.4.23-tangbao.*` release metadata into this new baseline branch.

## Required production config

Before promoting this release in a household profile that needs LAN/soft-router access, set:

```yaml
security:
  allow_private_urls: true
```

## Validation

See `UPGRADE_v2026.5.7-tangbao.1.md` for exact validation commands and results.

## Production boundary

Brownie prepared this branch in a development worktree only. Production deployment, tag creation, and release symlink changes require Dirty-bun or explicit user authorization.
