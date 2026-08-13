# Oh My Flunky — Local Daemon

**English** · [한국어](./README.md)

To use Oh My Flunky, a **local daemon** must be running on your PC.
The daemon keeps your agents, skills, workflows, knowledge, tools, run history, and
**AI model keys** **entirely on your own machine**, and runs AI execution inside this
process. No domain data and no API key ever leaves over the network.

> The central server only handles **Google sign-in (identity)**. Everything else is handled by the local daemon.

---

## 1. Install

### Option A — npm (recommended, macOS & Windows)

If you have **Node.js ≥ 16**, one line installs it. The matching binary for your OS is
downloaded automatically from GitHub Releases, and it registers auto-start for you.

```bash
npm install -g oh-my-flunky-daemon
oh-my-flunky-daemon --install    # register the auto-start service
```

- macOS: registered with `launchd` (log: `/tmp/oh-my-flunky-daemon.log`)
- Windows: registered with Task Scheduler (starts on login)
- On install the executable is **copied** into the app-support folder, so the daemon
  keeps working even if you later remove the npm package.

**Update:**

```bash
npm update -g oh-my-flunky-daemon
oh-my-flunky-daemon --install    # apply the new binary to the service
```

### Option B — Direct download

Grab the file for your OS from the latest release.

| OS | File |
|----|------|
| **macOS** | [`oh-my-flunky-daemon-macos`](https://github.com/suha-lee/oh-my-flunky-daemon/releases/latest/download/oh-my-flunky-daemon-macos) |
| **Windows** | [`oh-my-flunky-daemon.exe`](https://github.com/suha-lee/oh-my-flunky-daemon/releases/latest/download/oh-my-flunky-daemon.exe) |

Or download directly from the [releases page](https://github.com/suha-lee/oh-my-flunky-daemon/releases/latest).

**You only need to run it once.** The daemon registers auto-start by itself and moves to
the background. From then on it **starts automatically on every reboot / login.**

#### macOS

```bash
# 1) Make the downloaded file executable
chmod +x ~/Downloads/oh-my-flunky-daemon-macos

# 2) Remove the quarantine flag (prevents the "unidentified developer" warning)
xattr -d com.apple.quarantine ~/Downloads/oh-my-flunky-daemon-macos 2>/dev/null

# 3) Run — first run registers auto-start (LaunchAgent) + goes to background
~/Downloads/oh-my-flunky-daemon-macos
```

- It moves to the background immediately and **starts automatically on every login** afterward.
- It restarts itself if it dies (`KeepAlive`).

> You can also double-click it in Finder. If an "unidentified developer" warning appears,
> **right-click → Open** once to allow it.

#### Windows

1. **Double-click** `oh-my-flunky-daemon.exe`.
2. On first run it registers auto-start (Task Scheduler) and runs in the background.
   - No window appears (background execution).
   - It **starts automatically on every login** afterward.

> If SmartScreen warns you, click **More info → Run anyway**.

---

## 2. Verify it's running

When the daemon is up, Oh My Flunky shows a green **"Daemon connected"** indicator at the
bottom-left of the screen. It reconnects automatically on refresh, and if you start the
daemon late it attaches within a few seconds.

The daemon runs at the local address `http://127.0.0.1:18799`.

---

## 3. Command-line options

| Command | Description |
|---------|-------------|
| _(no option)_ | **First run**: register auto-start service + run in background (recommended) |
| `--install` | Register the auto-start service only |
| `--uninstall` | Remove the auto-start service |
| `--foreground` | Run in the **foreground** without a service (to watch logs in the terminal) |
| `--no-service` | Run in the background **for this session only**, without registering a service |
| `--version` | Print version / banner |

Examples:

```bash
# Remove auto-start (uninstall)
./oh-my-flunky-daemon-macos --uninstall     # macOS
oh-my-flunky-daemon.exe --uninstall         # Windows
```

> Installed via npm? Just use the `oh-my-flunky-daemon` command instead of the binary path:
> `oh-my-flunky-daemon --uninstall`.

---

## 4. Data location

Everything is stored locally.

| OS | Path |
|----|------|
| **macOS** | `~/Library/Application Support/oh-my-flunky/flunky_local.db` |
| **Windows** | `%APPDATA%\oh-my-flunky\flunky_local.db` |
| **Linux** | `~/.local/share/oh-my-flunky/flunky_local.db` |

The installed executable is copied to `<app-support>/oh-my-flunky/bin/`, so you can delete
the originally downloaded file after installing.

---

## 5. Logs & troubleshooting

- **Log location (macOS)**: `/tmp/oh-my-flunky-daemon.log`
- **"Daemon not running" keeps showing**
  - Confirm the daemon is running, then hard-refresh the app (`Cmd/Ctrl+Shift+R`).
  - If it still fails, run the daemon again and make sure the port (`18799`) is not taken by another program.
- **macOS security warning**: allow it with the `xattr` command above, or **right-click → Open**.
- **Full removal**: run `--uninstall` to remove auto-start, then delete the downloaded executable.

---

## Automatic updates

The app always points you at the daemon from the **latest** release.
- **npm users**: `npm update -g oh-my-flunky-daemon && oh-my-flunky-daemon --install`.
- **Direct download users**: re-download the latest file from the links above and run it once.
