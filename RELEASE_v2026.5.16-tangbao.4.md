# Hermes Agent v2026.5.16-tangbao.4

**Release type:** 糖包定制版  
**Base version:** v0.14.0 (official 2026.5.16 line)  
**Release date:** 2026-05-28  
**Planned Git tag:** `v2026.5.16-tangbao.4`

> Versioning note: the leading date follows the upstream official base version date. This is the fourth Tangbao custom release based on the official `2026.5.16` line.

## Summary

Bugfix release addressing a Codex stream crash where the Codex private backend returns `response.completed.response.output = null`, causing the OpenAI SDK to raise `TypeError: 'NoneType' object is not iterable` during stream iteration or `get_final_response()`.

## Fix: Codex Stream — null-output TypeError recovery (#27)

**Symptom:** When the Codex backend (`/backend-api/codex`) returned `"output": null` in its SSE `response.completed` event, the OpenAI SDK's `_ResponsesStream.__iter__` → `handle_event()` → `parse_response()` would iterate over `response.output` and raise `TypeError: 'NoneType' object is not iterable`. This could happen either during the `for event in stream` iteration (more common) or at `stream.get_final_response()`. The agent had no recovery path and the error propagated as an unrecoverable failure.

**Root cause:** The backfill logic in `_run_codex_stream()` and `_CodexCompletionsAdapter.create()` only handled `output == []` (empty list), not `output == None`. Additionally, the TypeError catch was scoped too narrowly — only wrapping `get_final_response()`, missing the iteration-phase crash path.

**Fix:**
- `run_agent.py`: Added `_is_codex_null_output_type_error()` static helper. Moved `try/except TypeError` to wrap both `for event in stream` and `get_final_response()`, recovering from collected stream events (`output_item.done` items or `output_text.delta` parts).
- `agent/auxiliary_client.py`: Same recovery logic in `_CodexCompletionsAdapter.create()`.
- Broadened backfill condition from `isinstance(_out, list) and not _out` to `not isinstance(_out, list) or not _out` (covers both `None` and empty list).
- `_run_codex_create_stream_fallback`: Added dict/namespace-aware `_resp_get`/`_resp_set` helpers for reading/writing `output` on both object and dict terminal responses. Same broadened backfill condition.

**Files changed:**
- `run_agent.py` (+87 / -11)
- `agent/auxiliary_client.py` (+71 / -10)

**Testing:** 286 tests pass (54 codex responses + 172 auxiliary client + 47 streaming + 13 auth codex provider), including 5 new tests covering:
- Iteration-phase TypeError recovery (text delta + function_call)
- `get_final_response()`-phase TypeError recovery
- Auxiliary adapter iteration-phase recovery
- Fallback `create(stream=True)` with `output=None` terminal response

## Tangbao Changes since v2026.5.16-tangbao.3

- Fixed Codex stream null-output TypeError crash (#27). This is a household-developed fix (二开补丁) targeting the Codex backend specifically.

## Validation

- 286/286 Codex and related tests pass.
- Functional verification: fake Codex stream with `iter_error=TypeError("'NoneType' object is not iterable")` recovers correctly from collected delta events and function_call items.
- Planned tag `v2026.5.16-tangbao.4` is absent locally and on `origin`.

## Contributors

- Brownie

## Post-merge tag command

After this release note is merged, create the annotated tag from the updated Tangbao version integration branch:

```bash
git fetch origin master-v2026.5.16 --tags
git checkout master-v2026.5.16
git pull --ff-only origin master-v2026.5.16
git tag -a v2026.5.16-tangbao.4 -m "Hermes Agent v2026.5.16-tangbao.4"
git push origin v2026.5.16-tangbao.4
```
