# Hermes Agent v2026.5.16-tangbao.3

**Release type:** 糖包定制版  
**Base version:** v0.14.0 (official 2026.5.16 line)  
**Release date:** 2026-05-28  
**Planned Git tag:** `v2026.5.16-tangbao.3`

> Versioning note: the leading date follows the upstream official base version date. This is the third Tangbao custom release based on the official `2026.5.16` line.

## Summary

Bugfix release addressing a Codex transport crash (`'NoneType' object is not iterable`) that caused the agent to fall back to OpenRouter when using the Codex provider without tools.

## Fix: Codex Transport — tools=None crash (#25)

**Symptom:** When invoking Codex without tools (or with tools that resolve to a falsy value), the transport inserted `"tools": None` into the Responses API kwargs dict. The Codex backend (`/backend-api/codex`) responded with `"output": null` in its SSE `response.completed` event, causing the OpenAI SDK to raise `TypeError: 'NoneType' object is not iterable` on response parsing. The agent then classified this as a non-retryable local error and fell back to OpenRouter.

**Root cause:** `agent/transports/codex.py:build_kwargs()` unconditionally set `kwargs["tools"] = response_tools` before the `if response_tools:` guard, so a falsy `response_tools` value (None, empty list) leaked into the API request.

**Fix:** Move `kwargs["tools"] = response_tools` inside the `if response_tools:` block so it is only set when tools are actually present.

**Files changed:**
- `agent/transports/codex.py` (+1 / -1)

**Testing:** 125/125 tests pass (39 codex transport + 47 codex responses + 39 transport framework).

## Tangbao Changes since v2026.5.16-tangbao.2

- Fixed Codex transport `tools=None` crash (#25).
- This is a household-developed fix (二开补丁) targeting the Codex backend specifically.

## Validation

- 125/125 Codex and transport tests pass.
- Functional verification: `ResponsesApiTransport.build_kwargs(tools=None)` no longer includes `"tools"` key in output kwargs.
- Planned tag `v2026.5.16-tangbao.3` is absent locally and on `origin`.

## Contributors

- Brownie

## Post-merge tag command

After this release note is merged, create the annotated tag from the updated Tangbao version integration branch:

```bash
git fetch origin master-v2026.5.16 --tags
git checkout master-v2026.5.16
git pull --ff-only origin master-v2026.5.16
git tag -a v2026.5.16-tangbao.3 -m "Hermes Agent v2026.5.16-tangbao.3"
git push origin v2026.5.16-tangbao.3
```
