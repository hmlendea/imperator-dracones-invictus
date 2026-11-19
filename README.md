[![Donate](https://img.shields.io/badge/-%E2%99%A5%20Donate-%23ff69b4)](https://hmlendea.go.ro/funding)
[![Latest Release](https://img.shields.io/github/v/release/hmlendea/imperator-dracones-invictus)](https://github.com/hmlendea/imperator-dracones-invictus/releases/latest)
[![License](https://img.shields.io/github/license/hmlendea/imperator-dracones-invictus)](https://github.com/hmlendea/imperator-dracones-invictus/blob/master/LICENSE)

# Dracones Invictus

Dracones Invictus is a content-only mod for Imperator: Rome that expands the Dacian culture group with additional male and female names and revises the English text of the Zamolxian faith and the Zamolxis treasure.

## 📑 Table of Contents

- [Capabilities](#capabilities)
- [Use Cases](#use-cases)
- [Usage](#usage)
- [Known Limitations](#known-limitations)
- [Installation](#installation)
  - [Manual Installation](#manual-installation)
- [Localisation](#localisation)
- [Compatibility](#compatibility)
- [Privacy and Data](#privacy-and-data)
- [GitHub Actions](#github-actions)
- [Project Structure](#project-structure)
  - [Directories](#directories)
- [Architecture](#architecture)
- [Documentation](#documentation)
- [Versioning](#versioning)
- [Troubleshooting](#troubleshooting)
- [FAQ](#faq)
- [Contributing](#contributing)
- [Code of Conduct](#code-of-conduct)
- [Security](#security)
- [Support](#support)
- [Project Engagement](#project-engagement)
- [License](#license)

## ✨ Capabilities

- Supplies 79 male and 5 female Dacian names in addition to the vanilla pools from which the game draws recently generated characters
- Presents the Zalmoxian faith as Zamolxian throughout the English display text, including the conversion decision
- Revises the English description of the Zamolxis treasure
- Preserves every vanilla name token, culture identifier, and key, so existing saved games continue to load

## 🎯 Use Cases

- **Dacian playthroughs:** broadens the variety of names the game assigns to recently generated Dacian characters without altering any game mechanic
- **Corrected faith and treasure text:** displays the Zamolxian faith and the Zamolxis treasure under the spelling that the mod adopts

## 🚀 Usage

Activate the mod in the Imperator: Rome launcher and launch the game. The expanded Dacian name pool applies to recently generated characters, and the revised text appears wherever the game displays the affected records.

## ⚠️ Known Limitations

- The treasure name key contains a stray `.yml` fragment, so that record does not supersede the vanilla key and the treasure keeps its original display name
- The culture file supersedes the whole vanilla culture group definition rather than patching individual fields, so any field omitted from the mod file is lost for the group
- Only English localisation is supplied; every other language continues to display vanilla text

## 📦 Installation

[![Obtain it from Nexus Mods](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/nexus.png)](https://nexusmods.com/imperatorrome/mods/24)
[![Obtain it from Steam Workshop](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/steam-workshop.png)](https://steamcommunity.com/sharedfiles/filedetails/?id=1745923673)
[![Obtain it from GitHub](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/github.png)](https://github.com/hmlendea/imperator-dracones-invictus/releases)

### Manual Installation

- Download the [latest release](https://github.com/hmlendea/imperator-dracones-invictus/releases/latest)
- Extract the contents into the game's mod directory, deleting the previous files first

## 🌍 Localisation

The mod supplies English text for the localisation keys that it supersedes. Every other record, and every other language, resolves to the vanilla game text.

| Language | Locale | Status |
|----------|--------|--------|
| English | `en` | Supplied for the superseded keys |

## 🧩 Compatibility

| Component | Supported Versions | Notes |
|-----------|--------------------|-------|
| Imperator: Rome | 2.0.x | Required host; declared by `supported_version="2.0.*"` in both descriptors |
| Saved games | Existing saves | Only the name pool and the display text change; host identifiers are preserved |

## 🛡️ Privacy and Data

Dracones Invictus manages no personal data. The mod consists solely of static culture definitions and English localisation text that the Imperator: Rome game engine reads locally, and it performs no network requests.

For the detailed description of how the application handles privacy and personal data, see the [Privacy Statement](./PRIVACY.md).

## ⚙️ GitHub Actions

| Workflow | Purpose | What it does |
|----------|---------|--------------|
| [ github-release.yml ](https://github.com/hmlendea/imperator-dracones-invictus/blob/master/.github/workflows/github-release.yml) | GitHub Release | Runs on a published release or manual dispatch: checks out the released tag, packages `dracones-invictus/` and `dracones-invictus.mod` into `dracones-invictus_<version>.zip`, and attaches the archive to the GitHub release |
| [ nexus-mods-release.yml ](https://github.com/hmlendea/imperator-dracones-invictus/blob/master/.github/workflows/nexus-mods-release.yml) | Release to Nexus Mods | Runs on a `v*.*.*` tag push or manual dispatch: checks out the tag, packages the mod into `release.zip`, and uploads it to Nexus Mods file `526528` as `dracones-invictus.zip` |
| [ steam-workshop-release.yml ](https://github.com/hmlendea/imperator-dracones-invictus/blob/master/.github/workflows/steam-workshop-release.yml) | Release to Steam Workshop | Runs on a `v*.*.*` tag push or manual dispatch: checks out the tag, packages the mod, extracts the archive, and uploads the content directory to Workshop item `1745923673` |

## 🗂️ Project Structure

The repository holds one content package, its documentation, and its release automation.

### Directories

| Directory | Purpose |
|-----------|---------|
| `dracones-invictus/` | The mountable mod package; its layout mirrors the vanilla game paths that it supersedes |
| `dracones-invictus/common/cultures/` | The `dacia_group` culture group definition |
| `dracones-invictus/localization/english/` | The English localisation overrides |
| `.github/workflows/` | Release automation |
| `docs/` | Maintainer documentation |

## 🏗️ Architecture

Dracones Invictus is a static data-only mod that supersedes the Dacian culture group and selected English localisation keys through the game's native mod loader. It contains no executable code, runtime, scripting, or network communication.

See the [Architecture Documentation](./ARCHITECTURE.md) for the system context, principal components, runtime processes, ownership boundaries, dependencies, constraints, and extension points.

## 📚 Documentation

Comprehensive documentation for maintainers and contributors is available in the [`docs/`](./docs/) directory:

| Resource | Description |
|----------|-------------|
| [Documentation Index](./docs/INDEX.md) | Master catalogue, navigation aids, and coverage status |
| [Data Contracts](./docs/api-reference/INDEX.md) | Culture group, descriptor, and localisation schemas, and override precedence |
| [Quick Start](./docs/operations/quick-start.md) | Onboarding for a new contributor |
| [Architecture Deep Dive](./docs/architecture.md) | The two override mechanisms and their consequences |
| [Change Guide](./docs/operations/change-guide.md) | Safe procedures for modifying names, localisation, and metadata |
| [Repository Structure](./docs/repository-structure.md) | File inventory, tree semantics, and naming conventions |
| [Build and Deployment](./docs/build-and-deployment.md) | Packaging, tagging, and publishing pipeline |
| [Invariants](./docs/invariants.md) | Properties that must hold across every release |
| [Troubleshooting](./docs/operations/troubleshooting.md) | Diagnosing load and publishing failures |
| [FAQ](./docs/operations/faq.md) | Frequently asked questions |

## 🏷️ Versioning

Mod releases are published as GitHub tags of the form `v<major>.<minor>.<patch>`, and the current version is declared in `dracones-invictus.mod` and `dracones-invictus/descriptor.mod`. Only the latest release is supported; preceding versions receive no additional maintenance.

## 🩺 Troubleshooting

See [Troubleshooting Guide](./docs/operations/troubleshooting.md) for diagnostic procedures and resolutions to recurring problems.

## ❓ FAQ

See the [FAQ](./docs/operations/faq.md) for responses to frequently raised inquiries.

## 🤝 Contributing

You are welcome to submit any suggestion, feedback, or modification to this project.

When doing so, please:
- Maintain cross-platform compatibility
- Submit focused pull requests that conform to the existing code style
- Maintain your branch synchronised with `master`
- Revise the documentation when functionality changes
- Raise a new [issue](https://github.com/hmlendea/imperator-dracones-invictus/issues) for problems or suggestions

See the [Contributing Guidelines](./CONTRIBUTING.md) for details on how to report issues, suggest enhancements, and submit changes.

## 📜 Code of Conduct

See the [Code of Conduct](./CODE_OF_CONDUCT.md) for the standards of conduct expected in all project spaces.

## 🔒 Security

Dracones Invictus is a static data-only mod for Imperator: Rome with no executable code, runtime, or network communication, so its security surface is limited to the integrity of the distributed mod files.

For information on reporting security vulnerabilities, see the [Security Policy](./SECURITY.md).

## 💬 Support

For support, please [open an issue](https://github.com/hmlendea/imperator-dracones-invictus/issues).

## 💝 Project Engagement

Discovered a problem with Dracones Invictus or have a suggestion for it? [Open an issue](https://github.com/hmlendea/imperator-dracones-invictus/issues)!

If you find this project useful, consider [funding it](https://hmlendea.go.ro/funding) or starring ⭐️ it on GitHub, or giving it a thumbs up and adding it as a favourite on the [Steam Workshop](https://steamcommunity.com/sharedfiles/filedetails/?id=1745923673)!

[![Donate](https://raw.githubusercontent.com/hmlendea/readme-assets/master/donate_generic.png)](https://hmlendea.go.ro/funding)

## 📄 License

This project is being distributed under the `GNU General Public License v3.0`.
See [LICENSE](./LICENSE) for additional information.
