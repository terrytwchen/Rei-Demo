# Changelog

All notable changes to Rei are documented here. Format based on
[Keep a Changelog](https://keepachangelog.com/); versioning follows
[Semantic Versioning](https://semver.org/).

## [0.1.1] - 2026-06-28

Compliance, security, and reliability hardening on top of the v0.1.0 MVP.

### Added
- **Legal pages & consent** — Privacy Policy and Terms, reachable from a footer on
  every page; clear in-app consent before connecting a Yahoo account; a privacy
  note on the LLM-key screen.
- **Cold-start readiness gate** — on the free tier the app now waits for the
  backends to wake and shows a "starting up" screen instead of an error, and holds
  redirect actions (e.g. Connect Yahoo) until the backend is ready.

### Changed
- **Bilingual replies** — the assistant answers in the language you ask in
  (English or Traditional Chinese).
- **Yahoo data retention** — cached Yahoo league data is purged within 24 hours, in
  line with Yahoo's API terms.

### Security
- **Hardened service-to-service access controls** — strengthened authentication
  between the API gateway and the backend services and tightened how inbound
  request headers are handled.

## [0.1.0] - 2026-06-21

First production release — **Advisory Mode MVP**, deployed to Render (Singapore)
behind the `ttwdev.com` domain (`rei.ttwdev.com` frontend, `rei-api.ttwdev.com` gateway).

### Added
- **Auth** — Yahoo OAuth 2.0 login + guest sessions; Rei-signed JWT via Token
  Exchange; HttpOnly cookies with silent refresh (access + refresh token rotation).
- **Roster & leagues** — sync all Yahoo leagues; per-league roster with
  batting/pitching stat columns (Basic / Advanced / Statcast percentile),
  Season / Last-30d / Date-range, Total / Per-game.
- **AI Agent** — natural-language Q&A (Plan-and-Execute) with BYOK LLM
  (Gemini / Claude / OpenAI via LiteLLM); multi-session chat, SSE streaming,
  inline result tables; topical guardrails.
- **Baseball data** — MLB Stats API + Baseball Savant (xERA / xwOBA / percentiles,
  date-range stats); MongoDB read-through cache.
- **Infrastructure** — 4 Docker services + static PWA on Render; Prometheus
  metrics; GitHub Actions CD (tag-driven) + scheduled cron (Yahoo sync, stats
  refresh, guest cleanup); Cloudflare DNS + Let's Encrypt TLS.
- **Versioning** — frontend shows the release version (Settings); backend
  `/health` (and api-gateway `/version`) report the deployed commit.

### Known limitations
- Free-tier hosting: services sleep after ~15 min idle; the first request after
  waking incurs a ~1 min cold start. Mitigated by a roster "Retry" hint and the
  6-hourly sync cron; full warmth requires a paid instance.

[0.1.1]: https://github.com/terrytwchen/Rei-Demo/releases/tag/v0.1.1
[0.1.0]: https://github.com/terrytwchen/Rei-Demo/releases/tag/v0.1.0
