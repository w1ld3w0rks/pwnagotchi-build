# 02 — Configuration

## Config File Location

The primary config lives at:

```
/etc/pwnagotchi/config.toml
```

The default config (do not edit directly) lives at:

```
/etc/pwnagotchi/default.toml
```

Your `config.toml` overrides defaults — you only need to specify values you want to change.

---

## Deploying the Config

Copy your local config to the device over SSH:

```bash
scp config/config.toml pi@10.0.0.2:/etc/pwnagotchi/config.toml
```

Then restart the service:

```bash
ssh pi@10.0.0.2 "sudo systemctl restart pwnagotchi"
```

---

## Key Settings

### Identity

```toml
[main]
name = "your-name-here"
```

### Whitelist

Networks in the whitelist are never touched. Add your home network and any hotspots you own:

```toml
[main]
whitelist = [
    "HomeSSID",
    "MyPhoneHotspot",
]
```

### Display

```toml
[ui.display]
type = "waveshare_2"
rotation = 180
enabled = true
```

`waveshare_2` is the correct value for the Waveshare 2.13" e-ink HAT v2. Do not use `waveshare2in13` or other variants — they map to different driver code.

---

## Personality Tuning

The personality section controls how aggressively pwnagotchi hunts. Defaults work well for most setups.

```toml
[personality]
min_rssi = -200        # Include APs regardless of signal strength
```

Reduce `min_rssi` to a less negative value (e.g., `-80`) to only target nearby APs.

---

## Applying Changes

Any change to `config.toml` requires a service restart:

```bash
sudo systemctl restart pwnagotchi
```

To watch logs in real time after restart:

```bash
sudo journalctl -u pwnagotchi -f
```
