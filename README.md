<img width="1920" height="1020" alt="Screenshot 2026-04-20 124601" src="https://github.com/user-attachments/assets/53b17431-bed4-40a6-a6f1-5e237af49d4a" />

# Kura — PS3 / PS4 / PS5 Game Manager

A fast, offline-first Electron desktop app for scanning, organising, and installing PS3, PS4, and PS5 games across local drives, network shares, and FTP consoles.

---

## Features

### Scanning
- **All Drives** — scans every local drive in parallel (C: first, then all others simultaneously)
- **PS3** — PKG files (both `\x7fPKG` magic), JB folders (BLES/BCUS/BCES/NPEB/NPUB etc. with `PS3_GAME/PARAM.SFO`), and PS3 disc ISOs (read via ISO9660 directory walk, ~12 KB per disc regardless of 25-50 GB size)
- **PS4 PKG** — reads PKG headers (`\x7fCNT` magic) directly; extracts title, content ID, region, firmware, cover
- **PS5 Folders** — finds `sce_sys/param.sfo` and `sce_sys/param.json` (native PS5 metadata); correctly uses the game root folder, not `sce_sys/` itself
- **PS5 FTP with PS4 FPKGs** — detects PS4 FPKGs installed on PS5 under `/user/app/<CUSA>/` via random-access PKG header reads over FTP (2-3 MB per game regardless of PKG size)
- **FTP Console** — 3-attempt strategy per game: `sce_sys/param.json` → `param.sfo` fallback → PPSA folder-name last resort; 350 ms inter-op delay between connections to protect the PS5 FTP daemon
- **Network safe** — `readdirSafe()` with hard timeouts (8 s network / 10 s local); adaptive concurrency (4 workers on SMB/UNC, 16 on local); `UV_THREADPOOL_SIZE=128` for maximum I/O parallelism
- Scan depth: **12 levels** (All Drives caps at 8 to avoid system directories)
- Skips: `$Recycle.Bin`, `Windows`, `Program Files`, `node_modules`, `boot`, `efi`, `msocache`, and more

### Accurate Platform Detection
Kura reads each PKG's header fields to identify the platform correctly:
- **Magic bytes** at offset 0x00 distinguish PS3/PSV (`\x7fPKG`) from PS4/PS5 (`\x7fCNT`)
- **Content-type field** at offset 0x74 distinguishes PS4 (0x1A–0x1F) from PS5 (0x20–0x2F)
- **Title ID prefix** acts as a fallback (PPSA→PS5, CUSA→PS4, BLES/BCUS/NPUB→PS3)
- Uncertain PKGs are badged as `PS4?` or `?` — Kura never silently labels unknowns as PS4

| Badge | Meaning |
|-------|---------|
| `PS3`  | Authoritative PS3 PKG — won't install on PS4/PS5 |
| `PS4`  | Authoritative PS4 PKG |
| `PS5`  | Authoritative PS5 PKG |
| `PSV`  | PS Vita PKG (shares PS3 magic but has different title prefix) |
| `PS4?` | PS4/PS5 magic matched but content-type unrecognised — likely PS4 |
| `?`    | Neither magic matched — probably corrupt or incomplete PKG |

### Game Library
- Live filtering by name, title ID, or filename
- Category tabs: GAME · PATCH · DLC · HOMEBREW · APP · CLASSIC · MINI · DEMO · THEME · PS3 · PS4 · PS5 · INSTALLED · DUPES
- Sort by title, size, region, firmware, version, type
- Grid and table views
- Duplicate detection by Content ID + version + region (cross-drive, cross-session)
- Size calculation runs in background after scan — library appears immediately, sizes fill in. No time-based caps; runs to completion even for 150+ GB games.

### Transfers
- Copy or Move to any local folder or FTP destination
- **PS3 / PS5 folder-format transfers** use recursive FTP upload/download (`uploadFromDir` / `downloadToDir`) so the whole `BLES01234/PS3_GAME/USRDIR/...` tree arrives intact
- Layout presets for PS4/PS5: **etaHEN**, **ItemzFlow**, **Dump Runner**, **Porkfolio**, **Game Title**, **Game/PPSA**, **PPSA Only**, **Title ID**, **By Category**, **GoldHEN / CFW**, **Custom Rename**
- Layout presets for PS3 webMAN: **GAMES/**, **GAMEZ/**, **GAMEI/**, **PS3ISO/**, **packages/** — each maps to a webMAN MOD-scanned path
- Smart layout auto-select: when 60%+ of selected items are PS3, the dropdown switches from a PS5-only layout to the right webMAN path automatically
- Conflict detection with Rename / Skip / Overwrite per-file
- After MOVE: empty parent folders are automatically cleaned up (up to 8 levels)
- Atomic writes (`.part` → rename) — no corrupt partial files on crash

### Install to Console
Three install methods — Kura's toolbar buttons enable/disable automatically based on the selected PKGs' platform badges:

| Method | Platforms | Port | Button enables when... |
|--------|-----------|------|------------------------|
| **ETAHEN / RPI** | PS4 | 12800 (installer) + 8091 (PKG server) | PS4 PKGs selected |
| **VOIDSHELL** | PS4, PS5 | 7007 / 9200 / 9300 (auto-detect) | PS4/PS5 PKGs selected |
| **WEBMAN** | PS3 only | 80 (webMAN HTTP) + 8091 (PKG server) | PS3 PKGs selected |

- **etaHEN / RPI** — POSTs `{type,packages}` JSON to `/api/install`; serves PKG over HTTP; polls task status for progress
- **VoidShell** — same POST pattern but with port auto-detection across VoidShell versions
- **webMAN** — GET to `http://<PS3>/install.ps3?<PKG_URL>`; triggers webMAN to download into `/dev_hdd0/packages/`; final install step is manual (XMB → Game → Package Manager → Install Package Files — Sony security gate)

Mixed-platform selections are filtered to the right platform for each button with a visible warning count.

Install progress tracked via console polling (etaHEN/VoidShell) or byte-served counter (webMAN); cancel supported.

### User-Visible Timers
All four elapsed-time displays (scan, transfer, install, PS5 transfer) use a shared **bulletproof timer factory** with three-tier fallback chain:
1. **Web Worker** (primary) — runs on a separate OS thread, cannot be blocked by main-thread work
2. **requestAnimationFrame** (fallback) — runs on repaint cycles
3. **setInterval** (last resort)

Result: real-time seconds that never stall, jump, or skip during heavy scan-result ingestion or FTP progress bursts.

### Themes
Seven built-in themes. Click **Made by Nookie** in the top-right corner to cycle through them:

| Name | Style |
|------|-------|
| Phantom | Default dark |
| Bloodborne | Deep crimson |
| Horizon | Ocean blue |
| The Last of Us | Forest green |
| Uncharted | Warm amber |
| Death Stranding | Cool slate |
| ★ WipEout | Neon racing |

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+A`       | Select all visible items |
| `Ctrl+F` / `K` | Focus search box |
| `Delete`       | Delete selected items |
| `Enter`        | Install selected PKGs (platform-matching button must be enabled) |
| `F5`           | Re-scan current source |
| `Space`        | Toggle selection on hovered row |
| `↑` / `↓`      | Navigate table rows |
| `Escape`       | Close modal / deselect all |
| `?`            | Open keyboard shortcuts panel |
| Double-click   | Open game info panel |
| Right-click    | Per-item context menu |

---

## Layout Presets Explained

### PS4 / PS5 layouts
| Preset | Output path |
|--------|-------------|
| etaHEN | `dest/etaHEN/games/Game Title (01.000)/` |
| ItemzFlow | `dest/games/Game Title (01.000)/` |
| Dump Runner | `dest/homebrew/Game Title (01.000)/` |
| Porkfolio | `dest/Game Title (01.000) PPSA00000/` |
| Game Title | `dest/Game Title (01.000)/` |
| Game Title / PPSA | `dest/Game Title (01.000)/PPSA00000/` |
| PPSA ID Only | `dest/PPSA00000/` |
| Title ID | `dest/CUSA00000/game.pkg` |
| By Category | `dest/GAME/Game Title/game.pkg` |
| GoldHEN / CFW | `dest/data/GoldHEN/games/Title/game.pkg` |
| Custom Rename | User-defined format with `{TITLE}`, `{TITLE_ID}`, `{VERSION}`, `{REGION}`, `{CATEGORY}`, `{FW}` |

### PS3 webMAN / multiMAN layouts
| Preset | Output path | For |
|--------|-------------|-----|
| `GAMES/`    | `dest/GAMES/BLES01234/` | JB folder games — webMAN mounts and runs |
| `GAMEZ/`    | `dest/GAMEZ/BLES01234/` | alt JB folder path (legacy multiMAN) |
| `GAMEI/`    | `dest/GAMEI/BLES01234/` | PSN PKGs extracted to folder |
| `PS3ISO/`   | `dest/PS3ISO/BLES01234.iso` | disc images — webMAN mounts ISOs directly (no extraction) |
| `packages/` | `dest/packages/BLES01234.pkg` | raw PKG files for install via XMB Package Manager |

---

## Console Safety

Kura is designed to never crash, kernel panic, or corrupt game databases on connected consoles:

- **FTP scans are read-only** — no writes to the console during scanning
- **350 ms inter-op delay** between every FTP connection during PS5 scanning — prevents TCP TIME_WAIT socket exhaustion that crashes the PS5's FTP daemon
- **Skip list** — `cache_ps5`, `shader_ps5`, `shadercache`, `savedata`, `trophy`, `sce_module`, `sce_atlas` and 15+ other dirs are never traversed (thousands of tiny files can hang the FTP daemon)
- **FTP pool capped at 4** — etaHEN/ftpsrv supports 3–4 simultaneous connections; Kura stays within this limit
- **Throwaway FTP clients** for range-read operations (PS5 FTP PKG parsing) — destroying a data socket to abort a partial read would otherwise taint the shared scan client
- **PKG install is pull-only** — Kura runs an HTTP server; the console connects to us and downloads. We never push data or write to the console file system
- **Platform mismatch prevention** — WEBMAN button rejects non-PS3 PKGs before sending; ETAHEN/VOIDSHELL do the reverse. No chance of sending a PS5 PKG to a PS3
- **FTP delete** — only deletes the exact file path the user explicitly selects; never touches system paths
- **No writes to `/system`, `/system_data`, `/mnt/sandbox`** — ever

---

## Log File

Located at `%APPDATA%\Kura\kura.log` (Windows).

```

Key events logged: scan start/complete, each transfer file, errors, install pushes (etaHEN/VoidShell/webMAN).
Log rotates at 2 MB (keeps one `.bak`).

---

## Technical Notes

- **Long path aware** — paths over 260 characters work on Windows 10/11 (`longPathAware: true` in the build manifest)
- **Single instance lock** — opening Kura twice focuses the existing window
- **Atomic file writes** — all copies use `.part` → rename to prevent corruption on crash or power loss
- **Library persisted** — game library saved to `%LOCALAPPDATA%\Kura\library-meta-default.json`; loaded on startup so previous scans are restored instantly
- **Cover art cached** separately in `icons-cache-default.json` and loaded on the splash screen before the UI opens — no pop-in on restart
- **All JSON.parse guarded** — every JSON deserialisation is wrapped in try/catch; corrupt library/config files can never crash the renderer

---

*Built by Nookie · PS3 · PS4 · PS5 · all from one window*
