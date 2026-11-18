# Quick start

## What this repository is

A content-only mod for Imperator: Rome 2.0.x that adds Dacian names and renames the Zalmoxian faith to Zamolxian in text. It contains no code.

## Prerequisites

| Requirement | Purpose |
|-------------|---------|
| A text editor that preserves UTF-8 with a byte order mark, or an editor configured to write one | Localisation files must keep the byte order mark |
| An Imperator: Rome 2.0.x installation | To diff against host data and to test in game |
| Git | To clone the repository and to open a pull request |

No SDK, runtime, package manager, or compiler is required.

## Setup

```bash
git clone https://github.com/hmlendea/imperator-dracones-invictus.git
cd imperator-dracones-invictus
```

There is nothing to install.

## Orientation in two minutes

| Question | Answer |
|----------|--------|
| Where are the names? | `dracones-invictus/common/cultures/00_dacian.txt` |
| Where is the text? | `dracones-invictus/localization/english/zzz_DI_l_english.yml` |
| Where is the metadata? | `dracones-invictus.mod` and `dracones-invictus/descriptor.mod` |
| What is the entry point? | The descriptor, read by the launcher |
| What is the biggest trap? | The culture file replaces the whole host file rather than patching it |

## First task: add a male name

1. Open `dracones-invictus/common/cultures/00_dacian.txt`.
2. Find the `male_names` block.
3. Locate the `# Dracones Invictus - Dacian` comment.
4. Append the name to one of the token lines in that mod section, spaced from its neighbours as the surrounding tokens are.
5. Save the file, preserving the tab indentation used throughout.
6. Verify with the structural checklist in [testing.md](../testing.md).
7. Load the game and confirm the name appears in the Dacian pool.

The detailed procedure, with the reasoning and the checks, is in [operations/change-guide.md](./change-guide.md#adding-or-revising-names).

## Read next

1. [repository-overview.md](../repository-overview.md) — what the project is and is not.
2. [repository-structure.md](../repository-structure.md) — the file inventory.
3. [architecture.md](../architecture.md) — the two override mechanisms.
4. [operations/change-guide.md](./change-guide.md) — the safe edit procedures.
5. [invariants.md](../invariants.md) — what must never break.
