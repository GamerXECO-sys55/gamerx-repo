# gamerx-repo

🟢 **LIVE** — distribution point for the `[gamerx-core]` and `[gamerx-testing]`
pacman channels.

Built `.pkg.tar.zst` artifacts and signed db files live in this repo's GitHub
Releases. Pacman clients fetch them over plain HTTPS.

## What's currently in [gamerx-core]

| Package | Version |
|---|---|
| `gamerx-branding` | `20260528-1` |
| `gamerx-grub-theme` | `0.1.0-1` |
| `gamerx-keyring` | `20260528-1` |
| `gamerx-meta` | `20260528-1` |
| `gamerx-meta-aria` | `20260528-1` |
| `gamerx-mirrorlist` | `20260528-1` |
| `gamerx-plymouth-theme` | `0.1.0-1` |
| `gamerx-sddm-theme` | `0.1.0-1` |
| `gamerx-shell` | `0.1.0-1` |
| `gamerx-theme` | `0.1.0-1` |
| `gamerx-tweaks` | `20260528-1` |

## Adding the repo manually

> **Note**: GamerX OS users get this pre-configured. These instructions are
> for trying GamerX packages on existing Arch installs.

### 1. Trust the GamerX OS signing key

```bash
# Fetch and install our keyring package
curl -L -O https://github.com/GamerXECO-sys55/gamerx-repo/releases/download/gamerx-core-x86_64/gamerx-keyring-20260528-1-any.pkg.tar.zst
sudo pacman -U gamerx-keyring-*.pkg.tar.zst
```

The keyring's `post_install` scriptlet imports and locally signs the master key
(fingerprint `3C7C0F0E E2EC008D 417FA2D6 4354EA4A AA4736D3`).

### 2. Add the repo to /etc/pacman.conf

```ini
[gamerx-core]
SigLevel = Required DatabaseOptional
Server = https://github.com/GamerXECO-sys55/gamerx-repo/releases/download/$repo-$arch
```

### 3. Sync and install

```bash
sudo pacman -Sy
sudo pacman -S gamerx-meta          # vanilla install set
# or
sudo pacman -S gamerx-meta-aria     # Aria edition
```

## Channels

| Channel | Tag | Stability |
|---|---|---|
| `gamerx-core` | `gamerx-core-x86_64` | Stable. Default channel for end users. |
| `gamerx-testing` | `gamerx-testing-x86_64` | Nightly. Built from dev branches. (not yet enabled) |

## How packages get here

```
gamerx-packages (sources)  ───CI on tag───▶  gamerx-repo (release)
                                  │
                                  ├─ build PKGBUILDs in clean Arch container
                                  ├─ namcap lint
                                  ├─ sign each .pkg.tar.zst
                                  ├─ build/sign db with `repo-add --sign`
                                  └─ upload to GitHub Release
```

Currently published **manually** via `scripts/sign_and_publish.sh` in the meta
project. P9 lands the GitHub Actions automation.

## Hosting plan

- **v1.0**: GitHub Releases (this) — free, no infra.
- **v1.1+**: Migrate to Cloudflare R2 + custom domain if scale demands.

## Status

- ✅ Phase 5 — repo signing pipeline live, 11 packages published.
- 🚧 Phase 9 — CI automation pending.

## License

GPL-3.0 (for tooling); individual packages keep their own licenses.
