# Testing

## Position

There is no automated test suite, no test project, and no test command in this repository. The mod is declarative data consumed by an external game engine, and the repository contains no harness capable of loading that engine. Verification is therefore manual, evidence-based, and documented here in full.

## Validation levels

| Level | Target | Boundary verified | Method available |
|-------|--------|-------------------|------------------|
| Structural | Descriptor and data files | Syntax, completeness, and descriptor agreement | Static inspection |
| Key agreement | Localisation records | Exact key match with the host | Diff against installed host data |
| Concentration | Name pools | Additive, non-destructive edit | Diff against installed host data |
| Load integration | Mount and resolution | The mod mounts and supersedes the vanilla data | In-game session |
| Visual | Text and content | The intended names and text appear | In-game observation |
| Release integration | Archive and pipeline | The workflow retrieves and publishes the asset | Workflow run inspection |

## Structural checklist

Run before every commit that changes data or metadata:

- [ ] `dacia_group` opens and closes exactly once, with balanced braces.
- [ ] Every field present in the host version of the file remains present: `color`, `primary`, `second`, `flank`, `primary_navy`, `secondary_navy`, `flank_navy`, `levy_template`, `male_names`, `female_names`, `family`, `culture`, `barbarian_names`, `graphical_culture`, `ethnicities`.
- [ ] Every vanilla name token is still present in its original block.
- [ ] Every mod token sits after the `# Dracones Invictus - Dacian` marker.
- [ ] `levy_dacians` remains the group-level `levy_template`, and `odrysi` retains its `levy_thracians` override.
- [ ] `dracones-invictus.mod` and `dracones-invictus/descriptor.mod` agree on `name`, `tags`, `supported_version`, and `path`.
- [ ] `path` equals `mod/dracones-invictus`, matching the directory name.
- [ ] `zzz_DI_l_english.yml` retains the `l_english:` header at column zero and one leading space before each record.
- [ ] The file remains UTF-8 with a byte order mark, matching the host localisation convention.
- [ ] No key contains a filename fragment such as `.yml`.

## Key agreement procedure

This is the highest-value check, because key disagreement fails silently.

1. Open the host file that declares each key, for example `game/localization/english/decisions_l_english.yml`.
2. Copy the host key exactly, character for character, including underscore placement and the `:0` version separator.
3. Paste the key into `zzz_DI_l_english.yml` and confirm that the two strings are identical.
4. Confirm that the mod file's filename still sorts after the host file that declares the key.

The six verified keys and their host locations are tabulated in [api-reference/INDEX.md](./api-reference/INDEX.md).

## Concentration procedure

The mod must remain additive.

1. Diff the mod culture file against the host version of the same path.
2. Confirm that the diff additions are comment lines and name lines only.
3. Confirm that no host token has been removed, reordered, or altered.

The verified baseline against the installed host data is: 79 male names added, 5 female names added, 0 family names changed, 0 culture records changed.

## Load integration procedure

1. Confirm that the host installation is version 2.0.x.
2. Enable the mod in the launcher and start the game.
3. Confirm that the launcher displays the mod without a version warning.
4. Create or inspect a Dacian character and confirm that names from the `# Dracones Invictus - Dacian` block appear.
5. Open the conversion decision and confirm that its title and description read `Zamolxian`-based text.
6. Open the faith view and confirm the faith name and description read the revised text.
7. Open the Zalmoxis treasure and confirm its name and description text.

Step 7 is expected to reveal the known defect in [operations/troubleshooting.md](./operations/troubleshooting.md#the-treasure-display-name-does-not-change).

## Release integration procedure

1. Push a tag matching `v*.*.*` and publish the GitHub release.
2. Observe the GitHub Release workflow attaching `dracones-invictus_<version>.zip` to the release.
3. Observe the Workshop workflow `Prepare` step completing the download and extraction.
4. Observe the Workshop `Upload` step completing and confirm the item shows the new version.
5. Observe the Nexus Mods workflow `Build the release archive` and `Upload` steps completing, and confirm the file shows the new version.

## Coverage gaps

| Gap | Consequence | Mitigation |
|-----|-------------|------------|
| No automated schema validation against the host schema | A malformed block may reach a release before a human notices | Structural checklist before commit |
| No automated key comparison | A mistyped key fails silently and changes nothing in game | Key agreement procedure |
| No automated descriptor comparison | Descriptor divergence manifests as inconsistent mounting | Both descriptors revised together in one commit |
| No automated mount test | A version gate closure is discovered only by a player | Manual in-game check after any `supported_version` change |
| No upstream CI baseline for the host data | A host update may invalidate a reference silently | Re-diff against the installed host data after a host update |

## Commands

There is no executable verification command in the repository, so none is prescribed here. The applicable checks are the manual procedures above. This is a deliberate coverage restriction rather than an omission, and it is recorded in [design-decisions.md](./design-decisions.md).

## Related documentation

- [testing boundaries in operations/troubleshooting.md](./operations/troubleshooting.md) — interpreting failures.
- [invariants.md](./invariants.md) — the properties that the checks protect.
- [flows/startup-and-mounting.md](./flows/startup-and-mounting.md) — the behaviour being verified.
