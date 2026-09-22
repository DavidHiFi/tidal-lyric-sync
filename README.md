# tidal-lyric-sync

Show the **current lyric line** from TIDAL or Spotify as your **Discord custom status**.

This repository is a standalone copy of the **LyricsStatus** plugin from [TestcordDev/Testcord](https://github.com/TestcordDev/Testcord), with TIDAL support and reliability fixes applied. Lyrics are fetched from [LRCLIB](https://lrclib.net) and timed against your playback position.

## Features

- Syncs the active lyric line into your Discord custom status
- Playback sources: **TIDAL** (via TidaLuna / TIDALuna local API) and **Spotify**
- Template support: `{lyrics}`, `{song}`, `{artist}` (default `🎵 {lyrics}`)
- Optional status when playback stops: custom message **or** restore your previous status
- Optional user-area panel toggle button
- Global + per-song lyric delay (shared with TestCord music controls)
- Rate-limited status writes with a single retry so Discord does not drop updates

## Requirements

- A Discord client mod that loads TestCord plugins ([Testcord](https://github.com/TestcordDev/Testcord))
- For TIDAL: [TidaLuna](https://github.com/Inrixia/TidaLuna) installed in the TIDAL desktop app (local WebSocket `ws://localhost:24123`, configurable in music controls settings)
- For Spotify: Spotify desktop player with the usual client-mod Spotify integration
- Network access to `https://lrclib.net` for synced lyrics

## Installation

Copy the plugin folder into your TestCord checkout so the path matches:

```text
<src>/testcordplugins/lyricsStatus/index.tsx
```

Example for a standard TestCord tree:

```bash
git clone https://github.com/DavidHiFi/tidal-lyric-sync.git
cp -r tidal-lyric-sync/src/lyricsStatus <TestCord>/src/testcordplugins/lyricsStatus
```

Then rebuild / reinstall your client mod as usual (for TestCord: `pnpm build` and inject, or use your existing dev workflow).

After a rebuild, **fully restart Discord** (quit the process, then start it again). A simple `Ctrl+R` reload can keep serving a cached renderer and will not pick up the new plugin code.

## Usage

1. Open **TestCord → Plugins → LyricsStatus**.
2. Set **Source** to `TIDAL via TIDALuna` or `Spotify`.
3. Optionally edit **Format**, stop behaviour, and the panel button.
4. Play a track — the lyric line appears as your custom status while it plays.

### Settings

| Setting | Description |
| --- | --- |
| Format | Status template. `{lyrics}` = current line, `{song}` = track, `{artist}` = artist |
| Source | `TIDAL via TIDALuna` or `Spotify` |
| Custom message on stop | Write a fixed status when music stops or the plugin is disabled |
| Custom message | Text used by the option above (blank clears the status) |
| Restore status on stop | Put back the custom status from before music started |
| Show panel button | Toggle the music-note button in the user area panel |

## How it works

1. Subscribe to playback state (TIDAL store or Spotify player events).
2. Fetch synced lyrics for the current track from LRCLIB (cached per track id).
3. On a short timer, pick the line matching `position + lyric delay`.
4. Write that text into your Discord custom status setting, rate-limited and retried once on failure.

## Credits

- Original **LyricsStatus** plugin by **Sharp** and **x2b** in [TestcordDev/Testcord](https://github.com/TestcordDev/Testcord)
- TestCord / Vencord / Equicord ecosystem
- [LRCLIB](https://lrclib.net) for public synced lyrics
- [TidaLuna](https://github.com/Inrixia/TidaLuna) for the local TIDAL control API

## License

[GPL-3.0-or-later](LICENSE) — same license as the upstream TestCord source.
