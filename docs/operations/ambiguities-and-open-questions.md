# Ambiguities and open questions

Items where the evidence available in the repository is incomplete, contradictory, or absent. Each entry states what is known, what is unknown, and what would resolve it.

## Verified defects

### A-1: The treasure name key contains a filename fragment

| Aspect | Detail |
|--------|--------|
| Known | The localisation record uses the key `treasure_axe_of_zalmoxis.yml` |
| Known | The host key is `treasure_axe_of_zalmoxis`, declared in `treasures_l_english.yml` |
| Consequence | The record defines a distinct key, so the axe display name does not change in game |
| Unknown | Whether the original author intended a different key or made a transcription error |
| Resolution | Change the key to the host key verbatim, then confirm in game |
| Verified by | Direct comparison of the mod key with the host key |

### A-2: The release change note links to a different repository

| Aspect | Detail |
|--------|--------|
| Known | The `changenote` input constructs a URL containing `hmlendea/more-cultural-names` |
| Consequence | The published change note points at the wrong release page |
| Unknown | Whether this is a copy-paste artefact or an intentional cross-reference |
| Resolution | Point the URL at `hmlendea/imperator-dracones-invictus` |
| Verified by | Read of the workflow `changenote` input |

## Unverified areas

### A-3: The README manual-download link

| Aspect | Detail |
|--------|--------|
| Known | The README manual installation step previously linked the latest release to `hmlendea/more-cultural-names/releases` |
| Resolution | Corrected to point at `hmlendea/imperator-dracones-invictus/releases/latest` |
| Status | Resolved |

### A-4: Whether the mod data matches a clean upstream install

| Aspect | Detail |
|--------|--------|
| Known | The mod data was verified by diff against the host installation on the maintainer machine |
| Unknown | Whether that installation is a pristine 2.0.x install or carries other mod content |
| Consequence | A diff against a modified install could mask a discrepancy |
| Resolution | Re-diff against a clean install, or record the host build identifier used |

### A-5: The host's exact version-build string

| Aspect | Detail |
|--------|--------|
| Known | The loaded installation reports 2.0.x-compatible data, and the descriptors gate on `2.0.*` |
| Unknown | The precise build or patch revision string |
| Consequence | A future host update could change the schema without an obvious version marker in this repository |
| Resolution | Record the host build identifier at each validation |

### A-6: Whether other mods depend on this one

| Aspect | Detail |
|--------|----------|
| Known | This mod depends only on host identifiers and declares no dependency on another mod |
| Unknown | Whether any third-party mod or submod references `dacia_group` or expects the vanilla name set |
| Consequence | A third-party mod that expects the vanilla group declaration may behave differently with this mod enabled |
| Resolution | Inspect the compatibility notes of related mods, or state the absence of a guarantee |

### A-7: Whether the name pools have a host-imposed size limit

| Aspect | Detail |
|--------|--------|
| Known | The current pools are 125 male and 18 female tokens, and the host loads them |
| Unknown | Whether the host truncates, warns, or degrades beyond some threshold |
| Consequence | A large future addition could behave unexpectedly |
| Resolution | Observe host behaviour while increasing a pool incrementally |

### A-8: Whether a malformed block fails silently or loudly

| Aspect | Detail |
|--------|--------|
| Known | The host owns parsing and this repository contains no error reporting of its own |
| Unknown | Whether a malformed group is discarded silently, partially accepted, or reported |
| Consequence | The documented failure behaviour in [error-handling.md](../error-handling.md) is partly inferred |
| Resolution | Test a deliberately malformed block in a disposable installation and record the observed behaviour |

### A-9: Whether the `Interface` tag has an effect

| Aspect | Detail |
|--------|--------|
| Known | Both descriptors declare the tags `Ironman Compatible` and `Interface` |
| Unknown | What the launcher or load logic does with the `Interface` tag, since the mod changes no interface asset |
| Consequence | The tag may be cosmetic, or it may carry a load-order meaning |
| Resolution | Consult host documentation or test with and without the tag |

### A-10: Whether the two descriptors must remain byte-identical

| Aspect | Detail |
|--------|--------|
| Known | The two files currently carry the same four values, but the `path` field can legitimately differ, because a Workshop-installed descriptor carries an absolute path |
| Unknown | Whether any other field is permitted to differ |
| Consequence | A rule that demanded byte identity would be too strict |
| Resolution | The invariant is stated as value agreement for the four shared fields rather than byte identity; revisit if a divergence proves harmless |

## Deliberately unresolved

| Item | Reason for deferral |
|------|---------------------|
| Pinning the Workshop upload action | Not evidenced as required; a decision, not an unknown |
| Adding non-English localisation | A scope decision, not an unknown |
| Adding an automated validation script | The repository has no execution surface; a decision recorded in DD-10 |
| Adding a content integrity checksum to releases | A supply-chain improvement with no current requirement |

## How to resolve an entry

1. Gather the evidence named in the `Resolution` row.
2. Update the affected document rather than only this page.
3. Move the entry to a resolved note in [design-decisions.md](../design-decisions.md) if the resolution is a decision, or delete it if the resolution is a correction.
4. Record the resolution date in the maintenance table in [INDEX.md](../INDEX.md).

## Related documentation

- [design-decisions.md](../design-decisions.md) — the decisions that are established.
- [invariants.md](../invariants.md) — the properties that are established.
- [operations/troubleshooting.md](./troubleshooting.md) — the user-visible impact of A-1 and A-2.
