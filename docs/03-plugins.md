# 03 — Plugins

## Plugin System Overview

Plugins are Python modules loaded at startup from:

```
/usr/local/share/pwnagotchi/plugins/default/   # built-in
/usr/local/share/pwnagotchi/custom-plugins/    # custom/third-party
```

Enable or disable plugins in `config.toml` under `[main.plugins.<name>]`.

---

## Enabled Plugins

### auto-update

Checks for and applies Pwnagotchi firmware updates automatically on boot.

```toml
[main.plugins.auto-update]
enabled = true
```

> Requires internet access. Pair with `bt-tether` or connect via USB tethering with internet sharing enabled on the host.

---

### auto_backup

Backs up config and AI brain after each session. Saves you from losing trained behavior after a flash.

```toml
[main.plugins.auto_backup]
enabled = true
backup_path = "/root/backup"
```

Retrieve backups:

```bash
scp pi@10.0.0.2:/root/backup/* ./backups/
```

---

### bt-tether

Provides SSH access and internet sharing over Bluetooth or USB. Essential for headless management.

```toml
[main.plugins.bt-tether]
enabled = true
# phone-mac = "AA:BB:CC:DD:EE:FF"
# ip = "192.168.44.44"
```

Pair your phone first:

```bash
bluetoothctl
  power on
  agent on
  scan on
  pair AA:BB:CC:DD:EE:FF
  trust AA:BB:CC:DD:EE:FF
```

---

### fix_services

Monitors bettercap and other critical daemons. Restarts them automatically if they crash.

```toml
[main.plugins.fix_services]
enabled = true
```

Helpful during long unattended sessions where bettercap may OOM-crash.

---

### grid

Registers your unit on the global Pwnagotchi grid map at pwnagotchi.ai.

```toml
[main.plugins.grid]
enabled = true
```

Requires internet access. Your unit's name, location approximation, and stats are reported.

---

### wpa-sec

Uploads captured WPA handshake hashes to [wpa-sec.stanev.org](https://wpa-sec.stanev.org) for distributed cracking.

```toml
[main.plugins.wpa-sec]
enabled = true
# api_key = "YOUR_API_KEY"
```

1. Create a free account at wpa-sec.stanev.org.
2. Copy your API key into `config.toml`.
3. Captured `.pcap` handshakes are automatically uploaded after each session.

---

### session-stats

Renders per-session capture statistics directly on the e-ink display.

```toml
[main.plugins.session-stats]
enabled = true
```

Displays: APs seen, handshakes captured, peers encountered, session uptime.

---

## Installing Custom Plugins

```bash
# Copy plugin to the custom plugins directory
scp my_plugin.py pi@10.0.0.2:/usr/local/share/pwnagotchi/custom-plugins/

# Enable in config.toml
[main.plugins.my_plugin]
enabled = true
```

Set the custom plugins path in config if not already set:

```toml
[main]
custom_plugins = "/usr/local/share/pwnagotchi/custom-plugins/"
```

---

## Troubleshooting Plugins

Check which plugins loaded successfully:

```bash
sudo journalctl -u pwnagotchi | grep -i plugin
```

A plugin that fails to import will log an error but won't crash the daemon.
