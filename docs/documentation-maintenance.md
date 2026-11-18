# Documentation maintenance

## Ownership

The documentation is maintained by the repository maintainer. There is no separate documentation owner, and no automated generation step, so the documents are the source of truth for the reasoning that the data files cannot express.

## Update triggers

| Trigger | Documents to revise |
|---------|---------------------|
| A name or family change | [data-model.md](./data-model.md) composition figures, [repository-overview.md](./repository-overview.md) delivered content table, [api-reference/INDEX.md](./api-reference/INDEX.md) concentration table |
| A culture record change | [data-model.md](./data-model.md), [invariants.md](./invariants.md), [operations/change-guide.md](./operations/change-guide.md) |
| A localisation record change | [api-reference/INDEX.md](./api-reference/INDEX.md) record table, [data-model.md](./data-model.md) |
| A descriptor change | [configuration.md](./configuration.md), [api-reference/INDEX.md](./api-reference/INDEX.md), [invariants.md](./invariants.md) |
| A workflow change | [build-and-deployment.md](./build-and-deployment.md), [flows/release-and-publishing.md](./flows/release-and-publishing.md), [dependencies.md](./dependencies.md) |
| A new release | [build-and-deployment.md](./build-and-deployment.md) tag or asset examples, [dependencies.md](./dependencies.md) version table |
| A host version change | [configuration.md](./configuration.md), [dependencies.md](./dependencies.md), [testing.md](./testing.md), [operations/ambiguities-and-open-questions.md](./operations/ambiguities-and-open-questions.md) |
| A root policy document change | [INDEX.md](./INDEX.md) root document map, and the referencing document |
| A defect corrected | [operations/ambiguities-and-open-questions.md](./operations/ambiguities-and-open-questions.md), [operations/troubleshooting.md](./operations/troubleshooting.md), [invariants.md](./invariants.md) |
| A design decision reversed | [design-decisions.md](./design-decisions.md), and whatever depended on it |

## Review cadence

| Review | Frequency | Scope |
|--------|-----------|-------|
| Post-change | Every pull request that changes data or metadata | Only the documents named by the trigger |
| Full review | Quarterly, or after a host version change | Every document, against the checklist in [INDEX.md](./INDEX.md) |
| Defect sweep | When a defect is reported | [operations/ambiguities-and-open-questions.md](./operations/ambiguities-and-open-questions.md) |

A host version change warrants a full review rather than a targeted one, because the schema, the keys, and the reference templates may all shift together.

## Style conventions

| Convention | Rule |
|------------|------|
| File names | kebab-case |
| Directory names | kebab-case, plural for collections |
| Headings | Sentence case |
| Cross-references | Repository-relative Markdown links |
| Code symbols and paths | Backticks |
| Verification statements | Name the method, for example "verified by diff against the installed host file" |
| Inferences | Explicitly labelled, for example "inferred, not verified" |
| Absence | Stated explicitly rather than left blank, because many readers arrive asking whether a mechanism exists |

## Evidence discipline

Every factual statement in these documents should be traceable to one of:

1. A file in this repository.
2. A git tag, commit, or release asset.
3. The installed host game data.
4. The platform metadata of a distribution channel.

Where a statement rests on an assumption, it is labelled and usually also recorded in [operations/ambiguities-and-open-questions.md](./operations/ambiguities-and-open-questions.md). Do not remove that labelling during an editorial pass, because the labelling is what lets a future agent decide how much to trust a claim.

## Staleness indicators

A document is likely stale if any of these is true:

- It names a count that the data files contradict.
- It references a file that no longer exists.
- It describes a workflow step that the workflow no longer contains.
- It asserts the absence of a mechanism that now exists.
- It cites a host path that the host has moved.

## Adding a document

1. Confirm that no existing document covers the subject.
2. Place it under the correct directory, or at the docs root for a cross-cutting topic.
3. Follow the naming and style conventions above.
4. Link it from [INDEX.md](./INDEX.md), in the catalogue and in any relevant navigation aid.
5. Link it from at least one adjacent document so that it is reachable by traversal and not only by the index.

## Removing a document

1. Confirm that no other document links to it, or remove the links.
2. Remove its catalogue entry from [INDEX.md](./INDEX.md).
3. Move any still-relevant analysis into the surviving document rather than losing it.

## Related documentation

- [INDEX.md](./INDEX.md) — the catalogue and review checklist.
- [operations/ambiguities-and-open-questions.md](./operations/ambiguities-and-open-questions.md) — the open evidence gaps.
