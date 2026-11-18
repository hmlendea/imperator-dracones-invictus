# Concurrency and scheduling

## Position

The mod has no concurrency surface. It contains no threads, no asynchronous work, no scheduler, no job queue, no timer, and no parallel execution. This page records that fact, explains why no concurrency concern can arise in this repository, and identifies the only place where temporal ordering matters at all.

## Concurrency inventory

| Mechanism | Present? | Reason |
|-----------|----------|--------|
| Threads | No | No executable content exists |
| Async or await | No | No executable content exists |
| Parallel processing | No | No executable content exists |
| Schedulers or cron | No | The release workflows trigger on release publication and tag push, which are event-driven rather than scheduled |
| Locks or mutexes | No | No shared mutable state exists |
| Worker pools | No | No work is dispatched |
| Timers or debounce | No | No runtime exists |
| Background jobs | No | The workflow runs as a single job with sequential steps |

## Why no concurrency concern can arise

The mod is a set of static files. Every file is read at a point chosen by the host, and the host decides whether any reading is concurrent. Whatever concurrency the host applies to culture and localisation loading is a host concern with a host-owned consistency model, and it is not influenced by anything in this repository.

The mod therefore cannot introduce a race, a deadlock, a starvation condition, a torn read, or a visibility problem. It also cannot defend against one.

## The one ordering concern

Filename ordering is the only temporal consideration in the repository, and it is a *resolution order*, not an execution order.

| Ordering | Mechanism | Consequence |
|----------|-----------|-------------|
| Localisation record resolution | Filename sort order across the localisation directory | Later files win a key; the `zzz_DI_` prefix provides the margin |
| Culture file resolution | Path identity rather than ordering | The prefix `00_` aligns with host layout but does not decide resolution |
| Release steps | Sequential steps in each workflow job | In the Workshop workflow, the download step must complete before the upload step |

Only the first row is genuinely order-sensitive in a way that can silently change behaviour. It is recorded as an invariant in [invariants.md](./invariants.md) and as a risk in [architecture.md](./architecture.md).

## Workflow concurrency

| Aspect | Detail |
|--------|--------|
| Job count | One |
| Step execution | Sequential within the job |
| Concurrent runs | Possible if two tags are pushed near-simultaneously |
| Effect of concurrent runs | Each run publishes its own version; the later upload determines the final item content |
| Guidance | Push one release tag at a time, and confirm the run completes before pushing the next |

This is the only place where two independent executions can influence a shared resource, and the shared resource is the Workshop item rather than any repository state.

## Resource use

No runtime resource is consumed. Storage consumption is a few kilobytes of text and one thumbnail. There is no memory growth, no connection pool, no buffer, and no cache to evict.

## Capacity limits

| Limit | Value | Source |
|-------|-------|--------|
| Name pool size | Not documented by the host; the current pools are 125 and 18 tokens | Repository data |
| Localisation file size | Not documented by the host; the current file is seven lines | Repository data |
| Description length for the Workshop change note | Imposed by the platform, not by the repository | Platform |

No repository-imposed limit exists.

## Related documentation

- [architecture.md](./architecture.md) — the resolution order analysis.
- [invariants.md](./invariants.md) — the precedence margin invariant.
- [flows/release-and-publishing.md](./flows/release-and-publishing.md) — the sequential publication steps.
