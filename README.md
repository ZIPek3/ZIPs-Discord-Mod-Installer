# ZIP's Discord Mod Installer

Install Discord **client mods** and **userplugins** on Windows without running the usual git / pnpm steps by hand.

ZIP's Discord Mod Installer can install **Vencord**, **Equicord**, **Endcord**, and **BetterDiscord**, add GitHub or local userplugins into those clients, and update, repair, or remove plugins you already have. It is a normal Windows app — not a developer-terminal workflow.

**Current version: 1.12.1**

## Features

- **Plugins**
  - Install userplugins from one or more GitHub URLs, or from a local folder
  - Multi-plugin install (one URL per line)
  - Name conflicts: overwrite, update, skip, or abort
  - **Manage plugins**: update (git pull), repair (rebuild + inject), uninstall (**plugin only** — client mod stays)
  - Auto plugin name from GitHub URL / folder (editable)
- **Client mods**
  - Install **Vencord**, **Equicord**, **Endcord**, **BetterDiscord**
  - **Custom** client mod from any git URL (Vencord-style: `pnpm build` + `pnpm inject`)
  - Official Windows installer when available, or source build + inject
  - **Repair inject** after Discord updates
- **Runtime**
  - Auto-closes Discord before build/inject and reopens the selected channel after
  - Backs up client settings before rebuild so plugin options are kept when possible
  - Detect screen: install missing **git**, **Node.js**, **pnpm** (winget / corepack / npm)
  - Full install log kept on failure, with **Copy log**, **Copy error**, and **Copy error + log** on every log panel
  - In-app check for a newer release of this installer
  - Works for any Windows user (`%USERPROFILE%` / AppData — no hard-coded usernames)

## Requirements

- Windows 10 or Windows 11, 64-bit
- Discord desktop (Stable, PTB, or Canary)
- Internet for downloads and clones

For **source builds** and **userplugins** the app needs **git**, **Node.js**, and **pnpm**. It looks in PATH and common folders, can install missing tools from Detect, and runs `node.exe` / `git.exe` with full paths so spaces under `Program Files` do not break builds.

Official client-installer mode mainly needs Discord and a network connection.

## Installation

1. Download the latest **Setup** installer from [GitHub Releases](https://github.com/ZIPek3/ZIPs-Discord-Mod-Installer/releases/latest).
2. Run `ZIPs-Discord-Mod-Installer-Setup-*.exe`.
3. If SmartScreen appears: **More info** → confirm the file is from this GitHub project → **Run anyway**.
4. Open **ZIP's Discord Mod Installer**.

You do **not** need to clone this repo or run `build.bat` for normal use. Release EXEs are the supported path.

## Quick start

1. Fully quit Discord (including the tray icon) if you want a clean inject — the app also force-closes Discord when it injects.
2. Open the installer.
3. **Tools & detect**: install any missing tools, then **Rescan**.
4. Choose:
   - **Install client mod** — if you still need Vencord / Equicord / Endcord / BetterDiscord / Custom
   - **Install plugin** — if the client mod is already there
   - **Manage plugins** — update / repair / uninstall plugins already in the work folder
5. For a plugin: paste GitHub URL(s) or pick a local folder that contains `index.ts` (or the plugin entry the client expects).
6. Wait for the log to finish. Use **Copy log** / **Copy error** if something fails.
7. Open Discord → **Settings → Plugins** → enable what you installed.

## Sidebar

| Section | What it does |
|--------|----------------|
| **Install plugin** | Target client + channel → plugin source → build & inject |
| **Manage plugins** | List work-folder userplugins; update / repair / uninstall |
| **Install client mod** | Official or source install of a client mod |
| **Repair inject** | Rebuild + inject an existing source tree after a Discord update |
| **Tools & detect** | Discord installs, detected mods, git / Node / pnpm |
| **Diagnostics** | Quick status and links |

## Discord close and reopen

On plugin install, Manage actions, Repair inject, and source client inject, the app:

1. Force-closes Discord processes (Stable / PTB / Canary and related)
2. Builds / injects
3. Reopens the Discord channel you selected

If inject still fails, quit Discord from the tray yourself and try **Repair inject** again.

## Endcord notes

- Endcord’s inject opens **EndcordInstaller.exe**. When that window appears, click **Install** only.
- **Uninstall** in that window removes the **whole client mod**, not a single plugin.
- Manage → **Uninstall** in *this* app only deletes the plugin under `src/userplugins` and rebuilds. It does **not** uninject Endcord.
- Local Endcord source builds may hit an upstream dual-list check (`betterRoleContext`). The installer soft-patches that check so userplugin builds can finish.

## Userplugins

Plugins go under the client source tree:

```text
%USERPROFILE%\DiscordPluginInstaller\<Client>\src\userplugins\
```

Then the client is rebuilt and injected.

| Manage action | Effect |
|---------------|--------|
| **Update** | `git pull` when the plugin is a git clone, then rebuild + inject (local data files kept) |
| **Repair** | Rebuild + inject without pulling |
| **Uninstall** | Delete the plugin folder only, then rebuild + inject (**client mod stays**) |

## Work folder

```text
%USERPROFILE%\DiscordPluginInstaller
```

Use **Open work folder** in the app to inspect clones and userplugins.

## Troubleshooting

### Windows blocks the EXE

Confirm the download is from this repo’s Releases, then **More info → Run anyway**. Builds are not code-signed yet.

### Tools missing / `node` or `git` not found in the log

Use **Tools & detect → Install**, accept UAC, **Rescan**. The app also runs Node/git via full paths and PATH shims so `Program Files` installs work for most users.

### Inject fails

Let the app close Discord, or quit from the tray, then **Repair inject** or install again. On Endcord, finish **Install** in their installer window (never Uninstall there for a plugin-only change).

### Plugin does not show in Discord

Same channel you injected (Stable / PTB / Canary). **Settings → Plugins** → enable. Restart Discord once.

### Log disappeared after a failure

As of **1.12.1**, Repair inject and tool installs **keep the log** on failure. Use **Copy log** / **Copy error** / **Copy error + log**.

### Lyrics only show the song title on Endcord

Reinstall the Spotify Lyrics plugin so the Endcord compatibility patch can apply, enable it, and test with a track that has lyrics.

### Official installer closed but nothing changed

Complete every step in that installer. If needed, run **Install client mod** again with **Source build**.

### Update banner never appears

Publish a newer GitHub Release with the **Setup** EXE asset on [ZIPek3/ZIPs-Discord-Mod-Installer](https://github.com/ZIPek3/ZIPs-Discord-Mod-Installer).

## Building a release (developers)

```bat
git clone https://github.com/ZIPek3/ZIPs-Discord-Mod-Installer.git
cd ZIPs-Discord-Mod-Installer
build.bat
```

Upload the **Setup** EXE from `dist\` to a new GitHub Release.

## Copyright and license

Copyright © 2026 ZIP. All rights reserved.

ZIP's Discord Mod Installer is proprietary software. You may install and use the application for personal or business use. You may not copy, modify, reverse engineer, sell, sublicense, or redistribute the software without written permission from ZIP. See [LICENSE](LICENSE) for the complete terms.

Third-party components remain the property of their respective owners and are covered by their own license terms. That includes Discord, Vencord, Equicord, Endcord, BetterDiscord, and any plugins or installers this application downloads or launches.

## Version

**1.12.1**

Includes: official + source client installs (Endcord included), userplugin install and multi-plugin, Manage update/repair/uninstall (plugin-only uninstall), Discord force-close and reopen, settings backup, PATH/tool shims for universal Windows installs, Endcord dual-list soft patch, copy log/error on all log panels, logs kept on repair failure, UI layout that fits standard window sizes, and release checks against this repository.
