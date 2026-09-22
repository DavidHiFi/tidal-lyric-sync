# Changelog

## Unreleased

### Added
- TIDAL source via TidaLuna / `TidalStore` (default source).
- Explicit `source` default of `tidal` so a missing setting resolves correctly.
- `resolveSource()` so unknown or legacy values fall back to TIDAL.
- Start-path logging: `Started (source=…, active=…)` and `start failed:` on error.

### Fixed
- Custom status format corruption when the stored value was not plain text (self-heals to `🎵 {lyrics}`).
- Status writes failing silently: retried once, then logged.
- Rate-limit friendly status updates (1500ms) with in-flight guard.
- Interval not cleared across start/stop cycles.

## Prior history

Inherited from TestCord `src/testcordplugins/lyricsStatus` (GPL-3.0-or-later): Spotify lyric status, custom message on stop, restore status on stop, user-area panel button, shared lyric delay with music controls.
