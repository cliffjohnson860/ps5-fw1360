# PS5 FW 13.60 Payload Audit

Private audit notes and payload set for a PS5 on firmware 13.60.

This setup answers the common stopping point:

> `elfldr is listening on port 9021`

That message means the browser exploit and ELF loader already ran. The next step is to send the working ELF payloads to the PS5 loader on port `9021`.

## Working Chain

1. Run Relapse exploit on the PS5.
2. Wait until the page says `elfldr is listening on port 9021`.
3. From this folder, send `kstuff-lite`:

```sh
nc -w 10 PS5_IP 9021 < downloads/kstuff-lite_v1.11_kstuff.elf
```

4. Send `ShadowMountPlus`:

```sh
nc -w 10 PS5_IP 9021 < downloads/shadowmountplus_1.7beta2.elf
```

For the audited console, `PS5_IP` was:

```sh
192.168.11.86
```

## Verified Runtime Evidence

`kstuff-lite v1.11`:

- `patching app.db`
- `Patching shellui instance`
- `Successfully patched SceShellUI trophy IsServerAvailable`

`ShadowMountPlus 1.7beta2`:

- `FW: 13.60`
- `runtime control ready`
- `hooks installed`
- `Library synchronized`
- `HTTP/JSON ready: http://127.0.0.1:10101/api/v1`

Note: `127.0.0.1:10101` is PS5 loopback only. Refused connection from a Mac/PC is expected.

## Do Not Use For This Setup

- `kstuff_v1.6.7.elf`
- `ShadowMountPlus_1.7beta1.elf`
- `OnionHEN v0.0.13`
- `etaHEN 2.5B` unless FW 13.60 support is verified separately

## Files

- `PS5-13.60-working-setup.txt` - full audit result
- `SHA256SUMS.txt` - payload hashes
- `downloads/` - local payload files
- `docs/` - upstream documentation snapshots used for the audit
- `before-manual-action.txt` / `after-manual-action.txt` - read-only network snapshots

## Credits

Audit / setup documentation:

- https://github.com/orchivillando

Upstream projects and authors:

- Relapse Exploit: https://github.com/ntfargo/Relapse-Exploit
- ps5-payload-elfldr: https://github.com/ps5-payload-dev/elfldr
- kstuff-lite: https://github.com/EchoStretch/kstuff-lite
- ShadowMountPlus: https://github.com/drakmor/ShadowMountPlus
- etaHEN reference docs: https://github.com/etaHEN/etaHEN

Use only on hardware you own or are authorized to test. This repository is for private audit, recovery, and research documentation.
