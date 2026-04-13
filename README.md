<img width="1920" height="1020" alt="image2" src="https://github.com/user-attachments/assets/b6e540db-e055-465a-86a7-ce46bf774124" />


# Kura — PS3 / PS4 / PS5 Package Manager

> Scan, organise, rename, and transfer your console game library. Local drives, USB, FTP, and direct console install — all in one app.

**Version 1.0.0** · Windows · Made by Nookie · [Discord](https://discord.gg/BzZC8xRBhJ)

---

## What Kura Does

- Scans local folders, USB drives, and console FTP for PS3/PS4/PS5 PKGs and game folders
- Reads metadata from SFO / param.json: title, version, region, category, cover art
- Organises your library with flexible destination layouts for every homebrew loader
- Batch renames files using tokens (`{TITLE_ID}`, `{TITLE}`, `{VERSION}`, etc.)
- Copies, moves, and installs to PS4/PS5 with a live speed graph and per-file progress
- Supports grid view (cover art) and table view (sortable columns)
- Persists your library to disk — instant load on next launch

---

## Quick Start

| Step | Action |
|---|---|
| **1. Source** | Type a folder path or `ftp://ip:port/path`. Click Browse or All Drives. |
| **2. Scan** | Click **SCAN**. Kura reads all PKGs and game folders and builds your library. |
| **3. Select** | Click rows to select. Use the pill tabs (GAME, PATCH, DLC…) to filter. |
| **4. Layout** | Choose a destination layout from the GOLDHEN / ETAHEN / FLAT dropdown. |
| **5. Dest** | Type a local path or click **CONSOLE** to set an FTP destination. |
| **6. Go** | Click **GO** to copy or move. Click **INSTALL PKG** to push directly to a console. |

---

## PS4 Destination Layouts

| Layout | Output path |
|---|---|
| FLAT | `/dest/GameName.pkg` |
| TITLE ID | `/dest/CUSA12345 (01.00)/GameName.pkg` |
| CATEGORY | `/dest/games/GameName.pkg` |
| RENAME | `/dest/{TITLE_ID} - {TITLE}.pkg` |
| RENAME+SORT | `/dest/games/{TITLE_ID} - {TITLE}.pkg` |
| GOLDHEN | `/data/GoldHEN/games/GameName.pkg` |
| ETAHEN | `/data/etaHEN/games/GameName.pkg` |

---

## PS5 Destination Layouts

| Layout | Output path |
|---|---|
| ETAHEN | `/data/etaHEN/games/Game Name (01.000.000)` |
| ITEMZFLOW | `/games/Game Name (01.000.000)` |
| GAME FOLDER | `/dest/Game Name (01.000.000)` |
| GAME/PPSA | `/dest/Game Name (01.000.000)/PPSA12345` |
| PPSA ONLY | `/dest/PPSA12345` |
| GAME [VER] | `/dest/Game Name [01.000.000]` |
| DUMPRUNNER | `/homebrew/PPSA12345` |
| PORKFOLIO | `/dest/Game Name (01.000.000) PPSA12345` |

---

## Remote Install

### PS4 — etaHEN / RPI

1. Open **INSTALL PKG**, select **ETAHEN / RPI** tab.
2. Click **Find IP** to auto-detect your PS4 on the network, or enter it manually.
3. Click **INSTALL TO PS4/PS5** — Kura starts a local HTTP server, sends the URL to your console, and monitors download progress with a live speed graph.
4. The progress modal shows current speed, ETA, per-PKG progress bar, and accurate completion stats.

> **Cancel detection** — Kura detects when the PS4 cancels the download via TCP disconnect (instant) or 8-second request timeout (fallback). The modal shows "Cancelled by console" automatically.

### PS5 — VoidShell 2.0

1. Open **INSTALL PKG**, select **VOIDSHELL** tab.
2. Enter your PS5 IP (or click **Find IP**).
3. Ports: `9200` (v2.0 primary), `9300` (v2.0 fallback), `7070` (v3.0).
4. Click **Send to VoidShell** — Kura tries all endpoints automatically and copies the install URL to clipboard as a fallback.

---

## FTP

| Console | Server | Default port |
|---|---|---|
| PS4 / PS5 | GoldHEN FTP or `ftpsrv.elf` | `2121` |
| PS3 | webMAN FTP | `21` |

Enter `ftp://192.168.x.x:2121` in the Source or Dest field, or click **CONSOLE** to set up a connection profile. Common console paths appear as quick-access chips.

---

## Batch Rename

Select items → Menu → **Batch Rename**. Type a format string using any combination of tokens:

| Token | Value |
|---|---|
| `{TITLE_ID}` | CUSA12345 / PPSA12345 / BLES00001 |
| `{TITLE}` | Game title from SFO / param.json |
| `{VERSION}` | Full version string (01.00 / 01.000.000) |
| `{FW}` | Required firmware version |
| `{CATEGORY}` | GAME / PATCH / DLC / CLASSIC |
| `{REGION}` | US / EU / JP / ASIA |
| `{CONTENT_ID}` | Full content ID string |

A live preview shows all selected items with their resolved names before you apply. Conflict handling: Skip / Overwrite / Keep Both (auto-numbered suffix).

---

## Conflict Resolution

When copying to a destination that already has a file with the same name, Kura shows a conflict modal with three options:

| Choice | Behaviour |
|---|---|
| **Skip** | Item is excluded from the transfer entirely |
| **Overwrite** | Existing file is truncated and replaced |
| **Keep Both** | Destination filename gets a numbered suffix: `Game (1).pkg`, `Game (2).pkg` |

Check **Apply to all remaining** to apply your choice to every conflict in the batch without further prompts.

---

## Transfer Progress Modal

Every copy, move, and install shows a full-screen modal with:

- **Live speed graph** — purple waveform updated every 120ms
- **ETA** — calculated from EWMA speed and bytes remaining
- **File bar** — current file progress (0–100%)
- **Overall bar** — grand total across all items
- **Completion cards** — TRANSFERRED, AVG SPEED, PEAK, TIME with source labels

> If byte size is unavailable (instant same-filesystem rename), the speed cards show `files/s` instead of N/A.

---

## Themes

Switch via **Menu → THEME**. Persisted across sessions.

| Theme | Accent |
|---|---|
| Default | Indigo `#6366f1` |
| Crimson | Red `#ef4444` |
| Ocean | Cyan `#22d3ee` |
| Forest | Emerald `#10b981` |
| Amber | Gold `#f59e0b` |
| Slate | White `#e2e8f0` |

---

## Profiles

Multiple profiles let you maintain separate libraries for different drives or setups. Switch via the profile dropdown at the top right. Each profile has its own `library-meta-<name>.json` and `library-icons-<name>.json` in `%LOCALAPPDATA%\Kura\`.

---

## Auto-Update

Kura checks for updates automatically on launch via the GitHub releases endpoint:
`https://github.com/NookieAI/kura/releases/latest/download/latest.json`

When a new release is available, a dialog prompts you to install. Updates are signed and verified against the embedded public key before installation.

---

## Troubleshooting

**Splash stuck loading covers**
The cover cache file may be oversized. Kura detects this automatically and rebuilds — you will see a toast on that launch. Subsequent launches will be fast. To clear manually: delete `%LOCALAPPDATA%\Kura\library-icons-default.json`.

**Covers not loading on first launch**
Kura scans 12 PKGs concurrently with an 8-second timeout per file. On slow USB/network drives, the first scan can take 20–40 seconds. All covers are cached after the first scan.

**Transfer stuck at 0%**
Check that the destination path exists and is writable. For FTP destinations, verify the console is connected and the FTP server is running. Check `kura.log` for the specific error.

**Install — no progress / timer frozen**
Make sure your PC firewall allows Kura to serve HTTP on the configured port (default 12800). Check that the PS4 IP is reachable. The timer now continues counting even if the Tauri IPC call rejects early — the install keeps running.

**Library lost after crash**
Version 1.0.0 uses atomic writes (write to `.tmp` → rename). Crashes during save no longer corrupt your library. If you're on an older version and lost your library, it may be in `%LOCALAPPDATA%\Kura\` as a partial file.

**PS4 shows "Cancelled by console" immediately**
PS4 may have queued the download — check Notifications on your PS4. The download continues from the PS4 side even after Kura shows cancelled.

---

## Log File

`%LOCALAPPDATA%\Kura\kura.log`

Open via **Menu → View Log File**. Format: `[HH:MM:SS.mmm] [LEVEL] [TAG] message`. Covers startup, scan, transfer, install, and library events.

---

## Building from Source

Requirements: Rust 1.77+, Node.js 18+, Windows.

```bash
# Install JS dependencies
npm install

# Dev mode (hot reload)
npm run tauri dev

# Release build
npm run tauri build
```

Output: `src-tauri/target/release/kura.exe` and NSIS installer in `src-tauri/target/release/bundle/`.

---

## Community

Discord: **https://discord.gg/BzZC8xRBhJ**

GitHub: **https://github.com/NookieAI/kura**
