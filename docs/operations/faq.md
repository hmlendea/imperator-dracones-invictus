# Frequently asked questions

## About the mod

### What does this mod change?

It expands the Dacian culture group with additional male and female names, and it renames the Zalmoxian faith to Zamolxian in English display text. It also revises the English text of the Zalmoxis treasure.

### Does it change game mechanics?

No. The composition references, the levy template, the graphical culture, the ethnicity weighting, and every culture record are unchanged from the host. The changes are additive names plus display text.

### Does it work with saved games?

Yes, by design. The mod preserves host identifiers, so a save made without the mod loads with it, and the reverse holds. Only display text and the character name pool differ. See DD-3 in [design-decisions.md](../design-decisions.md).

### Does it work in Ironman mode?

The descriptor declares the `Ironman Compatible` tag, so the mod presents itself as compatible. That declaration is metadata that the launcher reads; the mod itself contains no rule that would affect Ironman eligibility.

### Which game version does it target?

Imperator: Rome 2.0.x, as declared by `supported_version="2.0.*"` in both descriptors.

## About contributing

### What do I need to install to contribute?

A text editor that can save UTF-8 with a byte order mark, and an Imperator: Rome 2.0.x installation for testing. Nothing else. See [operations/quick-start.md](./quick-start.md).

### Can I add a name without rewriting the whole file?

No. The host treats a culture group as one definition, so the mod file must remain a complete group declaration. Add your token inside the existing block. See DD-1 in [design-decisions.md](../design-decisions.md).

### Where do I add a name?

In `dracones-invictus/common/cultures/00_dacian.txt`, inside `male_names`, `female_names`, or `family`, after the `# Dracones Invictus - Dacian` comment. See [operations/change-guide.md](./change-guide.md#adding-or-revising-names).

### How do I change the faith text?

Edit the value in `dracones-invictus/localization/english/zzz_DI_l_english.yml`, keeping the key identical to the host key. See [operations/change-guide.md](./change-guide.md#revising-localisation-text).

### Why did my text change have no effect?

Almost always because the key does not match the host key exactly. Last-wins matching is exact, so a near-match defines a new unused key. See [operations/troubleshooting.md](./troubleshooting.md#the-faith-or-conversion-text-does-not-change).

### Are there tests I can run?

No. The repository has no test suite, because it has no execution surface. Verification is manual and is documented in [testing.md](../testing.md).

### Can I add support for another language?

It is possible but out of the current scope. It would mean adding a directory such as `localization/german/`, with records for the same keys. See DD-8 in [design-decisions.md](../design-decisions.md).

## About the repository

### Why are there two descriptor files?

The host has no include mechanism, so the launcher-visible descriptor at the repository root and the in-package descriptor duplicate four values. See DD-4 in [design-decisions.md](../design-decisions.md).

### Why is the culture file named `00_dacian.txt`?

The name mirrors the host file of the same relative path, which is what makes the override resolve. See [architecture.md](../architecture.md).

### Why does the localisation file start with `zzz_`?

To sort after every host localisation file, so that the mod's records win the last-wins key resolution. See [concurrency-and-scheduling.md](../concurrency-and-scheduling.md).

### Why is there a `.yml` in one of the keys?

That is a defect, not a convention. The treasure name key contains a stray `.yml` fragment, which prevents it from superseding the host key. See [operations/troubleshooting.md](./troubleshooting.md#the-treasure-display-name-does-not-change).

### Is there an API or a library?

No. The mod is data consumed by the game. The data contracts are documented in [api-reference/INDEX.md](../api-reference/INDEX.md) as the reference for future agents.

## About the release

### How is a release published?

A maintainer pushes a `v*.*.*` tag and publishes the GitHub release. The `github-release.yml` workflow builds the versioned archive and attaches it to the release, and the `steam-workshop.yml` workflow downloads that archive and uploads it to the Steam Workshop. See [flows/release-and-publishing.md](../flows/release-and-publishing.md).

### Why did the release fail?

The most common causes are a missing or misnamed release asset, a tag outside the pattern, or expired Steam secrets. See [operations/troubleshooting.md](./troubleshooting.md#publishing-failures).

### Can I roll back a release?

No rollback procedure exists. A bad release is corrected by publishing another release. See [build-and-deployment.md](../build-and-deployment.md).

## Related documentation

- [repository-overview.md](../repository-overview.md) — scope and key facts.
- [operations/ambiguities-and-open-questions.md](./ambiguities-and-open-questions.md) — where the evidence is incomplete.
