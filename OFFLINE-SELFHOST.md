# Offline Self-Hosting (No Internet Needed on PS5)

Everything in this fork can be run without internet access on the PS5 side.
You only need a local network (your PS5 and a laptop/phone on the same Wi-Fi).

## What "offline" means here

- The Relapse exploit page is hosted from YOUR machine, not from the internet.
- The payloads in `downloads/` are sent over your local network via netcat.
- Once cached, the PS5 browser can reload the exploit page without internet.

## Option A: Serve the Relapse host from your laptop

The upstream Relapse-Exploit repo ships a `serve.py` (archived in `docs/`).
If you clone the full host files:

```sh
# On your laptop (same Wi-Fi as the PS5):
python3 serve.py
# It prints something like: http://192.168.1.50:8000/
```

On the PS5 browser, open that `http://` address instead of the public host URL.
Run the exploit for firmware 13.60 as normal.

## Option B: ESP32 / Raspberry Pi host

Flash a PS5 exploit host image to an ESP32 or Raspberry Pi, join it to your
Wi-Fi, and point the PS5 browser at the device's IP. No internet required
after setup. (Many prebuilt ESP32 PS5 host firmwares exist in the scene.)

## Sending payloads (always local)

Once `elfldr is listening on port 9021`, send each payload from your laptop
over the LAN — no internet involved:

```sh
# Replace PS5_IP with your console's LAN address
nc -w 10 PS5_IP 9021 < downloads/kstuff-lite_v1.11_kstuff.elf
nc -w 10 PS5_IP 9021 < downloads/shadowmountplus_1.7beta3.elf
nc -w 10 PS5_IP 9021 < downloads/LegacyJB_1.2.1.elf
```

Order matters: kstuff-lite first, then ShadowMountPlus, then LegacyJB.
LegacyJB provides the jailbreak service that homebrew apps like
Spectrum Library require.

## Blocking updates (recommended)

On the PS5 network settings, set a DNS that blocks Sony update servers
(commonly `45.56.67.85`), or leave Wi-Fi off entirely when not jailbreaking.
Staying on 13.60 keeps this chain working.
