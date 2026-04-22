# Hermes Agent v2026.4.16-tangbao.1

**Release Date:** April 22, 2026

> The Intranet Freedom release — support for whitelisting local network IP ranges to bypass SSRF protection, enabling Hermes to interact with internal services.

---

## ✨ Highlights

- **Intranet IP Whitelisting** — Introduced `safe_intranet_ips` configuration to allow bypassing SSRF protection for local network IP ranges/CIDRs. Perfect for users behind transparent proxies or internal service environments. ([personal#3](https://github.com/DongWei-4/hermes-agent/pull/3))

---

## 🐛 Bug Fixes & Improvements

- **Gemini Schema Fix** — Resolved a critical API compatibility issue where Gemini models would fail on `integer enum` tool parameters. The schema has been updated to use strings with automatic integer casting in the handler, unblocking tool-use (such as thread creation) for Gemini users. ([personal#1](https://github.com/DongWei-4/hermes-agent/pull/1))
- **Cloudflare Bypass** — Updated internal network requests to include a custom `hermes-agent/1.0` User-Agent string, preventing 403 Forbidden errors when fetching model lists or using web tools.

---

## 👥 Contributors

- **@DongWei-4** — Lead developer for the tangbao branch.

---

**Full Changelog**: [v2026.4.16...v2026.4.16-tangbao.1](https://github.com/DongWei-4/hermes-agent/compare/v2026.4.16...v2026.4.16-tangbao.1)
