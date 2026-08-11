# 04 — Troubleshooting Log

Running log of issues encountered during this build, their root causes, and how they were fixed.

---

## Log

| Date | Symptom | Root Cause | Fix | journalctl Snippet |
|------|---------|------------|-----|--------------------|
| — | — | — | — | — |

---

## Template — How to Add an Entry

When you hit an issue, add a row to the table above using this format:

| Field | Notes |
|-------|-------|
| **Date** | YYYY-MM-DD |
| **Symptom** | What you observed — display frozen, service crashed, SSH unreachable, etc. |
| **Root Cause** | What actually caused it — misconfigured value, incompatible driver, OOM, etc. |
| **Fix** | Exact steps taken to resolve |
| **journalctl Snippet** | Paste the key log lines (keep short — omit timestamps if noisy) |

---

## Useful Diagnostic Commands

```bash
# Live pwnagotchi logs
sudo journalctl -u pwnagotchi -f

# Last 100 lines of pwnagotchi log
sudo journalctl -u pwnagotchi -n 100 --no-pager

# Check bettercap service status
sudo systemctl status bettercap

# Check display driver
sudo journalctl -u pwnagotchi | grep -i "display\|waveshare\|epaper"

# Check plugin load errors
sudo journalctl -u pwnagotchi | grep -i "plugin\|error\|exception"

# System resource usage (helpful for OOM debugging)
free -h
top -b -n 1 | head -20

# Check all services
sudo systemctl list-units --failed
```

---

## Common Issues Reference

### Display shows nothing / stays white

- Verify `ui.display.type = "waveshare_2"` (not `waveshare2in13` or other variants)
- Confirm HAT is fully seated on GPIO header
- Check for HAT driver errors: `sudo journalctl -u pwnagotchi | grep waveshare`

### bettercap crashes repeatedly

- Enable `fix_services` plugin
- Check available RAM: `free -h` — Pi Zero 2 W has 512 MB; bettercap is hungry
- Look for `signal: killed` in logs — indicates OOM kill

### SSH via USB not working

- Ensure you're using the **USB** port, not the **PWR IN** port
- Confirm `usb0` interface exists: `ip link show`
- On macOS: check System Preferences → Network for RNDIS/USB device

### Grid not connecting

- Requires internet — verify `bt-tether` is working first
- Manually set `grid.report.url` if the default endpoint is unreachable
- Check DNS resolution: `nslookup api.pwnagotchi.ai`

### wpa-sec uploads failing

- Verify `api_key` is set correctly in config.toml
- Test connectivity: `curl -I https://wpa-sec.stanev.org`
- Check plugin log output: `sudo journalctl -u pwnagotchi | grep wpa-sec`
