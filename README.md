# pwnagotchi-build

Personal build documentation and configuration reference for my Pwnagotchi running on a Raspberry Pi Zero 2 W with a Waveshare 2.13" e-ink display.

---

## Overview

This repo tracks my Pwnagotchi setup: hardware selection, flashing, configuration, plugin stack, troubleshooting history, and the 3D-printed case. It is not a fork of the Pwnagotchi firmware — it is a living reference for my specific build.

---

## Hardware Used

| Component | Details |
|-----------|---------|
| SBC | Raspberry Pi Zero 2 W |
| Display | Waveshare 2.13" e-ink HAT (v2) |
| Storage | 32 GB microSD (Class 10 / A1) |
| Power | Anker PowerCore 5000 (USB-A) |
| Case | Custom 3D-printed — see [3D Case](#3d-case) |

---

## Setup Instructions

Follow the docs in order:

1. [Flashing & First Boot](docs/01-flashing-and-first-boot.md)
2. [Configuration](docs/02-configuration.md)
3. [Plugins](docs/03-plugins.md)
4. [Troubleshooting Log](docs/04-troubleshooting-log.md)
5. [3D Case](docs/05-3d-case.md)

---

## Plugins Enabled

| Plugin | Purpose |
|--------|---------|
| `auto-update` | Keeps Pwnagotchi firmware up to date automatically |
| `auto_backup` | Backs up config and brain to `/root/backup/` |
| `bt-tether` | USB/Bluetooth tethering for SSH and internet sharing |
| `fix_services` | Restarts crashed services (bettercap, etc.) |
| `grid` | Registers unit on the global Pwnagotchi grid |
| `wpa-sec` | Uploads cracked handshakes to wpa-sec.stanev.org |
| `session-stats` | Tracks per-session capture stats on the display |

---

## Troubleshooting Highlights

See the full log in [docs/04-troubleshooting-log.md](docs/04-troubleshooting-log.md).

Notable issues resolved:
- Display not rendering — wrong `ui.display.type` value
- bettercap crashing on boot — fixed via `fix_services` plugin
- Grid not connecting — required manual `grid.report.url` entry

---

## 3D Case

Custom case printed in PLA. See [docs/05-3d-case.md](docs/05-3d-case.md) for STL source, print settings, and designer credit.

---

## License

This repo contains my personal configuration and documentation only — no firmware code is included. Released under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
