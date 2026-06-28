# Changelog

All notable changes to this project will be documented in this file.

## Attributors

- **Bert Berrevoets** — Project author
- **Claude Code** — AI-assisted development

## [1.1.3] - 2026-06-29

### Fixed

Author: *Claude Code*

- `esphome_flash`/`esphome_logs` default OTA host was the literal
  `${name}.local` for substitution/package-based configs, because the raw
  YAML parse can't resolve `substitutions:`. `_default_ota_host()` now falls
  back to the YAML filename stem (which matches the device name for these
  configs), so the default host works without passing `host=` explicitly.

### Changed

- Raised `esphome_compile` and `esphome_flash` subprocess timeouts from
  300s/600s to 1800s. Heavy builds (e.g. voice assistants with
  esp-tflite-micro wake-word models, LVGL displays) exceed 300s; killing the
  build mid-archive left a corrupted `.esphome/build` dir. (Note: MCP clients
  may impose their own idle timeout — e.g. Claude Code ~300s — but the build
  now completes server-side, so a subsequent cached compile returns quickly.)

## [1.1.2] - 2026-06-29

### Fixed

Author: *Claude Code*

- `esphome_flash` had the same interactive-picker hang as `esphome_logs`
  (fixed in 1.0.1): `esphome run` blocked on "Found multiple options for
  uploading" with no stdin, then died with `EOFError`, whenever serial
  adapters were present alongside the OTA target. Added the same
  `--device <host>` passthrough, defaulting to OTA at
  `<esphome.name>.local`, shared with `esphome_logs` via a new
  `_default_ota_host()` helper.

## [1.1.1] - 2026-06-28

### Fixed

Author: *Claude Code*

- `push_files`/`pull_files` now reject filenames that resolve outside
  `/config/esphome` (realpath containment check), closing a path-traversal
  gap where a `../` filename could read or write elsewhere under `/config`.

## [1.1.0] - 2026-06-28

### Changed

Author: *Claude Code*

- Switched the add-on's base image from Alpine (`home-assistant/*-base-python`)
  to Debian (`ghcr.io/esphome/docker-base:debian-ha-addon-*`) — the same base
  the official ESPHome Device Builder add-on uses. `esphome_compile`/
  `esphome_flash` for esp-idf-framework devices failed under Alpine with
  "Failed to install Python dependencies into penv": pioarduino's esp-idf
  toolchain pip-installs manylinux (glibc) wheels into its own venv, which
  is incompatible with musl libc. Debian doesn't have this problem and
  already ships git/build-essential/curl, so the old `apk add` build-deps
  step was removed as unnecessary.

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
