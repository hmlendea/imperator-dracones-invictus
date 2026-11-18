# Repository overview

## Purpose

Dracones Invictus is a content-only modification for Imperator: Rome 2.0.x that expands the Dacian culture group and renames the Zalmoxian faith to Zamolxian. The repository contains declarative game data and no executable content of any kind.

## Scope

The mod owns exactly three data surfaces:

1. The Dacian culture group declaration, including its unit composition references, levy template reference, male and female name pools, family names, and constituent culture records.
2. Seven English localisation records that revise the display text of the Zalmoxian faith and the Zalmoxis treasure.
3. Mod metadata that the launcher and the game loader read: two descriptor files, a thumbnail, and three release workflows.

Everything else that a player observes — the game engine, the character naming algorithm, the faith mechanics, the treasury item behaviour, the launcher — belongs to the host and is out of scope.

## Key facts

| Fact | Value | Evidence |
|------|-------|----------|
| Target host | Imperator: Rome 2.0.x | `supported_version="2.0.*"` in both descriptors |
| Distribution channels | Steam Workshop, Nexus Mods, GitHub Releases | README installation badges; workflow upload steps |
| Steam application identifier | `859580` | `.github/workflows/steam-workshop-release.yml` |
| Steam Workshop item identifier | `1745923673` | `.github/workflows/steam-workshop-release.yml` |
| Release archive production | Automated | Produced and attached by `.github/workflows/github-release.yml`; the Workshop and Nexus Mods workflows build their own archives |
| Licence | GNU General Public License v3.0 | `LICENSE` |
| Executable content | None | Repository file inventory |
| Runtime network communication | None | Repository file inventory |
| Automated tests | None | Repository file inventory |
| Build step | None | Repository file inventory |

## Delivered content

| Content | Quantity | Location |
|---------|----------|----------|
| Culture group blocks | 1, `dacia_group` | `dracones-invictus/common/cultures/00_dacian.txt` |
| Added male names | 79 | `male_names` block |
| Added female names | 5 | `female_names` block |
| Localisation records | 6 declared keys (7 lines including the language header) | `dracones-invictus/localization/english/zzz_DI_l_english.yml` |
| Descriptors | 2, mutually identical | `dracones-invictus.mod`, `dracones-invictus/descriptor.mod` |
| Binary assets | 1 thumbnail, 500×500 JPEG | `dracones-invictus/thumb.jpg` |
| Workflows | 3 | `.github/workflows/github-release.yml`, `.github/workflows/steam-workshop-release.yml`, `.github/workflows/nexus-mods-release.yml` |

## Entry points

| Entry point | Consumed by | Location |
|-------------|-------------|----------|
| Launcher descriptor | Imperator: Rome launcher and mod loader | `dracones-invictus.mod` |
| In-package descriptor | Imperator: Rome mod loader after mounting | `dracones-invictus/descriptor.mod` |
| Culture group data | Culture loader | `dracones-invictus/common/cultures/00_dacian.txt` |
| Localisation records | Localisation loader | `dracones-invictus/localization/english/zzz_DI_l_english.yml` |
| Release trigger | GitHub Actions | A published GitHub release, or a `v*.*.*` tag push for the Workshop and Nexus Mods uploads |

## Notable characteristics

- The culture override is **file-level**, not record-level. The mod file must therefore remain a complete declaration of the group. This is the single most consequential architectural property of the repository and is analysed in [data-model.md](./data-model.md) and [invariants.md](./invariants.md).
- The localisation override is **record-level** and relies on filename load ordering, which is a weaker and more fragile mechanism than path supersession.
- The repository is intentionally thin. Most of the documentation effort for a repository of this kind lies in recording *why* the content is shaped as it is and *what must never change*, rather than in describing complex mechanics.

## Related documentation

- [repository-structure.md](./repository-structure.md) — the file inventory and naming conventions.
- [architecture.md](./architecture.md) — the architectural elaboration.
- [api-reference/INDEX.md](./api-reference/INDEX.md) — the data contracts in full.
- [../../ARCHITECTURE.md](../ARCHITECTURE.md) — the root architectural document.
