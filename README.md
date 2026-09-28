# layer-cachyos-extras

CachyOS host-parity workstation toolset for OpenCharly images.

The `cachyos-extras` candy installs the remaining CachyOS host workstation tools
not already covered by the charly dev stack — the CLI, diagnostic, and app gap
from the CachyOS netinstall plus the operator's manual installs, reverse-resolved
to top-level packages. It spans pacman/AUR helpers (`paru`, `pacman-contrib`,
`pkgfile`), monitors (`btop`, `duf`, `glances`, `pv`), editors and apps (`micro`,
`nano`, `vim`, `alacritty`, `meld`, `firefox`), networking/fs/mail tools, and
desktop-integration packages (`accountsservice`, `xdg-*`). Every entry is a real
pacman or AUR package, so each tool is independently verifiable on the built
image. Host-hardware, boot, firmware, and network entries are excluded by design.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `cachyos-extras` |
| Distro | Arch / CachyOS (`pac:` + `aur:`) |
| Highlights | `paru`, `btop`, `duf`, `glances`, `micro`, `vim`, `alacritty`, `meld`, `firefox`, `syncthing`, `cloudflared-bin`, `gvisor-tap-vsock` |
| Requires | `layer-yay` (AUR helper) |
| Service / port | none |

## How to use it

Compose the layer by pinning this repo in a CachyOS box's `candy:` list:

```yaml
my-cachyos-box:
  candy:
    base: cachyos
    candy:
      - '@github.com/opencharly/layer-cachyos-extras:v2026.239.1610'
```

Then, inside the built image:

```bash
btop --version
paru --version
command -v cloudflared
```

## Layout

- `charly.yml` — the candy manifest: the `pac:` and `aur:` package lists, an
  ordered `plan:` of build-time `check:` steps, and the embedded `skill:` entity
  (when present).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: none yet — routed to the skill-authoring batch
  [`opencharly/opencharly#291`](https://github.com/opencharly/opencharly/issues/291);
  meanwhile see `/charly-distros:cachyos` for the CachyOS base
- Requires: `/charly-tools:yay`
- Consumed by: `distro-cachyos` (the operator workstation profile)
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
