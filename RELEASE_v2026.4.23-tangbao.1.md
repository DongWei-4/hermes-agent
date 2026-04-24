# Hermes Agent v2026.4.23-tangbao.1

**Base Version:** v0.11.0 (Official)
**Release Date:** April 24, 2026

> 本版本是基于官方 **v0.11.0** 的深度定制版（脏脏包系列），在继承官方全新 TUI 和 GPT-5.5 支持的基础上，解决了内网环境下的访问受限问题以及 Gemini 模型的工具调用兼容性问题。

---

## ✨ 二开功能亮点

- **内网访问自由 (Intranet IP Whitelisting)** — 引入 `safe_intranet_ips` 配置项，允许自定义内网 IP 段/CIDR。该功能可绕过 SSRF 保护，使 Agent 能够直接与私有网络中的本地 API 或服务进行交互。
- **Gemini 工具调用修复 (Gemini Tool Schema Fix)** — 解决了 Gemini 模型在处理包含 `integer enum` 类型的工具参数时发生的架构不匹配错误。通过自动类型转换，确保了 Gemini 在 Discord 等工具场景下的正常运作。

---

## 👥 贡献者

- **@DongWei-4** — 脏脏包系列主要开发者

---

**Full Changelog**: [v2026.4.16-tangbao.1...v2026.4.23-tangbao.1](https://github.com/DongWei-4/hermes-agent/compare/v2026.4.16-tangbao.1...v2026.4.23-tangbao.1)
