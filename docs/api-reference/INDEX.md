# API Reference

> **Note**: This project is a **data-only Imperator: Rome mod** with no public API, no HTTP endpoints, no library interface, and no programmatic extension points. This document serves as a reference for the *data contracts* that constitute the mod's integration surface with the game engine.

Every contract documented here was verified against the mod source in this repository and, where the host defines the other side of the contract, against an installed Imperator: Rome 2.0.x data set at `game/common/cultures/00_dacian.txt`, `game/common/levy_templates/01_dacians.txt`, and `game/localization/english/*.yml`.

## Contract summary

| Contract | Provider | Consumer | Defined by | Boundary |
|----------|----------|----------|------------|----------|
| Culture group block | The mod | Imperator: Rome culture loader | Host schema | `dracones-invictus/common/cultures/00_dacian.txt` |
| Mod descriptor | The mod | Imperator: Rome launcher and loader | Host schema | `dracones-invictus.mod`, `dracones-invictus/descriptor.mod` |
| Localisation file | The mod | Imperator: Rome localisation loader | Host schema | `dracones-invictus/localization/english/zzz_DI_l_english.yml` |
| Override precedence | Host virtual filesystem | Both | Repository-relative path and filename ordering | Whole repository |
| Release archive | GitHub Releases | GitHub Release workflow | Workflow naming convention | `.github/workflows/github-release.yml` |

## Culture group schema

The culture group block is the primary data contract. The mod supplies exactly one top-level block, `dacia_group`, that supersedes the vanilla declaration of the identical name.

```text
dacia_group = {
        color = hsv { 0.0 0.5 1.0 }

        primary = archers
        second = heavy_infantry
        flank = light_cavalry
        primary_navy = tetrere
        secondary_navy = octere
        flank_navy = liburnian

        levy_template = levy_dacians

        male_names = { ... }
        female_names = { ... }
        family = { ... }

        culture = { ... }

        barbarian_names = { ... }

        graphical_culture = greek_gfx
        ethnicities = { 10 = levantine }
}
```

### Field reference

| Field | Type | Cardinality | Present in this mod | Host-visible effect |
|-------|------|-------------|---------------------|---------------------|
| `color` | `hsv` triple | 1 | Yes, unchanged from vanilla | Culture group colour in interface and map modes |
| `primary` | Unit type identifier | 1 | Yes, `archers` | Primary levy and army composition reference |
| `second` | Unit type identifier | 1 | Yes, `heavy_infantry` | Secondary composition reference |
| `flank` | Unit type identifier | 1 | Yes, `light_cavalry` | Flank composition reference |
| `primary_navy` | Vessel type identifier | 1 | Yes, `tetrere` | Naval composition reference |
| `secondary_navy` | Vessel type identifier | 1 | Yes, `octere` | Naval composition reference |
| `flank_navy` | Vessel type identifier | 1 | Yes, `liburnian` | Naval composition reference |
| `levy_template` | Identifier | 1 | Yes, `levy_dacians` | Selects a levy composition template declared in vanilla `game/common/levy_templates/01_dacians.txt` |
| `male_names` | Name token list | 1 | Yes, extended | Character name pool for male characters of the group |
| `female_names` | Name token list | 1 | Yes, extended | Character name pool for female characters of the group |
| `family` | Dot-separated tuple list | 1 | Yes, unchanged from vanilla | Family name forms |
| `culture` | Named records | 1 | Yes, unchanged from vanilla | Constituent culture identifiers |
| `barbarian_names` | Upper-case token list | 1 | Yes, unchanged from vanilla | Names for barbarian-tier polities |
| `graphical_culture` | Identifier | 1 | Yes, `greek_gfx` | Model and interface art set |
| `ethnicities` | Weighted identifiers | 1 | Yes, `10 = levantine` | Portrait ethnicity weighting |

### Name token contract

- Name tokens are whitespace-separated and order-independent.
- The block is positional only to the extent that a token must lie inside the correct block.
- Comments introduced with `#` are ignored by the loader and are used in this repository to delimit the vanilla and mod contributions.
- Vanilla tokens are retained verbatim; the mod appends its own tokens after them. Concentration is therefore additive, and no vanilla name is removed.

Verified concentration against the shipped vanilla file:

| Block | Vanilla tokens | Mod tokens | Added |
|-------|----------------|------------|-------|
| `male_names` | 45 | 125 | 79 |
| `female_names` | 13 | 18 | 5 |
| `family` | 18 | 18 | 0 |
| `barbarian_names` | 10 | 10 | 0 |
| `culture` records | 13 | 13 | 0 |

### Culture record contract

Each record inside `culture` is a key with an empty or near-empty body:

```text
        culture = {
                dacian = {}
                getian = {}
                triballoi = {}
                moesi = {}
                harpii = {}
                costoboci = {}
                buridavensi = {}
                caucoensi = {}
                ratacensi = {}
                biephi = {}
                odrysi = { levy_template = levy_thracians }
                bithynian = {}
                mariandynian = {}
        }
```

- The identifier is the record key, so it is a contract with any content that references the culture by name.
- `odrysi` carries a per-culture `levy_template` override that supersedes the group-level template for that culture alone. This is the only per-culture body in the group.
- The mod does not add, rename, or delete any culture record.

## Mod descriptor schema

Two files use the identical schema and, in this repository, the identical content.

```text
name="Dracones Invictus"
tags={
        "Ironman Compatible"
        "Interface"
}
supported_version="2.0.*"
path="mod/dracones-invictus"
```

| Field | Type | Value | Contract |
|-------|------|-------|----------|
| `name` | Quoted string | `Dracones Invictus` | Display name in the launcher and distribution channels |
| `tags` | Quoted string set | `Ironman Compatible`, `Interface` | Launcher-visible characteristics |
| `supported_version` | Quoted version pattern | `2.0.*` | Version compatibility gate evaluated by the loader |
| `path` | Quoted relative path | `mod/dracones-invictus` | Mount point for the mod content |

Local-descriptor variant: a descriptor installed into the user mod directory may instead carry an absolute `path` such as a Steam Workshop content directory. The `ugc_<itemid>.mod` file placed by the Workshop subscriber is such a variant and preserves the same `name`, `tags`, and `supported_version` values.

## Localisation schema

```text
l_english:
 shamanism:0 "Zamolxian"
 convert_to_shamanism:0 "Embrace the Zamolxian Faith"
 convert_to_shamanism_desc:0 "The teachings of great Zamolxis has become more apparent and important among our young these last years. They look to his words on how to live their lives, and even some of our elders have been affected by this shift to the new faith. It is time the rest of us follow his ideas as well."
 desc_shamanism:0 "Whether Zamolxis was originally a prophet or a god, is unknown. The Dacians and Getae however, revere Zalmoxis as a divine being, ascribing many miraculous acts to him.."
 treasure_axe_of_zalmoxis.yml:0 "Zamolxis' Axe"
 treasure_axe_of_zalmoxis_desc:0 "Zamolxis is a philosopher, forefather, and divinity among the Thracians and Dacians, often portrayed with different weapons or tools."
```

| Record | Key | Vanilla location | Vanilla value | Mod value |
|--------|-----|------------------|---------------|-----------|
| Faith name | `shamanism` | `game/localization/english/text_l_english.yml` | `Zalmoxian` | `Zamolxian` |
| Conversion decision | `convert_to_shamanism` | `game/localization/english/decisions_l_english.yml` | `Embrace the Zalmoxian Faith` | `Embrace the Zamolxian Faith` |
| Conversion description | `convert_to_shamanism_desc` | `game/localization/english/decisions_l_english.yml` | mentions Zalmoxis | mentions Zamolxis |
| Faith description | `desc_shamanism` | `game/localization/english/text_l_english.yml` | mentions Zalmoxis | mentions Zamolxis |
| Treasure name | `treasure_axe_of_zalmoxis.yml` | `game/localization/english/treasures_l_english.yml` as `treasure_axe_of_zalmoxis` | `Zalmoxis' Axe` | Intended `Zamolxis' Axe` |
| Treasure description | `treasure_axe_of_zalmoxis_desc` | `game/localization/english/treasures_l_english.yml` | mentions Zalmoxis | mentions Zamolxis |

Record syntax rules observed by the host:

- The file opens with a language header line, `l_english:`, at column zero.
- Each subsequent record begins with one leading space, then `key:version "text"`.
- The version integer is the localisation version, not the value version.
- Encoding is UTF-8 with a byte order mark, matching the vanilla localisation files.
- Files are evaluated in load order, and the last record for a key wins. The `zzz_DI_` prefix places this file after the vanilla files.

> **Defect to be aware of**: the treasure name record uses the key `treasure_axe_of_zalmoxis.yml`, which contains a literal `.yml` fragment. The vanilla key is `treasure_axe_of_zalmoxis`. The record therefore defines a distinct key rather than superseding the vanilla one, so the axe display name is not expected to change in game. This is recorded in [../operations/ambiguities-and-open-questions.md](../operations/ambiguities-and-open-questions.md) and [../operations/troubleshooting.md](../operations/troubleshooting.md).

## Override precedence contract

The loader resolves the mod content through the repository-relative path, not through a manifest.

| Mod path | Vanilla path | Effect |
|----------|--------------|--------|
| `common/cultures/00_dacian.txt` | `game/common/cultures/00_dacian.txt` | File-level supersession of the whole Dacian culture group file |
| `localization/english/zzz_DI_l_english.yml` | No vanilla file of that name | Record-level supersession by later load order |

Because the culture override is file-level rather than record-level, the mod file must remain a complete and valid declaration of `dacia_group`. It cannot declare only the added names. Any vanilla field omitted from the mod file is lost for that group.

## Release archive contract

| Concern | Value |
|---------|-------|
| Build trigger | Publication of a GitHub release, or manual dispatch with a tag input |
| Archive name | `dracones-invictus_<tag-without-v>.zip`, for example `dracones-invictus_2.0.0.zip` |
| Archive content | The `dracones-invictus/` directory tree and the root `dracones-invictus.mod`, at the archive root |
| Asset location | `https://github.com/hmlendea/imperator-dracones-invictus/releases/download/<tag>/<asset>` |
| Consumer trigger | Push of a tag matching `v*.*.*` |
| Extraction target | `dracones-invictus/` in the workflow workspace |
| Steam upload target | Steam Workshop application `859580`, item `1745923673` |
| Nexus upload target | Nexus Mods `imperatorrome/mods/24`, file `526528`, archive `release.zip` displayed as `dracones-invictus.zip` |

## Versioning

| Surface | Scheme | Evidence |
|---------|--------|----------|
| Repository releases | Semantic version tags, `v<major>.<minor>.<patch>` | `v2.0.0`, `v1.0.1` |
| Historical tag | A non-semantic legacy tag exists | `v19.139` |
| Host compatibility | Pattern `2.0.*` | Both descriptors |

The version scheme is evidenced by the tags and by the release asset naming in the workflow. No stability or deprecation policy is documented in the repository, so none is asserted here.

## Related documentation

- [../data-model.md](../data-model.md) for the entity view of the culture group, name token, and localisation record.
- [../flows/startup-and-mounting.md](../flows/startup-and-mounting.md) for the resolution sequence that consumes these contracts.
- [../invariants.md](../invariants.md) for the properties that must hold across every revision.
- [../configuration.md](../configuration.md) for the descriptor configuration areas and load-order prefixes.
