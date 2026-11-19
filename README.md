[![Donate](https://img.shields.io/badge/-%E2%99%A5%20Donate-%23ff69b4)](https://hmlendea.go.ro/funding) [![Latest GitHub release](https://img.shields.io/github/v/release/hmlendea/imperator-dracones-invictus)](https://github.com/hmlendea/imperator-dracones-invictus/releases/latest) [![Build Status](https://github.com/hmlendea/imperator-dracones-invictus/actions/workflows/github-release.yml/badge.svg)](https://github.com/hmlendea/imperator-dracones-invictus/actions/workflows/github-release.yml)

# About

This is a mod for Imperator Rome that brings more flavour and changes to the dacian cultures.

## 📑 Table of Contents

- [About](#about)
- [Useful links](#useful-links)
- [Capabilities](#capabilities)
- [Usage](#usage)
- [Installation](#installation)
  - [Manual installation](#manual-installation)
- [GitHub Actions](#github-actions)
- [Architecture](#architecture)
- [Documentation](#documentation)
- [Contributing](#contributing)
- [Code of Conduct](#code-of-conduct)
- [Privacy and Data](#privacy-and-data)
- [Security](#security)

# Useful links

- [GitHub page](https://github.com/hmlendea/imperator-dracones-invictus)
- [Nexus Mods page](https://nexusmods.com/imperatorrome/mods/24)
- [Steam Workshop page](https://steamcommunity.com/sharedfiles/filedetails/?id=1745923673)

## ✨ Capabilities

 - Many new dacian male and female names
 - Renamed Zalmoxis/Zalmoxianism to Zamolxis/Zamolxianism
 - Some distinct flavour descriptions here and there (WIP)

## 🚀 Usage

Enable the mod in the Imperator: Rome launcher and start the game. The expanded Dacian name pool applies to newly generated characters, and the revised Zamolxian faith and treasure text appears wherever the game displays those records.

# Installation

[![Get it from the Workshop](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/steam-workshop.png)](https://steamcommunity.com/sharedfiles/filedetails/?id=1745923673) [![Get it from GitHub](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/github.png)](https://github.com/hmlendea/imperator-dracones-invictus/releases) [![Get it from the Nexus](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/nexus.png)](https://nexusmods.com/imperatorrome/mods/24)

## Manual installation

 - Download the [latest release](https://github.com/hmlendea/imperator-dracones-invictus/releases/latest)
 - Extract the contents into your game's mod directory (delete the old files when updating)

## ⚙️ GitHub Actions

| Workflow | Purpose | What it does |
|----------|---------|--------------|
| [ github-release.yml ](https://github.com/hmlendea/imperator-dracones-invictus/blob/master/.github/workflows/github-release.yml) | GitHub Release | Runs on a published release or manual dispatch: checks out the released tag, zips `dracones-invictus/` and `dracones-invictus.mod` into `dracones-invictus_<version>.zip`, and attaches the archive to the GitHub release |
| [ nexus-mods-release.yml ](https://github.com/hmlendea/imperator-dracones-invictus/blob/master/.github/workflows/nexus-mods-release.yml) | Release to Nexus Mods | Runs on a `v*.*.*` tag push or manual dispatch: checks out the tag, zips `dracones-invictus/` and `dracones-invictus.mod` into `release.zip`, and uploads it to Nexus Mods file `526528` as `dracones-invictus.zip` |
| [ steam-workshop-release.yml ](https://github.com/hmlendea/imperator-dracones-invictus/blob/master/.github/workflows/steam-workshop-release.yml) | Release to Steam Workshop | Runs on a `v*.*.*` tag push or manual dispatch: checks out the tag, zips `dracones-invictus/` and `dracones-invictus.mod` into `release.zip`, extracts it, and uploads the content directory to Workshop item `1745923673` |

## 🏗️ Architecture

Dracones Invictus is a static data-only mod that overrides the Imperator: Rome Dacian culture group and selected Zamolxian localisation keys through the game's native mod loader. It contains no executable code, runtime, scripting, or network communication.

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

## 🛡️ Privacy and Data

Dracones Invictus does not collect, process, transmit, or persist any personal data. The mod consists solely of static Dacian culture definitions and English localisation text that the Imperator: Rome game engine reads locally.

For the detailed description of how the application handles privacy and personal data, see the [Privacy Statement](./PRIVACY.md).

The mod performs no network requests and contains no analytics, diagnostics, telemetry, or update checks. All mod data remains in the local game mod directory under your direct control.

## 🔒 Security

Dracones Invictus is a static data-only mod for Imperator: Rome with no executable code, runtime, or network communication, so its security surface is limited to the integrity of the distributed mod files.

For information on reporting security vulnerabilities, see the [Security Policy](./SECURITY.md).
