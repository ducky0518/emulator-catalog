# Emulator catalog

For each video game system: the emulators worth using, in order, and the files each one opens
directly — including whether it opens `.zip` / `.7z` archives itself. Tools that prepare games for
a device (copying a collection to a drive, converting only what an emulator can't read) use it to
decide what to copy as is and what to convert.

`emulator-catalog.json` is the whole catalog. Game Manager Codex checks this file weekly and uses
a newer version when one is published.

## Where the data comes from

- **Which emulators, and their order:** the [Emulation General Wiki](https://emulation.gametechwiki.com/)'s
  recommendations — the "Recommended" column of each system page linked from its Main Page
  (✓ recommended, ~ semi-recommended), limited to emulators that run on Windows, Linux, macOS or
  Android. Where the wiki groups forks ("PCSX2-based", "Xenia-based", "Citra-based",
  "yuzu-based", "Ryujinx-based"), the catalog lists the current projects it names.
- **File types and archive support:** each emulator's own documentation.
- Systems the wiki has no page for list MAME, or the emulator the system is known for.

## Format (schema 1)

```json
{
  "schema": 1,
  "version": "2026.10.04",
  "updated": "2026-10-04",
  "source": "...",
  "emulators": {
    "duckstation": { "name": "DuckStation", "os": "WLMA" },
    "retroarch:swanstation": { "name": "RetroArch: SwanStation", "os": "WLMA", "kind": "retroarch" }
  },
  "systems": [
    {
      "key": "psx",
      "name": "PlayStation",
      "wiki": "PlayStation emulators",
      "launchbox": ["Sony Playstation"],
      "emulators": [
        { "id": "duckstation", "rating": "recommended",
          "opens": ["cue", "bin", "chd", "iso", "img", "ecm", "mds", "pbp", "m3u", "exe"],
          "archives": [] }
      ]
    }
  ]
}
```

- `version` — dated (`YYYY.MM.DD`, optionally `.N`); bump it for every change. Readers only switch
  to a higher version.
- `emulators` — every emulator, by id. `os`: `W` Windows, `L` Linux, `M` macOS, `A` Android.
  `kind`: `standalone` (default), `retroarch` (a RetroArch core) or `native` (nothing to emulate).
- `systems[].launchbox` — the [LaunchBox](https://www.launchbox-app.com/) platform names the
  system covers, so a platform named exactly like one is matched to it.
- `systems[].emulators` — most recommended first. `rating`: `recommended` (✓), `semi` (~), `core`
  (the RetroArch core the wiki lists), `added` (known emulator for a system with no wiki page) or
  `native`. `opens`: file extensions, lower case, no dot (`folder` means the game is a folder).
  `archives`: `zip` / `7z` the emulator opens itself. `notes`: BIOS needed, formats it can't
  read, etc.

## Local additions

Game Manager Codex also reads `emulator-catalog.local.json` (same format; systems and emulator
entries in it replace or add to the catalog's) from its data folder, and can export the emulators
and corrections made in the app in that format. Corrections are welcome here as pull requests or
issues.
