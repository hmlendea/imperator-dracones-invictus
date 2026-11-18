# Flow: release and publishing

This flow covers what happens from the maintainer deciding to publish to the content appearing on the Steam Workshop.

## Preconditions

- The mod data has passed the manual validation checklist in [testing.md](../testing.md).
- The intended version is decided.
- The descriptors carry the intended `supported_version`.

## Sequence

```mermaid
sequenceDiagram
    autonumber
    participant M as Maintainer
    participant G as GitHub Releases
    participant A as GitHub Actions (github-release.yml)
    participant W as GitHub Actions (steam-workshop.yml)
    participant N as GitHub Actions (nexus-mods-release.yml)
    participant S as Steam Workshop
    participant X as Nexus Mods
    participant P as Player

    M->>M: Validate the data against the checklist
    M->>M: Push tag vX.Y.Z
    M->>G: Publish the release for tag vX.Y.Z
    G->>A: Release published trigger
    A->>A: Check out the tag
    A->>A: Build dracones-invictus_X.Y.Z.zip from the mod directory and descriptor
    A->>G: Attach the archive to the release
    M->>W: Tag push vX.Y.Z
    W->>W: Match the trigger pattern v*.*.*
    W->>G: Download the asset for the tag
    G-->>W: The archive
    W->>W: Unzip into dracones-invictus/
    W->>S: Upload with appid 859580 and itemid 1745923673
    S-->>W: Item updated
    N->>N: Check out the tag and build release.zip
    N->>X: Upload to file 526528
    X-->>N: File updated
    S-->>P: The item is available at the new version
    X-->>P: The file is available at the new version
```

## Step detail

| Step | Actor | Input | Output | Failure behaviour |
|------|-------|-------|--------|-------------------|
| 1 | Maintainer | Changed data | Validated content | A missed check publishes an unvalidated package |
| 2 | Maintainer | Version number | A tag and a published release | A release published without the tag, or a tag without a release, breaks a later step |
| 3 | Actions (`github-release.yml`) | The released tag | A checked-out tag | A malformed tag reference fails the job |
| 4 | Actions (`github-release.yml`) | The tag name | A version with the leading `v` stripped | A tag outside the scheme produces an odd archive name |
| 5 | Actions (`github-release.yml`) | The mod directory and descriptor | An attached archive | A missing path fails the `zip` step |
| 6 | Maintainer | Tag | A Workshop workflow trigger | A tag outside the pattern does not trigger anything |
| 7 | Actions (`steam-workshop.yml`) | The tag reference | A resolved asset URL | A malformed construction fails the download |
| 8 | Actions (`steam-workshop.yml`) | The asset URL | The archive in the workspace | A download failure aborts the job |
| 9 | Actions (`steam-workshop.yml`) | The archive | The mod directory in the workspace | An extra nesting level produces a doubly nested tree; see [build-and-deployment.md](../build-and-deployment.md) |
| 10 | Actions (`steam-workshop.yml`) | The directory and the secrets | A published Workshop revision | An authentication or interface failure aborts the job |
| 11 | Actions (`nexus-mods-release.yml`) | The tag and the secrets | A built archive | A missing path or an authentication failure aborts the job |
| 12 | Actions (`nexus-mods-release.yml`) | The archive | An updated Nexus Mods file | An upload or version conflict aborts the job |
| 13 | Player | A subscription or download | The updated mod | None |

## The asset name computation

The download step computes the asset name from the tag in two parts:

| Part | Source | Value for tag `v2.0.0` |
|------|--------|------------------------|
| Repository and tag | `github.ref_name` | `v2.0.0` |
| Version suffix | `GITHUB_REF:11`, stripping `refs/tags/v` | `2.0.0` |
| Full asset name | Concatenation | `dracones-invictus_2.0.0.zip` |

Any deviation in the tag prefix length, the tag scheme, or the asset naming shifts the result and breaks the download. The contract is recorded in [invariants.md](../invariants.md).

## Alternate and manual paths

```mermaid
flowchart TD
    Release[Release published]
    Release --> Auto[Automated: archive build, attachment, Workshop upload, and Nexus Mods upload]
    Release --> ManualGh[Manual: GitHub Releases download by the player]
    Auto --> Player[Player]
    ManualGh --> Player
```

The automated path attaches the archive, publishes the Workshop item, and uploads to Nexus Mods. All three channels are now workflow-driven.

## Non-obvious behaviours

| Behaviour | Explanation |
|-----------|-------------|
| The workflows do not compile anything | The archive is built by zipping the already-authored mod directory |
| The Workshop upload depends on the release archive | The tag-push trigger and the release-published trigger are distinct, so a missing or misnamed asset fails the download step |
| The change note points at another repository | The `changenote` input links to `more-cultural-names`; recorded in [operations/ambiguities-and-open-questions.md](../operations/ambiguities-and-open-questions.md) |
| The Nexus workflow builds its own archive | Unlike the Workshop workflow, it does not consume the GitHub release asset, so it does not depend on the release being published |
| A failed upload leaves the previous item content | No partial state is introduced |
| No rollback exists | Correcting a bad release means publishing another release |

## Verifying a release

1. Confirm that the GitHub Release workflow attached the archive.
2. Confirm that the Workshop workflow run completed both the `Prepare` and `Upload` steps, and that the item shows the new version.
3. Confirm that the Nexus Mods workflow run completed both the `Build the release archive` and `Upload` steps, and that the file shows the new version.
3. Confirm that the change note renders as a link.
4. Optionally, load the game and run the visual checks in [testing.md](../testing.md).

## Related documentation

- [build-and-deployment.md](../build-and-deployment.md) — the pipeline reference.
- [integrations.md](../integrations.md) — the external systems involved.
- [operations/troubleshooting.md](../operations/troubleshooting.md#publishing-failures) — diagnosis.
