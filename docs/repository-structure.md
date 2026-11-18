# Repository structure

## Complete file inventory

```text
imperator-dracones-invictus/
├── .github/
│   ├── FUNDING.yml
│   └── workflows/
│       ├── github-release.yml
│       ├── nexus-mods-release.yml
│       └── steam-workshop-release.yml
├── dracones-invictus/
│   ├── common/
│   │   └── cultures/
│   │       └── 00_dacian.txt
│   ├── localization/
│   │   └── english/
│   │       └── zzz_DI_l_english.yml
│   ├── descriptor.mod
│   └── thumb.jpg
├── docs/                              # this documentation set
├── ARCHITECTURE.md
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
├── LICENSE
├── PRIVACY.md
├── README.md
├── SECURITY.md
└── dracones-invictus.mod
```

The repository contains no build output, no dependency manifest, no lockfile, no compilation unit, no test project, and no script.

## Tree semantics

### `dracones-invictus/` — the mountable mod package

This directory is the unit that the game mounts. Its internal layout mirrors the vanilla game layout exactly, so that the loader resolves the mod paths in place of the vanilla paths.

| Path | Mirrors | Purpose |
|------|---------|---------|
| `common/cultures/` | `game/common/cultures/` | Culture group data |
| `localization/english/` | `game/localization/english/` | English text records |
| `descriptor.mod` | — | In-package metadata that the loader reads after mounting |
| `thumb.jpg` | — | Thumbnail consumed by the launcher and distribution channels |

A change to this internal layout is a change to the mounting contract. Directory names are not free choices.

### Repository root — the delivery surface

| Path | Purpose |
|------|---------|
| `dracones-invictus.mod` | Launcher-visible descriptor. Read by the launcher before mounting, so it must lie outside the mounted directory. |
| `.github/workflows/github-release.yml` | Release automation. Builds and attaches the versioned archive to the GitHub release. |
| `.github/workflows/steam-workshop-release.yml` | Release automation. Builds the archive and uploads it to the Steam Workshop. |
| `.github/workflows/nexus-mods-release.yml` | Release automation. Builds the archive and uploads it to Nexus Mods. |
| `.github/FUNDING.yml` | Funding platform configuration, consumed by GitHub rather than by the game. |
| Root policy documents | Repository governance, not game content. |

The two descriptors are intentionally unrelated to each other by reference. They duplicate values instead of one including the other, because the host has no include mechanism. This duplication is a deliberate, accepted maintenance cost, documented in [invariants.md](./invariants.md).

## Naming conventions

| Convention | Rule | Rationale |
|------------|------|-----------|
| Culture file prefix | `00_` | Matches the vanilla culture file naming and keeps the file at the head of the culture file ordering |
| Localisation file prefix | `zzz_DI_` | Places the file late in the localisation load order, so its records win the last-wins resolution |
| Documentation files | kebab-case | House convention for `docs/` |
| Documentation directories | kebab-case, plural for collections | House convention for `docs/` |
| Descriptor filename | `<mod-directory-name>.mod` at the root | Launcher convention |
| Release asset | `dracones-invictus_<version>.zip` | Produced by the GitHub Release workflow and attached to the GitHub release |
| Tag | `v<major>.<minor>.<patch>` | Matches the workflow trigger pattern |

The two prefixes are load-order controls, not cosmetic identifiers. Renaming either file alters resolution precedence, as recorded in [configuration.md](./configuration.md).

## Where to make a change

| Change | Files to modify | Consult |
|--------|-----------------|---------|
| Append male or female names | `dracones-invictus/common/cultures/00_dacian.txt` | [operations/change-guide.md](./operations/change-guide.md#adding-or-revising-names) |
| Append family names | The same file, `family` block | [operations/change-guide.md](./operations/change-guide.md#adding-or-revising-names) |
| Append a culture record | The same file, `culture` block | [operations/change-guide.md](./operations/change-guide.md#adding-or-revising-culture-records) |
| Revise display text | `dracones-invictus/localization/english/zzz_DI_l_english.yml` | [operations/change-guide.md](./operations/change-guide.md#revising-localisation-text) |
| Change the targeted game version | Both descriptors | [configuration.md](./configuration.md) |
| Replace the thumbnail | `dracones-invictus/thumb.jpg` | [operations/change-guide.md](./operations/change-guide.md#replacing-the-thumbnail) |
| Change the release pipeline | `.github/workflows/github-release.yml`, `.github/workflows/steam-workshop-release.yml`, `.github/workflows/nexus-mods-release.yml` | [build-and-deployment.md](./build-and-deployment.md) |

## Related documentation

- [repository-overview.md](./repository-overview.md) — purpose, scope, and key facts.
- [data-model.md](./data-model.md) — the entities behind these files.
- [../../ARCHITECTURE.md](../ARCHITECTURE.md) — the root source map.
