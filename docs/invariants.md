# Invariants

Each invariant below must hold across every revision. A violation is a defect even when nothing visibly breaks, because the impact may appear only in a later session or for a different player.

## I-1: The culture file is a complete group definition

**Statement**: `dracones-invictus/common/cultures/00_dacian.txt` declares every field that the host version of the same path declares, with balanced braces.

**Reason**: The override is whole-file supersession. Any field omitted is lost for the group, with no error.

**Violation symptom**: A group attribute silently reverts to absent, for example the unit composition or the graphical culture.

**Verification**: Structural checklist in [testing.md](./testing.md).

## I-2: Name concentration is additive

**Statement**: No host name token is removed, reordered, or altered in any of the `male_names`, `female_names`, `family`, or `barbarian_names` blocks.

**Reason**: Host tokens may be referenced by saves or by player expectation.

**Violation symptom**: Characters generated earlier with a now-removed name, or player-visible name loss.

**Verification**: Diff against the installed host file, as described in [testing.md](./testing.md).

## I-3: Host identifiers remain unchanged

**Statement**: The group key `dacia_group`, every culture key inside the `culture` block, the levy template reference `levy_dacians`, and the localisation keys that the host owns remain identically spelled.

**Reason**: These are host and save contracts. A renamed key or identifier orphans existing state or silently stops superseding text.

**Violation symptom**: Lost content, or a display text change that does not take effect.

**Verification**: Structural checklist and key agreement procedure.

## I-4: Localisation keys match the host exactly

**Statement**: Every record key in `zzz_DI_l_english.yml` equals a host key character for character, with no filename fragment or other stray text.

**Reason**: Last-wins matching is exact. A near-match defines a new unused key and changes nothing.

**Violation symptom**: Text remains at its vanilla value with no error.

**Verification**: Key agreement procedure in [testing.md](./testing.md).

**Known current violation**: The treasure name key contains `.yml`, as recorded in [operations/troubleshooting.md](./operations/troubleshooting.md#the-treasure-display-name-does-not-change).

## I-5: Localisation precedence margin is preserved

**Statement**: The localisation filename continues to sort after every host file that declares a superseded key.

**Reason**: A host file that sorts later would win the record instead.

**Violation symptom**: Vanilla text appears despite a correct key.

**Verification**: Filename inspection against the host localisation listing.

## I-6: The two descriptors agree

**Statement**: `dracones-invictus.mod` and `dracones-invictus/descriptor.mod` carry identical `name`, `tags`, `supported_version`, and `path` values.

**Reason**: The host has no include mechanism, and divergence in the version gate or the mount path produces inconsistent mounting.

**Violation symptom**: The launcher accepts the mod while the loader rejects it, or the content does not resolve.

**Verification**: Structural checklist.

## I-7: The mount path matches the directory name

**Statement**: `path` equals `mod/dracones-invictus`, and the directory is named `dracones-invictus`.

**Reason**: The mount point is derived from the path value.

**Violation symptom**: The mod does not mount.

**Verification**: Structural checklist.

## I-8: The package remains content-only

**Statement**: The mod directory contains no executable file, script, library, installer, or archive.

**Reason**: The security and privacy posture depends on the mod being inert data, as recorded in [../../PRIVACY.md](../PRIVACY.md) and [security.md](./security.md).

**Violation symptom**: The documented posture becomes inaccurate, and the mod acquires a threat surface it previously lacked.

**Verification**: File inventory per [repository-structure.md](./repository-structure.md).

## I-9: The release asset naming contract holds

**Statement**: The asset attached to a tag `v<version>` is named `dracones-invictus_<version>.zip` and contains the `dracones-invictus/` directory tree at the archive root.

**Reason**: The `github-release.yml` build step produces that exact name from the tag, and the `steam-workshop.yml` download step constructs the same name to retrieve it.

**Violation symptom**: The build or download step fails and publication does not occur.

**Verification**: Release inspection, and inspection of the attached archive contents.

## I-10: English remains the only localised language unless a deliberate addition is made

**Statement**: Content changes continue to be supplied under `localization/english/` only, unless a deliberate decision adds another language directory.

**Reason**: A record placed in the wrong language directory has no effect for the intended language, and the limitation is documented, so an accidental addition would contradict the documentation.

**Violation symptom**: A text change appears for the wrong language or for none.

**Verification**: Directory inspection.

## I-11: The thumbnail remains a valid image at the expected size

**Statement**: `dracones-invictus/thumb.jpg` remains a valid JPEG at approximately 500×500.

**Reason**: The launcher and distribution channels display it; a corrupt or disproportionate image degrades presentation.

**Violation symptom**: A broken or distorted thumbnail.

**Verification**: Image inspection.

## I-12: Identifier stability preserves save compatibility

**Statement**: No change is made that renames or removes an identifier referenced by saved games.

**Reason**: Save compatibility is the main player-visible risk of a content mod, and the mod deliberately avoids it by superseding text rather than identifiers.

**Violation symptom**: Existing saves load with missing or corrupted entities.

**Verification**: Reasoning review before any identifier change, plus an in-game load of an existing save.

## Protected by design rather than by rule

| Property | Why no rule is needed |
|----------|-----------------------|
| No state corruption | The mod writes nothing, so corruption is impossible |
| No crash on data error | The host owns error handling and degrades by ignoring or discarding |
| No performance regression | The data is a handful of small text files |

## Related documentation

- [testing.md](./testing.md) — how each invariant is verified.
- [design-decisions.md](./design-decisions.md) — the reasoning behind these invariants.
- [api-reference/INDEX.md](./api-reference/INDEX.md) — the underlying contracts.
