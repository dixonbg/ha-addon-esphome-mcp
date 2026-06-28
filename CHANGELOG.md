# Changelog

All notable changes to this project will be documented in this file.

## Attributors

- **Bert Berrevoets** — Project author
- **Claude Code** — AI-assisted development

## [1.0.1] - 2026-06-28

### Fixed

Author: *Claude Code*

- `esphome_logs` no longer hangs and fails with `EOFError` when the HA host
  has multiple log sources available (e.g. Zigbee dongles alongside OTA).
  The tool now passes `--device <host>` to the ESPHome CLI, defaulting to
  OTA at `<esphome.name>.local`, and accepts an optional `host` argument to
  target a specific serial adapter instead.

## [1.0.0] - 2026-03-17

### Added

Author: *Bert Berrevoets, Claude Code*

- Initial release as Home Assistant add-on
- FastMCP server with streamable HTTP transport on port 8099
- Bearer token authentication (auto-generated or user-configured)
- Nine MCP tools: list_devices, validate, compile, flash, logs,
  push_files, pull_files, push_fonts, pull_fonts
- Direct filesystem access to `/config/esphome/` — no SSH required
- Alpine-based Docker image with ESPHome and PlatformIO pre-installed
- Multi-architecture support (aarch64, amd64)
- Add-on documentation (DOCS.md)
- secrets.yaml protection in push/pull operations
