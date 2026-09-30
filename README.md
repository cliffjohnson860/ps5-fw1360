# PS5 FW 13.60 Relapse Payload Setup

Working notes and payload set for a PS5 on firmware 13.60.

This setup answers the common stopping point:

> `elfldr is listening on port 9021`

That message means the browser exploit and ELF loader already ran. The next step is to send the working ELF payloads to the PS5 loader on port `9021`.

## How To Use

1. Open this Relapse host on the PS5 browser:

```text
https://soniciso1.github.io/relapse/
```

2. Run the exploit for firmware `13.60`.
3. Enable `ftpsrv` and `klosrv` from the host options.
4. Wait until the page says `elfldr is listening on port 9021`.
5. If the page stops there, send `kstuff-lite` from this folder:

```sh
nc -w 10 PS5_IP 9021 < downloads/kstuff-lite_v1.11_kstuff.elf
```

6. Then send `ShadowMountPlus`:

```sh
nc -w 10 PS5_IP 9021 < downloads/shadowmountplus_1.7beta2.elf
```

For the audited console, `PS5_IP` was:

```sh
192.168.11.86
```

## Working Chain

1. Relapse exploit
2. elfldr on port `9021`
3. kstuff-lite v1.11
4. ShadowMountPlus 1.7beta2

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
