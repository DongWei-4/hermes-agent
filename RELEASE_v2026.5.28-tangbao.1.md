# Hermes Agent v2026.5.28-tangbao.1

Tangbao custom release based on official Hermes Agent `v2026.5.28` / `v0.15.0`.

- **Release date:** 2026-05-29
- **Base version:** official `v2026.5.28` (`0c859a1c0`)
- **Planned Git tag:** `v2026.5.28-tangbao.1`
- **Release line:** 糖包定制版

## Summary

This release rebases Tangbao onto the official 2026.5.28 upstream release and reapplies the Feishu/Lark reply Markdown rendering fix that is not present in official `v2026.5.28`.

## Changes since official `v2026.5.28`

| Commit | Area | Description |
| --- | --- | --- |
| `f0baa2ba1` | Feishu gateway | Render Markdown replies/thread replies with native Feishu post elements instead of raw `tag: md` blocks. |

## Feishu rendering fix

Official `v2026.5.28` still sends Markdown content through Feishu post `md` elements. In reply or thread-reply surfaces, Feishu/Lark can render those `md` elements literally, causing headings, bold text, inline code, separators, and fenced code blocks to appear as raw Markdown.

Tangbao `v2026.5.28-tangbao.1` keeps normal non-reply Markdown sends on the existing path, but switches reply/thread reply sends to native Feishu post elements:

- headings / bold text -> `text` elements with `bold` style
- inline code -> `text` elements with `code` style
- fenced code blocks -> `code_block` elements
- horizontal rules -> `hr` elements
- links -> `a` elements

## Validation

Local validation on the upgrade branch:

```text
PYTHONPATH=$PWD /home/openclaw/hermes-prod/current/venv/bin/python -m pytest \
  tests/gateway/test_feishu.py::TestAdapterBehavior::test_send_reply_uses_native_post_elements_for_markdown \
  tests/gateway/test_feishu.py::TestAdapterBehavior::test_send_uses_post_for_inline_markdown \
  tests/gateway/test_feishu.py::TestAdapterBehavior::test_send_splits_fenced_code_blocks_into_separate_post_rows \
  tests/gateway/test_feishu.py::TestAdapterBehavior::test_send_falls_back_to_text_when_post_payload_is_rejected \
  -q -o 'addopts='
# 4 passed, 2 warnings

PYTHONPATH=$PWD /home/openclaw/hermes-prod/current/venv/bin/python -m py_compile \
  gateway/platforms/feishu.py tests/gateway/test_feishu.py
# passed
```

Full Feishu gateway suite was also run locally for this release branch.

## Contributors

- Brownie — Tangbao patch replay and validation

## Post-merge tagging

After the release branch is merged into `master-v2026.5.28`, create the annotated tag from the merged release commit:

```bash
git fetch origin master-v2026.5.28 --tags
git checkout master-v2026.5.28
git pull --ff-only origin master-v2026.5.28
git tag -a v2026.5.28-tangbao.1 -m "Hermes Agent v2026.5.28-tangbao.1"
git push origin v2026.5.28-tangbao.1
```
