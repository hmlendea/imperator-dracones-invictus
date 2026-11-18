# Error handling

The mod contains no code, so it owns no error handling. Every failure is produced by the host, the launcher, or the release pipeline, and is observed by the player or the maintainer. This document records the failure modes and the diagnosis at the boundary that owns them.

## Host-side failures

### Version gate refusal

| Aspect | Detail |
|--------|--------|
| Trigger | `supported_version` does not match the running host version |
| Owner | Host loader |
| Observable symptom | The mod appears disabled or absent; the culture and text changes do not appear |
| Mod-side recovery | Revise both descriptors to the current host version pattern |
| Preventive control | [configuration.md](./configuration.md) records the revision procedure |

### Malformed culture block

| Aspect | Detail |
|--------|--------|
| Trigger | Unbalanced braces, a missing required field, or a syntax error in `00_dacian.txt` |
| Owner | Host culture loader |
| Observable symptom | The group may be discarded silently, or the load may fail; the host's own behaviour governs |
| Mod-side recovery | Restore the complete field set, re-diff against the host file |
| Preventive control | Structural checklist in [testing.md](./testing.md) |

### Localisation key mismatch

| Aspect | Detail |
|--------|--------|
| Trigger | A record key does not match a host key exactly, or the file sorts too early |
| Owner | Host localisation loader |
| Observable symptom | The host or vanilla text remains, and the mod appears to have no effect |
| Mod-side recovery | Compare the key character by character with the host file |
| Preventive control | Key agreement procedure in [testing.md](./testing.md) |
| Known instance | The `.yml` fragment in the treasure name key; see [operations/troubleshooting.md](./operations/troubleshooting.md#the-treasure-display-name-does-not-change) |

### Mount path mismatch

| Aspect | Detail |
|--------|--------|
| Trigger | `path` does not match the actual directory name |
| Owner | Host loader |
| Observable symptom | The mod does not mount |
| Mod-side recovery | Align `path` with the directory name in both descriptors |
| Preventive control | Descriptor agreement check in [testing.md](./testing.md) |

## Pipeline failures

### Archive build failure

| Aspect | Detail |
|--------|--------|
| Trigger | A packaged path such as `dracones-invictus/` or `dracones-invictus.mod` is missing from the checked-out tag |
| Owner | GitHub Release workflow `Build the release archive` step |
| Observable symptom | `zip` fails or the attached archive is incomplete |
| Recovery | Correct the tag and republish the release |
| Preventive control | Packaging requirements in [build-and-deployment.md](./build-and-deployment.md) |

### Download failure

| Aspect | Detail |
|--------|--------|
| Trigger | The release asset does not exist, or its name does not match `dracones-invictus_<tag-without-v>.zip` |
| Owner | Workshop workflow `Prepare` step |
| Observable symptom | `wget` fails; the job aborts before upload |
| Recovery | Publish the release so the GitHub Release workflow attaches the archive, then re-run |
| Preventive control | Release procedure in [build-and-deployment.md](./build-and-deployment.md) |

### Nexus Mods upload failure

| Aspect | Detail |
|--------|--------|
| Trigger | The `NEXUSMODS_API_KEY` secret is invalid or expired, the file identifier is wrong, or the action interface changed |
| Owner | Nexus Mods workflow `Upload` step |
| Observable symptom | The action fails; the Nexus Mods file is not updated |
| Recovery | Correct the secret or the file identifier and re-run the workflow |
| Preventive control | Secrets and file identifier maintenance in [configuration.md](./configuration.md) |

### Upload failure

| Aspect | Detail |
|--------|--------|
| Trigger | Invalid or expired Steam credentials, or an interface change in the upload action |
| Owner | Workflow `Upload` step |
| Observable symptom | The step fails; the Workshop item retains its previous content |
| Recovery | Verify the repository secrets and re-run; review the action version if the inputs changed |
| Preventive control | Secrets remain in repository configuration only |

### Silent wrong-content publication

| Aspect | Detail |
|--------|--------|
| Trigger | A version tag pushed before the mod data was validated |
| Owner | Maintainer process, not the pipeline |
| Observable symptom | Players receive an untested package |
| Recovery | Publish a corrected release and note it in the change note |
| Preventive control | Manual checklist in [testing.md](./testing.md) before tagging |

## Degradation and fallback

| Situation | Degradation | Fallback |
|-----------|-------------|----------|
| Version gate refuses | No mod effect | Host data only |
| Culture file malformed | Group content lost or load failed | Host group data |
| Localisation key unmatched | Text unchanged | Host text |
| Thumbnail missing | Metadata display degraded | Mod still mounts and functions |
| Workshop upload fails | Publication delayed | Previous item content remains |

No failure mode produces partial corruption of host data, because the mod never writes to host files. The worst outcome is a no-op, which is why the testing procedures emphasise positive confirmation rather than absence of errors.

## No retry, timeout, or backpressure

These concepts do not apply. There is no mod-owned operation that could be retried, timed out, or backpressured.

## Related documentation

- [testing.md](./testing.md) — the procedures that detect each failure.
- [operations/troubleshooting.md](./operations/troubleshooting.md) — user-visible diagnosis.
- [invariants.md](./invariants.md) — the properties whose violation produces these failures.
