# Documentation Index

> **Note**: This project is a **data-only Imperator: Rome mod** with no public API, no HTTP endpoints, no library interface, and no programmatic extension points. This document serves as a reference for the *data contracts* that constitute the mod's integration surface with the game engine.

## Root Document Map

| Document | Purpose | Link |
|----------|---------|------|
| README.md | Project overview, features, installation channels | [README.md](../README.md) |
| ARCHITECTURE.md | System context, components, runtime process, contracts | [ARCHITECTURE.md](../ARCHITECTURE.md) |
| CONTRIBUTING.md | Contribution procedure, prerequisites, data conventions | [CONTRIBUTING.md](../CONTRIBUTING.md) |
| CODE_OF_CONDUCT.md | Conduct expected in project spaces | [CODE_OF_CONDUCT.md](../CODE_OF_CONDUCT.md) |
| PRIVACY.md | Data-handling position (none) | [PRIVACY.md](../PRIVACY.md) |
| SECURITY.md | Supported versions, report scope, disclosure policy | [SECURITY.md](../SECURITY.md) |
| LICENSE | GNU General Public License v3.0 | [LICENSE](../LICENSE) |
| dracones-invictus.mod | Launcher-visible mod descriptor | [dracones-invictus.mod](../dracones-invictus.mod) |
| .github/workflows/github-release.yml | Release automation (GitHub release archive) | [github-release.yml](../.github/workflows/github-release.yml) |
| .github/workflows/steam-workshop.yml | Release automation (Steam Workshop upload) | [steam-workshop.yml](../.github/workflows/steam-workshop.yml) |
| .github/FUNDING.yml | Funding platform configuration | [FUNDING.yml](../.github/FUNDING.yml) |

## Documentation Catalogue

### Data Contracts (this directory)

| Document | Description |
|----------|-------------|
| [api-reference/INDEX.md](./api-reference/INDEX.md) | Data-contract reference: culture group schema, descriptor schema, localisation schema, override precedence |

### Core Understanding

| Document | Description |
|----------|-------------|
| [repository-overview.md](./repository-overview.md) | Repository purpose, scope, key facts, and entry points |
| [repository-structure.md](./repository-structure.md) | Directory tree, file inventory, naming conventions |
| [architecture.md](./architecture.md) | Architectural elaboration complementing root ARCHITECTURE.md |
| [data-model.md](./data-model.md) | Culture group entities, name lists, localisation records, descriptor fields |

### Flows

| Document | Description |
|----------|-------------|
| [flows/startup-and-mounting.md](./flows/startup-and-mounting.md) | Game launch, descriptor validation, virtual filesystem mounting, data resolution order |
| [flows/release-and-publishing.md](./flows/release-and-publishing.md) | Release publication and tag push through archive attachment and retrieval to Workshop upload |

### Operations

| Document | Description |
|----------|-------------|
| [operations/quick-start.md](./operations/quick-start.md) | Onboarding for a new contributor |
| [operations/change-guide.md](./operations/change-guide.md) | How to modify names, localisation, or metadata safely |
| [operations/troubleshooting.md](./operations/troubleshooting.md) | Diagnosing load failures and publishing failures |
| [operations/faq.md](./operations/faq.md) | Frequently asked questions |
| [operations/ambiguities-and-open-questions.md](./operations/ambiguities-and-open-questions.md) | Unverified areas, known gaps, and investigation leads |

### Cross-Cutting Concerns

| Document | Description |
|----------|-------------|
| [build-and-deployment.md](./build-and-deployment.md) | Packaging, tagging, and publishing pipeline |
| [configuration.md](./configuration.md) | Descriptor configuration areas and load-order prefixes |
| [dependencies.md](./dependencies.md) | Host schema, tooling, and platform dependencies |
| [testing.md](./testing.md) | Manual validation checklist and coverage gaps |
| [design-decisions.md](./design-decisions.md) | Verified design choices and their consequences |
| [state-and-persistence.md](./state-and-persistence.md) | Absence of owned state; lifecycle of distributed artefacts |
| [error-handling.md](./error-handling.md) | Failure modes owned by the host, not by the mod |
| [logging.md](./logging.md) | Observability position (none in the mod) |
| [security.md](./security.md) | Implementation-specific security elaboration |
| [invariants.md](./invariants.md) | Properties that must hold across every release |
| [integrations.md](./integrations.md) | External systems: game loader, Workshop, Nexus Mods, GitHub |
| [concurrency-and-scheduling.md](./concurrency-and-scheduling.md) | Concurrency surface (none) and engine-side evaluation |
| [documentation-maintenance.md](./documentation-maintenance.md) | Update triggers, review cadence, and ownership |

## Navigation Aids

### Start Here (New Contributor)

1. [repository-overview.md](./repository-overview.md) — What this is
2. [operations/quick-start.md](./operations/quick-start.md) — Onboarding
3. [repository-structure.md](./repository-structure.md) — Where things are
4. [operations/change-guide.md](./operations/change-guide.md) — How to make a change

### Deep Dive (Maintainer)

1. [architecture.md](./architecture.md) — Architectural elaboration
2. [api-reference/INDEX.md](./api-reference/INDEX.md) — Data contracts
3. [flows/startup-and-mounting.md](./flows/startup-and-mounting.md) — Resolution order
4. [build-and-deployment.md](./build-and-deployment.md) — Release process
5. [invariants.md](./invariants.md) — What must never break
6. [design-decisions.md](./design-decisions.md) — Why the content is shaped as it is

### Flows (Process Reference)

| Flow | Document |
|------|----------|
| Game loads the mod | [flows/startup-and-mounting.md](./flows/startup-and-mounting.md) |
| Release is published | [flows/release-and-publishing.md](./flows/release-and-publishing.md) |
| Add or revise names | [operations/change-guide.md](./operations/change-guide.md#adding-or-revising-names) |
| Revise localisation text | [operations/change-guide.md](./operations/change-guide.md#revising-localisation-text) |
| Diagnose a load failure | [operations/troubleshooting.md](./operations/troubleshooting.md#load-failures) |

## Maintenance Metadata

| Field | Value |
|-------|-------|
| Last full review | 2026-11-18 |
| Owner | Maintainer |
| Coverage status | Complete for the current repository scope (data-only mod) |
| Verified host reference | Imperator: Rome 2.0.x installation on the maintainer machine |
| Material gaps | The mod data has not been diff-verified against a clean upstream game install in CI, and the Steam Workshop item page has not been inspected |

### Review Checklist (Per Review)

- [ ] Cross-references resolve
- [ ] No stale content (game version, tool versions, tags)
- [ ] Root document changes reflected
- [ ] New ambiguities captured
- [ ] Invariants still hold
- [ ] Manual validation checklist in [testing.md](./testing.md) still accurate
