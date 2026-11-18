# Design decisions

Each decision below is grounded in repository evidence. Where a decision is inferred rather than explicit, the inference is labelled.

## DD-1: File-level supersession over record-level patching for culture data

**Decision**: The mod replaces the whole `common/cultures/00_dacian.txt` file rather than appending a second file that declares only the added names.

**Evidence**: The repository contains exactly one culture file, and it reproduces every host field with vanilla tokens retained verbatim under a `# Vanilla` comment.

**Rationale**: The host treats the group as a single named object with no defined merge semantics for two declarations, so an additive second file cannot append names to the same group.

**Consequence**: Every revision must reproduce a complete, valid group definition. The cost is paid per edit; the benefit is that the override is deterministic, because it does not depend on filename ordering.

**Status**: Verified.

## DD-2: Record-level supersession for localisation data

**Decision**: The mod supplies only the keys it intends to supersede, in one file whose name sorts after every host file.

**Evidence**: `zzz_DI_l_english.yml` contains exactly the keys whose host text differs, and none other.

**Rationale**: The host localisation namespace is a flat key-value space with last-wins resolution, so a delta is composable where a whole-file replacement would not be.

**Consequence**: Supersession depends on the `zzz_` prefix and on exact key equality, both of which can fail silently. The failure modes are documented in [operations/troubleshooting.md](./operations/troubleshooting.md).

**Status**: Verified.

## DD-3: Identifier preservation for the faith and the treasure

**Decision**: The mod renames the Zalmoxian faith to Zamolxian in text without renaming the `shamanism` identifier, and without renaming the `treasure_axe_of_zalmoxis` treasure identifier.

**Evidence**: The file `00_dacian.txt` does not declare or alter any faith or treasure identifier, and the localisation file supplies new text only for existing keys.

**Rationale**: Identifiers are host and save contracts. Renaming an identifier would orphan existing player state, because saved characters refer to the old identifier. Text is display-only and is safe to supersede.

**Consequence**: The rename is cosmetic at the data level. Any future request to rename the identifier must be treated as a compatibility-breaking change, and must be justified against the save-orphaning consequence.

**Status**: Verified against the file contents; the save-orphaning consequence is a property of the host's identifier handling and is inferred, not verified in this repository.

## DD-4: Descriptor duplication instead of include

**Decision**: The root descriptor and the in-package descriptor carry identical values with no include or reference between them.

**Evidence**: Both files exist with the same four fields.

**Rationale**: The host has no include mechanism for descriptors. The duplication is imposed, not chosen.

**Consequence**: Divergence is possible and has a high-impact symptom in the version gate. The mitigation is the structural check in [testing.md](./testing.md) and the rule in [invariants.md](./invariants.md).

**Status**: Verified.

## DD-5: Additive name concentration with provenance comments

**Decision**: Vanilla name tokens are retained and marked with a `# Vanilla` comment; mod tokens follow a `# Dracones Invictus - Dacian` marker.

**Evidence**: Diff against the installed host file shows additions only.

**Rationale**: Additive concentration avoids removing host names that existing content or player expectation depends on, and the comments make the boundary reviewable.

**Consequence**: One token, `Cotiso`, appears in both the host and the mod groups, so it is duplicated. The duplicate is inert, and the alternative — deduplicating against the host — would couple the file to host contents and make future diffs noisier. Treat the duplication as accepted rather than accidental.

**Status**: Verified by diff. The inertness of the duplicate is inferred from token-list semantics.

## DD-6: Single-culture-group scope

**Decision**: The mod owns the Dacian group only.

**Evidence**: Exactly one culture file exists, and the group key is `dacia_group`.

**Rationale**: Concentration on one group keeps the file-level supersession risk contained to a single definition, and keeps the review surface small.

**Consequence**: Related content, such as the Thracian and Getian groups, remains host-owned. The `culture` block inside `dacia_group` lists those identifiers because the group contains those cultures, but the mod does not own their definitions elsewhere.

**Status**: Verified.

## DD-7: Unpinned action reference in the release workflow

**Decision**: The Workshop workflow consumes `hmlendea/steam-workshop-update@latest`, the GitHub Release workflow consumes the pin-major `softprops/action-gh-release@v2`, and the Nexus Mods workflow consumes the pin-minor `Nexus-Mods/upload-action@v1.0.0-beta.11`.

**Evidence**: The `uses` fields in `.github/workflows/steam-workshop.yml`, `.github/workflows/github-release.yml`, and `.github/workflows/nexus-mods-release.yml`.

**Rationale**: Not evidenced; inferred as convenience for the unpinned reference, because pinning to a commit hash would be the more conservative alternative.

**Consequence**: Behaviour of the Workshop upload step may change without any change in this repository. This is a supply-chain coupling rather than a data coupling, and it is recorded in [dependencies.md](./dependencies.md). The GitHub Release and Nexus Mods actions are pinned, which bounds surprise changes to their referenced lines.

**Status**: Verified presence of all three references; the unpinned rationale is inferred.

## DD-8: English-only localisation

**Decision**: Only English text is supplied.

**Evidence**: The only localisation directory is `localization/english/`.

**Rationale**: Not evidenced; inferred as scope limitation, because the mod's content change is a renaming of an English display term.

**Consequence**: Clients in other languages fall back to host text, or to missing text for records that the host does not declare. This is stated as a limitation in [../../ARCHITECTURE.md](../ARCHITECTURE.md) and is not a bug in the existing content.

**Status**: Verified presence of English-only content; the rationale is inferred.

## DD-9: Steam Workshop as the primary channel

**Decision**: The repository automates the GitHub release archive, the Steam Workshop upload, and the Nexus Mods upload.

**Evidence**: Three workflows exist (`.github/workflows/github-release.yml`, `.github/workflows/steam-workshop.yml`, and `.github/workflows/nexus-mods-release.yml`), and the README installation badges include all three channels.

**Rationale**: Not evidenced; inferred from the release, Workshop, and Nexus-specific triggers and workflow content.

**Consequence**: All three distribution channels are machine-published, so a publication defect propagates automatically to every channel. No channel retains a human checkpoint for the automated path.

**Status**: Verified that the GitHub release archive, the Workshop upload, and the Nexus Mods upload are automated; the rationale is inferred.

## DD-10: No documentation test harness

**Decision**: Verification of documentation accuracy is manual, through the checklist in [testing.md](./testing.md).

**Evidence**: No script, hook, or workflow validates the data files, and no test project exists.

**Rationale**: The repository has no execution surface, so a harness would test the host rather than the mod, and the host is out of scope.

**Consequence**: Documentation drift and data drift both go undetected until a human reviews them. The quarterly review cadence in [documentation-maintenance.md](./documentation-maintenance.md) is the compensating control.

**Status**: Verified.

## Decisions not made

| Candidate | Decision | Reason |
|-----------|----------|--------|
| Add a second culture file for names only | Rejected, see DD-1 | No merge semantics exist for a culture group |
| Rename the faith or treasure identifier | Rejected, see DD-3 | Orphans saved state |
| Pin the Workshop action | Deferred | Not evidenced as required; see DD-7 |
| Add non-English localisation | Deferred | Not evidenced as required; see DD-8 |

## Related documentation

- [invariants.md](./invariants.md) — properties that follow from these decisions.
- [ambiguities-and-open-questions.md](./operations/ambiguities-and-open-questions.md) — where the evidence is incomplete.
- [api-reference/INDEX.md](./api-reference/INDEX.md) — the contracts these decisions produce.
