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

## Kura Loader — the PS5 payload

**What it is.** Kura Loader is Kura's companion **PS5 homebrew payload**, shipped as **`kura-loader.elf`** and bundled with every release. It runs on a **jailbroken PS5** and is what unlocks Kura's PS5 console features. (It's a sibling project to the desktop app — the `.exe` and the `.elf` are released together.)

**What it does.** Once loaded on the console, `kura-loader.elf` runs a lightweight on-console server that the Kura desktop app talks to over your LAN:
- **Console dashboard** (web UI on port **8535**) — live temps/thermals, fan, network, storage, system health, save manager, trophies, avatars, crash logs, and more.
- **Game discovery & real names** — reads the console's own app database so installed PS5 games show up with their **real titles** (not raw `CUSA…`/`PPSA…` IDs).
- **File access & transfer** — an FTP/kfs service (port **2121**) so Kura can browse, transfer, and install directly to the console.
- **Auto-discovery** — announces itself on the LAN (mDNS) so the app can find your console with one click.

**What it's for.** It's the bridge between the desktop app and your PS5. **Without it**, Kura still works great for scanning and organising local files — but it can't see *installed* PS5 games, resolve their real names, or install to the PS5. **With it**, the PS5 lights up like every other platform.

**How to get & use it.**
- It's attached to each [release](https://github.com/NookieAI/kura/releases/latest) as `kura-loader.elf`, and the app's **Menu → Update kura.elf from GitHub** fetches the latest automatically.
- Load it on a **jailbroken PS5** via your payload loader (e.g. etaHEN), then open the console from Kura.
- Full step-by-step setup is in the app's **Menu → Help & Guide** → **"Kura Loader ELF (PS5 Dashboard)"** (start there for PS5).

**Requirements.** A jailbroken PS5 on compatible firmware with a payload loader. PS5 console features are optional — the rest of Kura needs none of this.

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
