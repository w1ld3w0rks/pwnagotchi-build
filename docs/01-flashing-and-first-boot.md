# 01 — Flashing & First Boot

## Requirements

- Raspberry Pi Zero 2 W
- microSD card (32 GB recommended, Class 10 / A1 or better)
- Waveshare 2.13" e-ink HAT (v2)
- USB data cable (not charge-only)
- macOS or Linux host machine

---

## Download the Image

1. Go to the [Pwnagotchi releases page](https://github.com/evilsocket/pwnagotchi/releases).
2. Download the latest `.img.zip` for Raspberry Pi.
3. Verify the SHA256 checksum before flashing.

```bash
shasum -a 256 pwnagotchi-raspi-*.img.zip
```

---

## Flash the Image

Use [Raspberry Pi Imager](https://www.raspberrypi.com/software/) or `dd`.

### With Raspberry Pi Imager

1. Open Imager → Choose OS → Use Custom → select the `.img` file.
2. Choose your microSD card as the target.
3. Click Write.

### With dd (macOS)

```bash
# Identify your SD card device
diskutil list

# Unmount (replace diskN with your disk number)
diskutil unmountDisk /dev/diskN

# Flash
sudo dd if=pwnagotchi.img of=/dev/rdiskN bs=1m status=progress

# Eject
diskutil eject /dev/diskN
```

---

## Pre-Boot Configuration

Before first boot, mount the SD card and place your `config.toml` in the boot partition:

```
/Volumes/boot/config.toml
```

At minimum, set `main.name` and `ui.display.type = "waveshare_2"` before booting.

---

## First Boot

1. Seat the Waveshare HAT on the Pi Zero 2 W GPIO header.
2. Insert the flashed microSD.
3. Connect USB cable to the **USB port** (not PWR IN) of the Pi.
4. First boot takes 3–5 minutes — the e-ink display will initialize and show the face.

> **Note:** The display may flicker several times during initialization. This is normal.

---

## SSH Access (USB Tethering)

After boot, the Pi appears as a USB Ethernet device:

```bash
ssh pi@10.0.0.2
# Default password: raspberry — change immediately
```

```bash
passwd
```

---

## Verify Display is Working

```bash
sudo pwnagotchi --debug
```

Watch for errors related to `waveshare_2` driver initialization in the output.
