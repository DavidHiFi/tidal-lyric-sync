# tidal-lyric-sync

A TestCord/Vencord plugin that puts the **current lyric line** from your music into your **Discord custom status**.

- **TIDAL** via [TidaLuna](https://github.com/Inrixia/TidaLuna) (local websocket `ws://localhost:24123`)
- **Spotify** via the desktop client (`SPOTIFY_PLAYER_STATE`)
- Lyrics timed from [LrcLib](https://lrclib.net)

Works with the music controls shipped in [TestCord](https://github.com/TestcordDev/TestCord) (Panel Layout → Music Controls), including shared lyric delay and per-song delay.

## Requirements

| Piece | Needed for |
| --- | --- |
| [TestCord](https://github.com/TestcordDev/TestCord) (or a compatible Vencord-style client) | Running the plugin |
| Music Controls / Panel Layout (bundled with TestCord) | TIDAL + Spotify playback state, lyric delay |
| [TidaLuna](https://github.com/Inrixia/TidaLuna) in the TIDAL desktop app | TIDAL playback → local websocket |
| Spotify desktop app | Spotify playback events |
| Network access to `lrclib.net` | Fetching synced lyrics |

Without TidaLuna, select **Spotify** as the source (or run without lyrics until playback is available).

## Install

1. Clone this repository next to your TestCord checkout (or copy the plugin folder in).
2. Ensure the plugin lands at `src/testcordplugins/lyricsStatus/index.tsx` inside TestCord:

   ```text
   TestCord/
     src/
       testcordplugins/
         lyricsStatus/
           index.tsx   ← this file
   ```

   If you already have a `lyricsStatus` folder, back it up before replacing it.

3. Build TestCord as usual (`pnpm build` from the TestCord root).
4. Restart Discord fully (quit the process, then open it again). Reloading the window alone is not enough for a rebuilt renderer on some injectors.
5. Enable **LyricsStatus** in TestCord plugin settings.

### Using this repo as a git remote for the plugin only

```bash
# inside TestCord
git remote add tidal-lyric-sync /path/to/tidal-lyric-sync
git fetch tidal-lyric-sync
git checkout tidal-lyric-sync/main -- src/testcordplugins/lyricsStatus
```

## Settings

| Setting | What it does |
| --- | --- |
| **Source** | `TIDAL via TIDALuna` (default) or `Spotify` |
| **Format** | Status template: `{lyrics}`, `{song}`, `{artist}` — default `🎵 {lyrics}` |
| **Custom message on stop** | Text written when music stops or the plugin disables |
| **Restore status on stop** | Put back the custom status you had before playback |
| **Show panel button** | Toggle button in the user area panel |

There is also a toggle button in the user panel (music-note icon) for enable/disable without opening settings.

## How it works

1. Subscribes to TIDAL (TidaLuna store) or Spotify player state.
2. Fetches synced LRC lyrics from LrcLib for the current track (cached per track id).
3. Every 2s, picks the line matching playback position + lyric delay.
4. Writes your Discord custom status (rate-limited, with one retry) until playback stops.

## Repository layout

```text
src/lyricsStatus/index.tsx   Plugin source (copy to TestCord path above)
LICENSE                      GPL-3.0-or-later
```

## Credits & license

- Upstream plugin: TestCord `lyricsStatus` (TestcordDev/TestCord contributors, including Sharp and x2b).
- TIDAL integration and status reliability fixes layered on top for local use.
- Licensed under [GPL-3.0-or-later](./LICENSE), same as TestCord/Vencord.

Not affiliated with Discord, TIDAL, or Spotify.
