# PassWall2 APK Repository

Auto-updated and signed APK package repository for [PassWall2](https://github.com/Openwrt-Passwall/openwrt-passwall2) on OpenWrt.

## How it works

GitHub Actions checks for new upstream releases every 6 hours. When a new version is detected:

1. All APK packages and per-architecture ZIP archives are downloaded
2. Packages are signed with the repository RSA key
3. `APKINDEX.tar.gz` is generated and signed for each architecture
4. APK repository is deployed to GitHub Pages
5. A GitHub Release is created with per-arch ZIP archives

## Installation on Router

### Option 1: APK Repository (recommended)

Add the signing key and repository — packages will be installed and updated via `apk`:

```sh
# Add signing key
wget -O /etc/apk/keys/passwall2-repo.rsa.pub \
  https://rage-ac.github.io/Passwall2/keys/passwall2-repo.rsa.pub

# Add repository (replace <arch> with your architecture, e.g. aarch64_generic)
echo "https://rage-ac.github.io/Passwall2/packages/<arch>" >> /etc/apk/repositories

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
https://rage-ac.github.io/Passwall2/packages/<arch>
```

## Supported architectures

| Architecture | Devices |
|---|---|
| `aarch64_cortex-a53` | Raspberry Pi 3/4, many ARM routers |
| `aarch64_cortex-a72` | Raspberry Pi 4/5 |
| `aarch64_generic` | Generic ARM64 |
| `arm_cortex-a7` | Many budget ARM routers |
| `arm_cortex-a9` | Older ARM routers |
| `mips_24kc` | MediaTek MT7621 and similar |
| `mipsel_74kc` | Broadcom BCM47xx |
| `x86_64` | x86 PCs / virtual machines |

## Upstream

- [Openwrt-Passwall/openwrt-passwall2](https://github.com/Openwrt-Passwall/openwrt-passwall2)
