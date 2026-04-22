## What does this PR do?

This PR resolves a type mismatch issue in `discord_tool.py` where the `auto_archive_duration` parameter caused failures when using Gemini models. Gemini models frequently encounter issues with strict integer enums in tool schemas. Switching the schema to `string` (while explicitly casting back to `int` in the handler) ensures robust tool calling across more model providers.

## Related Issue

Fixes # (关联相关 Issue)

## Type of Change

- [x] 🐛 Bug fix (non-breaking change that fixes an issue)
- [ ] ✨ New feature
- [ ] 🔒 Security fix
- [ ] 📝 Documentation update
- [ ] ✅ Tests
- [ ] ♻️ Refactor
- [ ] 🎯 New skill

## Changes Made

- Modified `tools/discord_tool.py`:
    - Updated `_build_schema` to change `auto_archive_duration` type from `integer` to `string`.
    - Updated `enum` values to strings: `["60", "1440", "4320", "10080"]`.
    - Updated `_create_thread` to explicitly cast `auto_archive_duration` to `int` before sending the POST request to the Discord API.
- Modified `tests/tools/test_discord_tool.py`:
    - Updated `test_schema_parameter_bounds` to match the new string-based enum schema.

## How to Test

1. 配置 Discord Tool 并提供有效的 Bot Token。
2. 使用 Gemini 系列模型（如 Gemini 1.5 Pro via OpenRouter）。
3. 让 Agent 创建一个线索（Thread）或基于消息的线索。
4. 验证 `auto_archive_duration` 能够被正确解析，且 Discord API 成功返回结果，不再出现模型侧的 Schema 验证错误。

## Checklist

### Code

- [x] I've read the [Contributing Guide](https://github.com/NousResearch/hermes-agent/blob/main/CONTRIBUTING.md)
- [x] My commit messages follow [Conventional Commits](https://www.conventionalcommits.org/)
- [x] I searched for [existing PRs](https://github.com/NousResearch/hermes-agent/pulls) to make sure this isn't a duplicate
- [x] My PR contains **only** changes related to this fix
- [x] I've run `pytest tests/ -q` and all tests pass
- [x] I've added tests for my changes
- [x] I've tested on my platform: Windows 11

### Documentation & Housekeeping

- [ ] I've updated relevant documentation — N/A
- [x] I've updated tool descriptions/schemas if I changed tool behavior
