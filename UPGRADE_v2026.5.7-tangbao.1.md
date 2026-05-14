# Upgrade Report: v2026.5.7-tangbao.1

## Scope

- Official baseline: `v2026.5.7` / Hermes Agent `v0.13.0` (The Tenacity Release)
- Target custom release: `v2026.5.7-tangbao.1`
- Worktree: `/home/openclaw/workspaces/brownie/upgrade-v2026.5.7-tangbao.1`
- Branch: `upgrade/v2026.5.7-tangbao.1`
- Production boundary: this branch is development-only. Do not deploy, tag, or change `/home/openclaw/hermes-prod/current` without explicit production authorization.

## Official baseline highlights

`v2026.5.7` includes major upstream changes since the current Tangbao baseline `v2026.4.23`, including durable multi-agent Kanban, `/goal`, Checkpoints v2, gateway auto-resume, `no_agent` cron watchdog, post-write delta lint, provider plugin/profile changes, Google Chat, i18n locales, SearXNG, MCP SSE, and security fixes.

## Patch queue decisions

| Tangbao patch / feature | Decision | Rationale |
|---|---:|---|
| Discord Gemini schema compatibility | keep | Still useful as a low-risk tool-schema compatibility hardening patch. Cherry-picked cleanly. |
| Empty assistant tool-call content normalization | keep | Provider compatibility fix for APIs that reject assistant tool-call messages with empty string content. Cherry-picked cleanly. |
| Tangbao fork-management policy docs | keep | Repository governance documentation for household fork/release discipline. |
| `network.safe_intranet_ips` | drop | Upstream now supports `security.allow_private_urls: true` / `HERMES_ALLOW_PRIVATE_URLS=true`, allowing soft-router/LAN/private URL access while keeping cloud metadata endpoints blocked. |
| Named custom provider credential preservation | drop | Upstream commit `e38ea3807 fix(credential_pool): resolve key mix-up when custom providers share base_url` adds provider-name-aware credential-pool lookup, covering the core bug. |
| Feishu LaTeX opt-in normalization | drop | No longer required because the Gemma-specific output issue is no longer in use. Avoids carrying Feishu gateway customization. |
| PR quality gate workflow | keep | User requested keeping it in this upgrade branch. Adds a lightweight PR quality workflow and aligns contributor-check with `master`. |
| Old `v2026.4.23-tangbao.*` release notes | preserve as history | Historical files remain in the repository, but are not replayed as functional patches or used as new-release metadata. |

## Required configuration migration

For household deployments that must access soft-router or private/LAN URLs, configure the target profile with the upstream setting:

```yaml
security:
  allow_private_urls: true
```

Equivalent environment override:

```bash
HERMES_ALLOW_PRIVATE_URLS=true
```

Security note: upstream still blocks cloud metadata / credential endpoints even when private URLs are enabled, including `169.254.169.254`, `169.254.170.2`, `169.254.169.253`, `fd00:ec2::254`, `metadata.google.internal`, and `metadata.goog`.

## Retained commits

- `90a905913` — `fix(tools): fix discord tool schema type mismatch for Gemini (#1)`
- `6a5f85615` — `fix: normalize empty tool-call content for affected chat APIs (#7)`
- `3795591db` — `docs(tangbao): define custom fork management policy (#14)`

## Files changed relative to official baseline

- `tools/discord_tool.py`
- `tests/tools/test_discord_tool.py`
- `run_agent.py`
- `tests/run_agent/test_empty_tool_call_content_normalization.py`
- `.github/workflows/contributor-check.yml`
- `.github/workflows/pr-quality-gate.yml`
- `scripts/release.py`
- `docs/tangbao/fork-management.md`
- `UPGRADE_v2026.5.7-tangbao.1.md`
- `RELEASE_v2026.5.7-tangbao.1.md`

## Validation results

Editable import check:

```text
/home/openclaw/workspaces/brownie/upgrade-v2026.5.7-tangbao.1/.venv/bin/python
/home/openclaw/workspaces/brownie/upgrade-v2026.5.7-tangbao.1/run_agent.py
```

Targeted tests run:

```bash
.venv/bin/python -m pytest \
  tests/tools/test_discord_tool.py \
  tests/run_agent/test_empty_tool_call_content_normalization.py \
  tests/tools/test_url_safety.py \
  tests/hermes_cli/test_runtime_provider_resolution.py \
  tests/gateway/test_feishu.py \
  -q
```

Result:

```text
500 passed in 18.97s
```

Static / syntax checks:

```bash
git diff --check origin/master..HEAD -- \
  .github/workflows/contributor-check.yml \
  .github/workflows/pr-quality-gate.yml \
  RELEASE_v2026.5.7-tangbao.1.md \
  UPGRADE_v2026.5.7-tangbao.1.md \
  run_agent.py \
  scripts/release.py \
  tests/run_agent/test_empty_tool_call_content_normalization.py \
  tests/tools/test_discord_tool.py \
  tools/discord_tool.py
.venv/bin/python -m py_compile scripts/release.py
.venv/bin/python -m compileall -q run_agent.py tools/discord_tool.py scripts/release.py
```

Result: passed with no whitespace or Python syntax errors in Tangbao-retained/custom files. Full upstream release diff contains existing upstream whitespace/conflict-marker warnings outside the retained custom patch set, so it is not used as the gate for this upgrade report.

Rationale:

- Discord test covers retained schema patch.
- Empty tool-call content test covers retained provider compatibility patch.
- URL safety test verifies upstream `security.allow_private_urls` behavior replacing `safe_intranet_ips`.
- Runtime provider test verifies upstream named custom provider fix remains intact.
- Feishu test verifies dropping the LaTeX customization does not break the default gateway behavior.

## Known risks

- `security.allow_private_urls: true` is broader than the old `network.safe_intranet_ips` allowlist. This is acceptable for the household soft-router deployment but should not be enabled for untrusted/internet-facing profiles without review.
- Official `v2026.5.7` contains large gateway/provider/tooling changes compared with `v2026.4.23`; production promotion should be handled by Dirty-bun or explicit user authorization after dev verification.
- If this branch is pushed to a fork, rewrite or validate commit author metadata according to the household contributor policy before opening a PR.
