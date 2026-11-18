# Logging

## Position

The mod emits no log entries. It contains no executable content, no runtime, and no diagnostics of its own. This page records that fact and points maintainers to the only surfaces where observable output exists.

## Framework and levels

Not applicable. No logging framework is present, no levels are defined, and no configuration is possible.

## Structured fields

Not applicable. No log entry exists, so there are no structured fields, no request identifiers, no correlation identifiers, and no timestamp formats.

## Destinations

There is no mod-side sink.

| Surface | Owner | Content |
|---------|-------|---------|
| Host game output | Host engine | Host-generated messages, which may include mount warnings for a descriptor issue |
| Launcher output | Host launcher | Host-generated messages, which may include version warnings |
| GitHub Actions run output | GitHub | Workflow step results for `Prepare` and `Upload` |
| Steam Workshop item history | Steam | Publication records and change notes |

## Correlation

Not applicable. No two mod-owned events can be correlated with each other, because there is one mod-owned event surface: the workflow run. Within that surface, correlation is by run identifier, which GitHub assigns.

## Redaction and secrets

The workflow consumes three secrets, `STEAM_USERNAME`, `STEAM_PASSWORD`, and `STEAM_2FASEED`, as environment variables. The repository contains no log, cache, or artefact of those values, and no workflow step writes them to output. Any diagnostic that would expose them must not be added. See [security.md](./security.md) and [../../SECURITY.md](../SECURITY.md).

## Retention

Not applicable on the mod side. Workflow logs are retained under the repository's GitHub retention policy, which is not controlled by this repository.

## Performance considerations

Not applicable. The mod contributes no logging overhead.

## Audit trail

The mod has no audit trail of its own. The closest equivalent is the combination of the git history, the release tags, the workflow runs, and the Workshop item history, which together reconstruct what was published and when. That reconstruction is manual.

## Related documentation

- [integrations.md](./integrations.md) — the external surfaces that do produce output.
- [operations/troubleshooting.md](./operations/troubleshooting.md) — where to look when something is wrong.
- [build-and-deployment.md](./build-and-deployment.md) — the workflow run record.
