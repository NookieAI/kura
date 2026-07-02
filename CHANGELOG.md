# Kura — Changelog

All notable changes to the Kura desktop app. Newest first.

## 1.1.1 — Kura-branded payload + a steadier console link

- **Fully Kura-branded console payload.** Every on-console popup now reads *Kura Loader* instead of leaking bundled-component names. Rebuilt + bundled, verified in the binary.
- **No more 60-second freeze when the console is busy.** Feature-sync now steps back cleanly in a few seconds if the PS5 payload is momentarily unresponsive, instead of hanging the app.
- **Steadier under the hood.** Version/build metadata stays in lockstep across every file; the full test suite is green top-to-bottom.

## 1.1.0 — seven new tools + reliability

- **Back up all saves in one click.** The Save Manager gets a *Back up all* button — pick one folder and every visible save is downloaded as its own .zip (sealed saves skipped), with a live progress bar.
- **See what's *not* on your console — and send it.** New console:no / console:yes search filters, plus a **Transfer Missing** button that selects every local game not yet on your console and hands it to the transfer queue.
- **Export a printable HTML catalogue.** Builds a self-contained web page of your whole library — cover art embedded, grouped by platform, with sizes and totals.
- **Trophy Overview dashboard.** A new Settings panel rolls up your trophy bundles per user with a completeness bar, cached for offline viewing.
- **Genre & release year.** PS5 catalogue titles now carry genre + year — filter with genre:/year:, sort by Year, and see both in a game's Info panel.
- **Console Cleanup advisor.** Lists installed games largest-first and flags the ones you already have a local copy of as safe to remove, with a guarded one-click delete.
- **Resumable transfers.** An interrupted transfer is remembered and offered for resume on next launch — already-uploaded bytes are skipped automatically.
- **Reliability & accessibility.** Focus-trapped transfer dialog, surfaced background errors, and size-capped inbound console replies.

## 1.0.5 — Cross-platform and install reliability

- **Mac and Linux auto-update now works.** The updater offers the right build for your system (.dmg for Mac, .AppImage for Linux) and opens it to install, instead of mistakenly handing Mac and Linux users a Windows file.
- **No more false "install succeeded".** Small DLC, theme, and unlock packages the console actually rejected could be reported as installed — Kura now confirms the file really transferred before calling it done.
- **Accurate progress when installing several games at once.** Each item's progress is now tracked on its own, so one item can no longer inflate the next one's percentage.
- **Clearer transfer errors.** A failed transfer now adds plain-language guidance (for example, that the PS5 may be offline or on a different network) instead of showing only a raw error code.
- **Better Mac and Linux support.** "Scan all drives" now finds external and USB volumes, setup guidance no longer says "Windows Firewall" on other systems, and recovery messages point to the right folder for your OS.
- **More robust under the hood.** Assorted stability and cleanup fixes, plus a build fix so releases publish reliably even when a temporary storage quota is hit.

## 1.0.4 — Completes the security update

- **Blank or anonymous FTP passwords are no longer saved** — and any previously stored blank password is cleared — so you won't run into confusing failed connections later (this is the common PS5 case).
- **Finishes rolling out the previous privacy and security update.** A file was accidentally left out of v1.0.3, so this release ships the remaining fix and makes the in-app version number correct and consistent.
- **Everything from the v1.0.3 hardening is now fully in place**, including your console login being encrypted on your computer, diagnostic reports no longer containing your login token, and exported settings no longer including your credentials.

## 1.0.3 — Privacy and security hardening

- **Diagnostic and incident reports are now safe to share** — they no longer include your console's dashboard login token, other secret-looking values (passwords, keys, tokens) are automatically masked, and the report now carries a warning reminding you to review it before posting it publicly.
- **Your console login tokens are now encrypted at rest** using your computer's built-in secure storage (Windows, macOS, or Linux) instead of sitting in plain text. Existing tokens upgrade automatically the next time they're saved (on systems without a keychain, they stay as-is).
- **Exported settings files no longer contain your login tokens**, so sharing or backing up your settings won't leak credentials.
- **Tighter network protection during game installs** — on Windows, the temporary file server used to transfer games is now limited to your local network instead of being reachable from anywhere.

## 1.0.2 — Smoother, lighter theme switching

- **Faster, lighter theme switching.** Changing themes on a large library used to freeze the app for up to a second before anything happened. Kura now swaps the theme instantly and plays a soft accent glow at the spot you clicked — the switch feels light and smooth, with none of the previous stutter or lag.
- **Even filter-bar text.** The count numbers on the category and platform chips (ALL, GAME, PS1, PS4, and the rest) now match the size of their labels, so the whole filter bar reads consistently.

## 1.0.1 — Scan big drives without crashing

- **Fixed the crash when scanning a NAS or network share** — pointing Kura at a large SMB/NAS drive (like a 16 TB share) could hard-crash the app mid-scan. It now safely skips oversized or corrupt files instead of running out of memory.
- **Much lighter on memory for huge libraries** — Kura no longer keeps every game's full-size cover loaded during a scan, and covers are shrunk in the background as they arrive, so five-figure collections stay light on RAM.
- **Smoother browsing of massive collections** — the library view now caps how many game cards load at once and shows a "showing the first N of M — narrow the search" note, instead of choking while building tens of thousands of covers.
- **Faster scanning over slow network drives** — folder-size calculations no longer flood slow SMB shares with an endless storm of lookups.
- **Clear macOS first-launch fix** — if macOS says the app is "damaged," a one-line Terminal command gets you running; it's just an unsigned-app block, not real damage.

## 1.0.0 — One library, every PlayStation

- **Now runs everywhere** — native builds for Windows, macOS (Apple Silicon and Intel), and Linux, all free with no installer, login, or account.
- **Sees more of your collection** — added support for ZSO games, PS3 disc images, extracted PS Vita folders, and more disc formats, plus RAR and ZIP archive scanning built right into the main scan (unsupported .7z is flagged "extract first" instead of quietly skipped).
- **Cleaner, more accurate library** — games are identified by what's actually inside the file, so PC-game installers that share the .pkg extension no longer sneak in as fake PlayStation entries.
- **New console tools** — a live kernel-log viewer, Process Manager, and Trophy Manager join the existing one-click FTP install, duplicate finder, and save manager.
- **Smarter PS5 companion** — every install now shows an on-console "Installed: Kura Loader" confirmation, restart reasons are logged instead of showing "unknown," kernel-log noise is filtered out by default, and diagnostics highlight recent restart reasons.
- **More polish** — a smooth new Liquid Ink-Bleed theme transition, steadier live timers, and tidied-up menus, options, and help.
