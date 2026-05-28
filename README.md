# gamerx-repo

Distribution point for the `[gamerx-core]` and `[gamerx-testing]` pacman repos.

Built `.pkg.tar.zst` artifacts and signed db files live in this repo's GitHub
Releases. Pacman clients fetch them over plain HTTPS.

## Adding the repo manually

> **Note**: GamerX OS users get this pre-configured. These instructions are for
> trying GamerX packages on existing Arch installs.

1. Install the keyring:
   ```bash
   sudo pacman-key --recv-keys <KEY-ID-TBD>
   sudo pacman-key --lsign-key <KEY-ID-TBD>
   ```
2. Add to `/etc/pacman.conf`:
   ```ini
   [gamerx-core]
   SigLevel = Required DatabaseOptional
   Server = https://github.com/GamerXECO-sys55/gamerx-repo/releases/download/core-x86_64/
   ```
3. `sudo pacman -Syu`

## Repo channels

| Channel | Tag prefix | Stability |
|---|---|---|
| `gamerx-core` | `core-*` | Stable. Default channel. Rolling releases triggered by main branch updates in `gamerx-packages`. |
| `gamerx-testing` | `testing-*` | Nightly. Built from dev branches. |

## How packages get here

```
gamerx-packages (sources)  ─────CI on tag────▶  gamerx-repo (release)
                                  │
                                  ├─ build PKGBUILDs in clean Arch container
                                  ├─ namcap lint
                                  ├─ sign each .pkg.tar.zst
                                  ├─ build/sign db
                                  └─ upload to GitHub release `core-x86_64`
```

## Hosting plan

- v1.0: GitHub Releases (free, no infra).
- v1.1+: Migrate to Cloudflare R2 + custom domain if scale demands.

## Status

🚧 **Phase 5 — not started.** GPG keypair and CI pipeline not yet generated.

## See also

- [Meta repo](https://github.com/GamerXECO-sys55/gamerx-os)
- [gamerx-packages](https://github.com/GamerXECO-sys55/gamerx-packages) — sources

## License

GPL-3.0 (for tooling); individual packages keep their own licenses.
