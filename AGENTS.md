# AGENTS.md — layer-cachyos-extras

Standalone candy repo for the `cachyos-extras` layer. The candy lives in
`charly.yml` at the repo root: the Arch/CachyOS `pac:` and `aur:` package lists,
an ordered `plan:` of build-time `check:` steps, and the embedded `skill:` entity
(when present). There is no source tree and no service of its own.

Canonical files:

- `charly.yml` — the `cachyos-extras:` candy entity (and the
  `cachyos-extras-skill:` skill entity, when present).
- `.github/workflows/deploy.yml` — the manifest gate.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-distros:cachyos` — the closest owning skill: the CachyOS base image
  this candy extends. Load before editing or troubleshooting the candy.
- `/charly-tools:yay` — the AUR helper the `aur:` section depends on. Load when
  touching AUR packages.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, package sections, `aur:`). Load before
  editing any entity field or plan step.

There is no dedicated `/charly-*:cachyos-extras` owning skill yet — this repo's
candy carries no `skill:` entity. When one is authored, add it here.

## Build / validate / test

- `charly box validate` at the repo root — the same structural gate CI runs: the
  manifest must parse and validate at the pinned charly. The CI pin lives in
  `.github/workflows/deploy.yml`; keep the `version:` schema stamp within the
  pinned charly's supported range (do not migrate the stamp past the pin).
- `.github/workflows/deploy.yml` — builds the pinned charly from a CI-time
  checkout and runs `charly box validate`. This is the merge gate.
- There is no live bed: the candy is package-only, so the evidence is its
  `plan:` `check:` steps, which assert representative binaries (`btop`, `duf`,
  `paru`, `cloudflared`, `syncthing`) exist and that `accountsservice` is
  installed.

## Modify this repo

- Edit the `cachyos-extras:` candy entity (and its `skill:` entity together, when
  one exists). The skill is the projected usage source, so a package or behaviour
  change that is not mirrored in the skill leaves the corpus stale.
- Add packages under `distro: arch: package:` (pacman) or `distro: arch: aur:
  package:` (AUR). Exclude host-hardware, boot, firmware, and network entries by
  design. New behaviour claims go in `plan:` as observable `check:` steps.
- Keep `version:` at the schema stamp the pinned CI charly supports.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
