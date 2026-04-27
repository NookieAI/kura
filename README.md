<img width="1920" height="1032" alt="image" src="https://github.com/user-attachments/assets/1d9eba45-8356-4985-9847-2062c4163c77" />

# Kura — One Tool for Your PlayStation Library

## What is it?

Kura is a free Windows app that scans your drives, identifies every PlayStation game you have, and helps you organise, transfer, and install them. It works across **PS1, PS2, PSP, PS3, PS4, PS5, and PS Vita** in one library.

No installer. No login. No internet account. Just a portable `.exe`.

---

## Why people use it

> *"It told me I had 47 duplicates I didn't know about."*

> *"First scanner that actually got my .cso files right — title, cover, version, all of it."*

> *"I switched my whole PS5 USB layout to etaHEN in two clicks."*

### The pitch in one paragraph
You have a folder of PlayStation games. Maybe it's on a USB stick, a network drive, your downloads folder, your console's FTP. Kura looks at all of them, figures out what each game actually is (not what the filename says), tells you which ones are duplicates of each other, lets you organise them onto whatever destination you need, and uploads them to your console with one click. It does this without phoning home, without an account, and without you having to know anything about title IDs or PARAM.SFO.

---

## What's different about Kura

### 🎯 It reads inside the files
Most scanners look at filenames and guess. Kura cracks open the PKG header, the ISO9660 directory, the SFO metadata, the ZIP central directory, the CSO sector index — and pulls out the real title, title ID, version, content ID, region, firmware requirement, and cover art. That means **`[FF7-REMAKE-PS4-FREE-DOWNLOAD-FIXED-FIXED2.pkg]`** shows up in your library as **"Final Fantasy VII Remake"**.

### 🧩 Every PlayStation, one library
PS1 disc images, PS2 ECM dumps, PSP UMDs and CSOs, PS3 JB folders and PKGs, PS4 fpkg/exfat backups, PS5 folder installs, PS Vita VPKs and folder dumps — same library, same filters, same workflow. Sort by size and you'll see the 25 GB PS5 game right next to the 80 MB PS1 game.

### 🚀 It scans fast
The all-drives scanner runs every drive in parallel. Compressed formats use partial decompression — only the few sectors needed to read metadata, never the whole image. A 1.8 GB CSO scan takes the same wall-clock time as a regular ISO.

### 🛡️ It's structural, not guesswork
Kura doesn't classify games by filename. It uses the SFO TITLE_ID prefix, the PNG signature on icons, the BOOT line in SYSTEM.CNF — actual format-defined fields. That means a folder of screenshots named "BCES00004-lair-ps3-iso" doesn't get added as 685 fake PS3 games (that's a real bug we shipped a fix for).

### 🎛️ Layouts for every console setup
Itemzflow expects `GAMES/{Title} ({TitleID})/`. webMAN expects `GAMES/{Title}/PS3_GAME/`. etaHEN expects `games/{TitleID}/`. Dump Runner expects something else again. Kura has presets for all of them, plus a custom-rename builder if your setup is unusual.

### 🔌 Direct console transfer
Plug your console into the network. Kura transfers over FTP straight to `/data/`, `/user/app/`, or wherever you've configured. For PS5 it respects the PS5 FTP daemon's throttling so transfers don't stall. For PS3 webMAN it uses the proper HTTP install endpoint so games show up in the XMB without rebooting.

### 📦 Bulletproof duplicate detection
Same game across three drives? Different versions of the same patch? PS4 and PS5 versions of one title? Kura groups them by content ID + version + region and shows you side-by-side what's different. Decide what stays and what goes from one modal.

---

## Who it's for

- People with PlayStation game collections on local storage that need organising
- People who back up to USB sticks for jailbroken PS3/PS4/PS5 consoles
- People with multiple console layouts (etaHEN + GoldHEN + Itemzflow)
- People who want to see what they actually have before buying again
- Anyone who's tired of spreadsheet-managing their game library

## Who it's not for

- People looking for a place to **download** games — Kura doesn't do that
- People who want a phone app — desktop only
- macOS / Linux users — Windows only for now (Wine works but isn't tested)

---

## Try it

1. Grab the latest `Kura.exe` from the Discord
2. Run it (no install)
3. Click **All Drives** and let it scan
4. Tell us what worked and what didn't

The **Discord** button in the top-right of the app has the invite link.

---

*Kura is built and maintained by Nookie. No company, no investors, no plan to monetize. If you like it, tell a friend.*

---

*Built by Nookie · PSX · PS2 · PSP · PSV · PS3 · PS4 · PS5 · all from one window*
