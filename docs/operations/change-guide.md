# Change guide

Safe procedures for the changes that this repository actually receives. Every procedure ends with a verification step, because the failure modes here are silent.

## General rule

Two different override mechanisms are in play, and each has its own trap.

| Surface | Model | Trap |
|---------|-------|------|
| Culture data | Whole-file supersession | Omitting a field loses it |
| Localisation | Last-wins record merge | A mistyped key silently does nothing |

Read [architecture.md](../architecture.md) before making a change in either surface.

## Adding or revising names

**Target**: `dracones-invictus/common/cultures/00_dacian.txt`.

1. Open the file and locate the block: `male_names`, `female_names`, or `family`.
2. Locate the `# Dracones Invictus - Dacian` comment. Additions belong after it.
3. Confirm that the `# Vanilla` section above it is untouched.
4. Append the new token or tokens to an existing mod line, matching the surrounding separation with a single space.
5. For a family name, use the four-segment dot-separated form.
6. Preserve the tab indentation of the file. Do not reformat surrounding lines.
7. Verify:
   - The file still opens and closes `dacia_group` exactly once with balanced braces.
   - The host tokens are unchanged.
   - The mod tokens appear only after the mod comment.
8. Diff against the host file to confirm that the change is additive:
   - Host path: `<game>/common/cultures/00_dacian.txt`.
   - Expect additions only.
9. Load the game and confirm the name appears.

**Do not**: restructure the block, reorder host tokens, or split a token across lines with a line break inside the word.

## Adding or revising culture records

**Target**: the `culture` block in the same file.

1. Add a record in the form `<identifier> = {}`.
2. Add a per-culture `levy_template` inside the braces only when a specific override is intended, as `odrysi` does.
3. Confirm that no existing record was renamed or removed, because culture identifiers are save contracts.
4. Verify by loading the game and confirming that the group still contains every culture.

Removing a culture record is prohibited by [invariants.md](../invariants.md) unless the removal is a deliberate, reasoned compatibility decision.

## Revising localisation text

**Target**: `dracones-invictus/localization/english/zzz_DI_l_english.yml`.

1. Copy the key from the host file that declares it. Do not retype it.
2. Confirm that the key in the mod file matches character for character, including underscore placement.
3. Confirm that the record keeps the leading single space and the `:0` version separator.
4. Replace only the quoted value.
5. Save with UTF-8 and a byte order mark.
6. Verify:
   - The `l_english:` header remains at column zero.
   - No key contains a filename fragment.
   - The file still sorts last, so the `zzz_` prefix is intact.
7. Load the game and confirm the new text appears.

**Known defect**: the treasure name record currently uses a key containing `.yml`, so it defines a new key instead of superseding the host one. Correcting it means using the host key `treasure_axe_of_zalmoxis` verbatim. See [operations/troubleshooting.md](./troubleshooting.md#the-treasure-display-name-does-not-change).

## Adding a new localisation record

1. Confirm that the host declares the key; otherwise the record has no visible effect and may indicate a misunderstanding.
2. Add the record exactly as the host spells the key.
3. Confirm that the record's presence does not unintentionally hide host text that should remain, because last-wins means the mod value replaces the host value entirely.
4. Verify as in the previous procedure.

## Changing the targeted game version

1. Edit `supported_version` in `dracones-invictus.mod`.
2. Edit `supported_version` in `dracones-invictus/descriptor.mod` to the identical value.
3. Re-diff the mod culture file against the host file, because a version change may accompany a schema change.
4. Re-verify every localisation key against the host file, because key names may change.
5. Load the game and confirm that the mod mounts.

## Replacing the thumbnail

1. Replace `dracones-invictus/thumb.jpg`.
2. Keep the format as JPEG and the dimensions near 500×500.
3. Confirm that the file is a valid image.
4. Confirm that no editor backup file was left in the directory, because the whole directory is mounted and packaged.

## Changing the release pipeline

1. Edit `.github/workflows/steam-workshop.yml`.
2. Preserve the trigger pattern unless the release scheme changes.
3. Preserve the asset name computation unless the asset naming convention changes, and if it changes, update [build-and-deployment.md](../build-and-deployment.md) and the release procedure.
4. Never print, echo, or serialise the environment, because it holds the Steam secrets.
5. Verify by pushing a test tag and inspecting the run outcome.

## Renaming the mod directory

This is the most invasive change available, because the mount contract depends on the name.

1. Rename the directory.
2. Update `path` in both descriptors to `mod/<new-name>`.
3. Confirm that the launcher picks up the change.
4. Note that the Workshop item and any subscriber installation keep the old name until republished.

Prefer not to rename. The name appears in the descriptor, the distribution metadata, and subscriber installations.

## Commit checklist

- [ ] Both descriptors agree, if either was changed.
- [ ] The culture file is a complete group definition with balanced braces.
- [ ] Name concentration remains additive.
- [ ] Localisation keys match host keys exactly.
- [ ] Filename prefixes are unchanged.
- [ ] No backup, temporary, or editor file was added to the mod directory.
- [ ] The relevant verification section in [testing.md](../testing.md) was run.

## Related documentation

- [testing.md](../testing.md) — the verification checklists referenced above.
- [invariants.md](../invariants.md) — the properties each procedure protects.
- [configuration.md](../configuration.md) — the descriptor and prefix configuration areas.
