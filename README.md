# Kura — one tool for your PlayStation library

Kura is a free Windows app that scans your drives, identifies every PlayStation game you have, and helps you **organise, transfer, and install** them — across **PS1, PS2, PSP, PS3, PS4, PS5, and PS Vita**, all in one library.

No installer. No login. No account. Just a portable `.exe`.

## Download

Grab the latest **`kura.exe`** from the [Releases](https://github.com/NookieAI/kura/releases/latest) page and run it — no installation, no admin rights, no Node.js.

> Each release also bundles **`kura-loader.elf`** — the Kura Loader payload for PS5. See [Kura Loader (PS5)](#kura-loader--the-ps5-payload) below.

## What makes it different

- **🎯 It reads inside the files.** Kura cracks open the PKG header, the ISO9660 directory, the SFO metadata, the ZIP central directory, the CSO sector index — and pulls out the real title, title ID, version, content ID, region, firmware requirement, and cover art. `FF7-REMAKE-PS4-FIXED-FIXED2.pkg` shows up as *Final Fantasy VII Remake*.
- **🧩 Every PlayStation, one library.** PS1 disc images, PS2 ECM dumps, PSP UMDs/CSOs, PS3 JB folders and PKGs, PS4 fpkg/exFAT backups, PS5 folder installs, PS Vita VPKs — same library, same filters, same workflow.
- **🚀 Fast scans.** All drives scan in parallel; compressed formats use partial decompression (only the sectors needed for metadata), so a 1.8 GB CSO scans as fast as a plain ISO.
- **🛡️ Structural, not guesswork.** Classification uses the SFO TITLE_ID prefix, the PNG icon signature, the BOOT line in SYSTEM.CNF — actual format-defined fields, not filenames.
- **🎛️ Layouts for every setup.** Presets for Itemzflow, webMAN, etaHEN, Dump Runner, GoldHEN, and more — plus a custom-rename builder.
- **🔌 Direct console transfer.** Transfer over FTP straight to the console; install PKGs through the right endpoint for your firmware. (PS5 transfers respect the PS5 FTP daemon's throttling so they don't stall.)
- **📦 Duplicate detection.** Groups the same game across drives / versions / regions (content ID + version + region) and shows side-by-side what differs.

## Kura Loader — the all-in-one PS5 payload

**Kura Loader** is Kura's companion **PS5 homebrew payload**, shipped as **`kura-loader.elf`** and bundled with every release. It's a **single self-contained ELF** — one payload to load on your jailbroken PS5 that brings up a full **web dashboard** *and* a stack of the scene's best tools, with no extra downloads. The desktop app talks to it over your LAN to light the PS5 up like every other platform.

> **One-click (or zero-click) install:** connect a jailbroken PS5 that's missing it and Kura fetches, pushes, and launches `kura-loader.elf` for you automatically — or use **Menu → Set up my PS5** to do it on demand.

### ⭐ ShadowMountPlus — play games straight from USB

Kura Loader bundles and manages **[ShadowMountPlus](https://github.com/drakmor/shadowMountPlus)** by **Drakmor** — a game **mount engine** that lets your PS5 **run titles directly from external/USB or network storage**, without copying them into the internal SSD first. Mount a game and play it; unmount when you're done. It's the cleanest way to enjoy a big library on a console with limited internal space, and Kura keeps it **auto-updated** to the latest build for you (one click in the dashboard).

### 🛠️ Everything else it includes

A jailbreak toolbox in one payload, all driven from the dashboard or the desktop app:

- **🎮 Install anything** — PKG install + upload, URL-push install, disc/folder installs, and tile auto-install. PS4 *and* PS5 packages.
- **📚 Game library + app dumper** — reads the console's own database for real titles, and can **dump installed games** back out.
- **🩹 Cheats & patches** — a built-in cheats engine (**1,500+** cheats) and **game patches** (370+), with auto-fetch.
- **💾 Save manager** — backup/restore game saves (Garlic save-manager integration).
- **🌡️ Fan control & thermals** — live CPU temp, custom fan curves, silent/balanced/aggressive presets.
- **🖼️ Avatar maker** — set custom PSN profile avatars.
- **🌐 Network tools** — LAN auto-discovery (mDNS), SMB shares, nanoDNS, network diagnostics.
- **🔌 Plugins + klog** — load plugins, and tap/stream the live kernel log for debugging.
- **🩺 Health & recovery** — process monitor, crash logs, safe-mode, and a built-in `elfldr` so Kura can re-launch it if it ever stops.

### 🖥️ The dashboard (web UI on port 8535)

Open it from Kura (**Menu → Kura Dashboard**) or any browser at `http://<console-ip>:8535/`: a clean, live console — temps & fan, memory/storage, processes, your library, installs, cheats, patches, save manager, network, logs, and more, all updating in real time.

### Get & use it

- Attached to each [release](https://github.com/NookieAI/kura/releases/latest) as `kura-loader.elf`; the app's **Menu → Update kura.elf from GitHub** and the auto-setup keep it current.
- **Requires a jailbroken PS5** on compatible firmware with a payload loader (e.g. etaHEN) — then just open the console from Kura (or let auto-setup do it).
- Full walkthrough: in-app **Menu → Help & Guide** → *"Kura Loader ELF (PS5 Dashboard)"*.

*PS5 console features are optional — the desktop app's scanning, organising and metadata work on any Windows PC without it.*

### Credits

Kura Loader stands on the shoulders of the PS5 homebrew scene — most notably **[ShadowMountPlus](https://github.com/drakmor/shadowMountPlus)** by **Drakmor**, plus the wider ps5-payload-dev ecosystem. Huge thanks to everyone who builds and shares these tools. 🙏

## Supported formats

ISO, BIN/CUE, IMG, CHD, CSO/ZSO, ECM, PBP, PKG (PS3/PS4/PS5), fpkg, exFAT game images, folder dumps, VPK, and compressed archives (ZIP/RAR/7z) — scanned in place via partial decompression.

## Privacy

Core operation is **fully offline**. The only optional online calls are cover-art / GameDB lookups (which you trigger). No telemetry, no account, no phoning home.

## Platform

Windows (portable `.exe`). macOS/Linux aren't officially supported (Wine may work but is untested).

## Links

- 📖 **In-app Help & Guide** (Menu → Help & Guide) — full walkthrough, layouts, console setup, troubleshooting
- 🐞 [Issues](https://github.com/NookieAI/kura/issues) — bug reports & requests
- 💬 Discord — invite link is in the app's top-right **Discord** button

---

*Kura is built and maintained by Nookie. No company, no investors, no plan to monetize. If you like it, tell a friend.*
