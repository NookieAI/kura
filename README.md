<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/24f04d0c-22f4-4003-bdd8-c0ca6bafb6e2" />

# Kura

**Manage your PlayStation game library across PS1, PS2, PSP, PS3, PS4, PS5, and Vita — all from one place.**

A free, offline desktop app for Windows that scans your drives, organises your collection, and transfers games to consoles or back to your PC. No accounts, no subscriptions, no internet required for the core workflow.

---

## Download & Run

Kura ships as a single portable `.exe` — no installer, no admin rights, no Node.js, nothing else needed.

1. Download the latest `Kura.exe` from the Discord
2. Double-click to run
3. That's it

The app stores its library and settings in `%APPDATA%\Kura\` so you can move the `.exe` anywhere without losing data. Delete that folder to fully reset Kura.

> **Windows 10 or 11 (64-bit) recommended.** Older builds work but performance will suffer on the all-drives parallel scanner.

---

## What Kura Does

### Scan
Point Kura at any drive, folder, FTP console, or "All Drives" and it walks the tree finding games. It recognises PlayStation games on disc, in PKG containers, in folder dumps, and in compressed archives — including formats most other tools ignore.

### Organise
Every game gets a real metadata-derived title (not the filename), the correct platform badge, region, firmware requirement, version, content ID, cover art, and DLC/patch grouping. The library lives in a single JSON file you can back up.

### Transfer
Copy or move games between drives, to a USB stick formatted for PS4/PS5, or directly to a console over FTP. Layout presets cover all the popular custom-firmware setups (etaHEN, GoldHEN, ItemzFlow, webMAN, etc.).

### Install
On compatible consoles Kura can drive the install via FTP or HTTP, including the etaHEN / RPI / VoidShell / WebMAN install endpoints.

---

## Supported Formats

| Platform | Formats |
|---|---|
| **PS1** | `.iso`, `.bin`/`.cue` (single + multi-track), `.img`, `.ecm`, `.chd` (header metadata), PSN `.pkg` |
| **PS2** | `.iso`, `.bin`/`.cue`, `.img`, `.ecm`, `.chd` (header metadata), PSN `.pkg` |
| **PSP** | `.pbp`, UMD `.iso`, `.cso` / `.ciso` (compressed), `.ziso` (LZ4-compressed), folder dumps |
| **PS3** | `.pkg`, JB folder dumps (`BLES.../PS3_GAME/PARAM.SFO`), disc `.iso` |
| **PS4** | `.pkg`, `.fpkg`, game-folder ZIPs, folder installs |
| **PS5** | `.pkg`, `.exfat`, `.ffpkg`, folder installs (with `sce_sys/param.json`), game-folder ZIPs |
| **PS Vita** | `.pkg`, `.vpk`, generic `.zip`, folder dumps (`PCSE.../sce_sys/param.sfo`) |

**Compressed archives:** Kura also peeks inside `.zip` and `.rar` archives (RAR5) to find PlayStation games stored compressed. Read-only — no extraction. STORED (uncompressed) entries inside RARs get full metadata + cover; compressed entries surface as "RAR archive: contains foo.pkg, bar.iso" so you still see the file in your library and know what's in it.

For compressed formats (`.cso`, `.ziso`, `.ecm`) Kura uses **partial decompression** — it decompresses only the few sectors needed to read the metadata, never the whole image. A 1.8 GB CSO scan takes the same time as a regular ISO.

---

## Key Features

### Accurate platform detection
Kura reads PKG headers and PARAM.SFO/PARAM.JSON metadata directly. It distinguishes PS4 from PS5 from PSV using header magic bytes and the content-type field — not just the filename. Uncertain PKGs are badged `PS4?` or `?`; Kura never silently labels unknowns.

### Game library
- Live filtering by name, title ID, or filename
- Category tabs: **GAME · PATCH · DLC · HOMEBREW · APP · CLASSIC · MINI · DEMO · THEME · OTHER**
- Platform tabs: **PS1 · PS2 · PSP · PS3 · PS4 · PS5 · PSV**
- Sort by title, size, region, firmware, version, type
- Grid and table views
- Duplicate detection by Content ID + version + region
- Size calculation runs in the background — the library appears immediately, sizes fill in

### Transfers
- Copy or Move to any local folder or FTP destination
- Folder-format transfers (PS3 JB dumps, PS5 game folders) use recursive FTP so the whole tree arrives intact
- Layout presets for PS4/PS5: **etaHEN**, **ItemzFlow**, **Dump Runner**, **Porkfolio**, **Game Title**, **Game/PPSA**, **PPSA Only**, **Title ID**, **By Category**, **GoldHEN / CFW**, **Custom Rename**
- Layout presets for PS3 webMAN: `GAMES/`, `GAMEZ/`, `GAMEI/`, `PS3ISO/`, `packages/`
- Pre-flight space check across the destination, including all `.bin` companions for multi-track `.cue` sets

### Console install
- **etaHEN** (PS5) — direct install via FTP/HTTP endpoint
- **RPI / VoidShell** (PS5) — JSON pkg list with retry tolerance
- **WebMAN-MOD** (PS3) — `/install.ps3?` HTTP endpoint with progress polling
- **Itemzflow** (PS5) — folder upload + register

### Save data
Kura recognises save data folders (`SAVEDATA/`, `pcsavedata/`, `sce_sys/savedata`) where they exist alongside game data, but does not currently transfer or sync save files between PC and console. Save-data management is on the roadmap, not shipped yet.

### Privacy & offline
Kura never phones home. The only network I/O is:
- FTP/HTTP traffic to the consoles you tell it to talk to
- Optional cover-art lookups from the public libretro thumbnail mirror (you can disable this in settings)
- Optional GameDB downloads for PS1/PS2/PSP title lookups (one-time, cached locally)

No telemetry, no analytics, no account.

---

## Troubleshooting

**Kura doesn't see my games**
Check `%APPDATA%\Kura\kura.log` — every scan attempt is logged with a `[ps12-detect]` or `[pkg-classify]` line explaining why a file was accepted, rejected, or skipped.

**A game shows the wrong platform/badge**
Click the (i) icon on the row to see the metadata. If the title ID prefix looks correct but the badge is wrong, post the log line in Discord.

**Duplicate detection grouped two different games**
Use the **Resolve Dupes** button in the header. Each duplicate group shows the conflicting fields side-by-side; you can keep the one you want, mark the other as not-a-duplicate, or delete it.

**FTP transfers stall on PS5**
The PS5 FTP daemon throttles if you hammer it. Kura adds a 350 ms inter-operation delay automatically. If you still see stalls, drop the concurrency in **Menu → Settings → Transfers**.

**The library has hundreds of games I didn't expect**
Check the source folder. Common causes: a developer machine with `node_modules`, a Go module cache, or a folder of screenshots named after game IDs. Kura skips most of these by default but custom layouts can slip through. The **Filter by name, ID, filename** box is your fastest triage tool.

---

## Where things live

| What | Path |
|---|---|
| Library JSON | `%APPDATA%\Kura\library.json` |
| Settings | `%APPDATA%\Kura\settings.json` |
| Log file | `%APPDATA%\Kura\kura.log` |
| Cover cache | `%APPDATA%\Kura\covers\` |
| GameDB cache | `%APPDATA%\Kura\gamedb-*.json` |

To uninstall: delete the `.exe` and the `%APPDATA%\Kura\` folder.

---

## Help & Community

- **Discord** — top-right of the app for the invite link
- **Bug reports** — paste your `kura.log` (only file paths and titles, no personal info)
- **Feature requests** — Discord #suggestions

Made with care by Nookie.
