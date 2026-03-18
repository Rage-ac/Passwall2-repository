# PassWall2 APK Repository

[![Update PassWall2 Packages](https://github.com/Rage-ac/Passwall2/actions/workflows/sync-passwall2.yml/badge.svg)](https://github.com/Rage-ac/Passwall2/actions/workflows/sync-passwall2.yml)
[![pages-build-deployment](https://github.com/Rage-ac/Passwall2/actions/workflows/pages/pages-build-deployment/badge.svg)](https://github.com/Rage-ac/Passwall2/actions/workflows/pages/pages-build-deployment)

Auto-updated and signed APK package repository for **PassWall2** on OpenWrt.

> **Note:** This repository provides **only PassWall2** (version 2). PassWall v1 is not supported.

**Website:** [rage-ac.github.io](https://rage-ac.github.io/)

## How it works

GitHub Actions checks for new upstream releases every 6 hours. When a new version is detected:

1. All APK packages and per-architecture ZIP archives are downloaded
2. Packages are signed with the repository RSA key
3. `APKINDEX.tar.gz` is generated and signed for each architecture
4. APK repository is deployed to GitHub Pages
5. A GitHub Release is created with per-arch ZIP archives

## Technical requirements

| Parameter | Minimum |
|---|---|
| Processor | 700 MHz |
| RAM | 256 MB |
| Firmware | OpenWrt (APK package manager) |

> Routers with 128 MB RAM should use OpenWrt 22.03.3.
>
> WAN and LAN addresses **must** be different.

## Supported protocols

| Protocol | Xray | Sing-Box |
|---|---|---|
| VLESS | ✅ | ✅ |
| VMESS | ✅ | ✅ |
| REALITY | ✅ | ❌ |
| TROJAN | ✅ | ✅ |
| SHADOWSOCKS | ✅ | ✅ |
| WIREGUARD | ✅ | ✅ |
| SOCKS | ✅ | ✅ |
| HTTP | ✅ | ✅ |
| HYSTERIA2 | ❌ | ✅ |
| TUIC | ❌ | ✅ |

## Features

- Single-command installation via `apk`
- Automatic package updates every 6 hours from upstream
- All packages are signed with a repository RSA key
- 16+ supported architectures (ARM, ARM64, MIPS, x86_64)
- Kill switch functionality
- TLS fragmentation support
- WARP connection support

## Installation on Router

### Option 1: APK Repository (recommended)

Add the signing key and repository — packages will be installed and updated via `apk`:

```sh
# Add signing key
wget -O /etc/apk/keys/passwall2-repo.rsa.pub \
  https://rage-ac.github.io/Passwall2/keys/passwall2-repo.rsa.pub

# Add repository (only if not already added)
grep -q 'rage-ac.github.io/Passwall2' /etc/apk/repositories || \
  echo "https://rage-ac.github.io/Passwall2/packages" >> /etc/apk/repositories

# Install
apk update
apk add luci-app-passwall2
```

### Option 2: Manual download

Download ZIP from [Releases](../../releases/latest) for your architecture:

```sh
wget https://github.com/Rage-ac/Passwall2/releases/latest/download/passwall2_signed_apk_aarch64_generic.zip
unzip passwall2_signed_apk_aarch64_generic.zip
apk add --allow-untrusted *.apk
```

## Repository URL

```
https://rage-ac.github.io/Passwall2/packages
```

`apk` automatically appends your device architecture (e.g. `aarch64_cortex-a53`) to the URL.

## Signing key verification

All packages are signed with a repository RSA key. After downloading the key, verify its integrity:

**Key URL:** `https://rage-ac.github.io/Passwall2/keys/passwall2-repo.rsa.pub`

| Algorithm | Hash |
|---|---|
| SHA-256 | `b62fb975e40d489c914c73bcca477ecd8821e7901ad34706c92807e919d2cd89` |
| SHA-1 | `da86ba359996d347d4205b133ad5eb9acc9dfcb1` |
| MD5 | `0a6b509e115ffc2d0ff077ce638fdb53` |

**Verify after download:**

```sh
# Download the key
wget -O /tmp/passwall2-repo.rsa.pub \
  https://rage-ac.github.io/Passwall2/keys/passwall2-repo.rsa.pub

# Check SHA-256
sha256sum /tmp/passwall2-repo.rsa.pub
# Expected: b62fb975e40d489c914c73bcca477ecd8821e7901ad34706c92807e919d2cd89

# If openssl is available on the router:
openssl pkey -pubin -in /tmp/passwall2-repo.rsa.pub -outform DER | sha256sum
# Expected: b62fb975e40d489c914c73bcca477ecd8821e7901ad34706c92807e919d2cd89
```

## Supported architectures

| Architecture | Devices |
|---|---|
| `aarch64_cortex-a53` | Raspberry Pi 3/4, many ARM routers |
| `aarch64_cortex-a72` | Raspberry Pi 4/5 |
| `aarch64_generic` | Generic ARM64 |
| `arm_cortex-a5_vfpv4` | Some ARM routers |
| `arm_cortex-a7` | Many budget ARM routers |
| `arm_cortex-a7_neon-vfpv4` | ARM routers with NEON |
| `arm_cortex-a8_vfpv3` | TI OMAP3 / Beagle series |
| `arm_cortex-a9` | Older ARM routers |
| `arm_cortex-a9_neon` | ARM routers with NEON |
| `arm_cortex-a9_vfpv3-d16` | Marvell Armada 370/XP |
| `arm_cortex-a15_neon-vfpv4` | Marvell Armada 38x |
| `mips_24kc` | MediaTek MT7621 and similar |
| `mips_4kec` | Older Atheros |
| `mips_mips32` | Generic MIPS |
| `mipsel_74kc` | Broadcom BCM47xx |
| `mipsel_mips32` | Generic MIPS little-endian |
| `x86_64` | x86 PCs / virtual machines |

## Upstream

- [Openwrt-Passwall/openwrt-passwall2](https://github.com/Openwrt-Passwall/openwrt-passwall2)

## License

GPLv3
