# GhostPak

> **Experimental Conceptual Project**

[![Built with Rust](https://img.shields.io/badge/Built%20with-Rust-b7410e?style=flat-square&logo=rust&logoColor=white)](https://www.rust-lang.org/)
[![Platform](https://img.shields.io/badge/Platform-Linux-000000?style=flat-square&logo=linux&logoColor=white)](https://www.linux.org/)
[![Wine Compatible](https://img.shields.io/badge/Wine-Compatible-722F37?style=flat-square&logo=wine&logoColor=white)](https://www.winehq.org/)

A TOML-driven system for running Windows applications on Linux using Wine/Proton in a structured, isolated, and GPU-aware environment.

## About

GhostPak manages Windows applications on Linux through declarative TOML manifests. Each app runs in a dedicated environment with its own prefix and configuration, with optional container/namespace isolation.

This project draws inspiration from [Winboat](https://github.com/nicholasgcoles/winboat), a concept we found interesting. GhostPak aims to explore a similar idea—portable, self-contained Windows application packaging for Linux—but built from the ground up with different tools and frameworks. Our approach centers on Rust, TOML-driven manifests, and a modular architecture separating concerns into core libraries, a daemon, CLI, and GUI components.

## Features

- **TOML Manifests** - Declarative configuration for runtimes and applications
- **Runtime Management** - Versioned Wine/Proton stacks with DXVK/VKD3D support
- **App Isolation** - Dedicated prefixes and optional container isolation per app
- **GPU-Aware** - Discrete GPU preference, DXVK/VKD3D integration
- **Desktop Integration** - Automatic `.desktop` file and MIME type generation
- **Modular Architecture** - Daemon, CLI (`gpak`), and GUI components

## Architecture

```
ghostpak/
├── ghostpak-core/    # Shared manifests, models, utilities
├── ghostpak-daemon/  # Background service for app management
├── ghostpak-cli/     # Command-line interface (gpak)
└── ghostpak-gui/     # GhostPak Manager (egui + wgpu)
```

## CLI Usage

```bash
# Runtime management
gpak runtime list
gpak runtime add <manifest.toml>
gpak runtime info <runtime-id>

# App management
gpak app install <manifest.toml>
gpak app list
gpak app run <app-id>
gpak app remove <app-id>
gpak app logs <app-id>

# Diagnostics
gpak diag
```

## Manifest Examples

**Runtime Manifest:**
```toml
spec_version = "1.0.0"
id = "gpak-runtime-gaming-1"
name = "GhostPak Gaming Runtime"
kind = "runtime"
version = "1.0.0"
arch = "win64"

[runtime]
proton_build = "GE-Proton9-10"

[runtime.dxvk]
enabled = true
version = "2.4"
```

**App Manifest:**
```toml
spec_version = "1.0.0"
id = "com.example.app"
name = "Example Application"
kind = "app"
runtime = "gpak-runtime-gaming-1"
arch = "win64"

[installer]
type = "exe"
url = "https://example.com/setup.exe"

[gpu]
acceleration = true
prefer_discrete = true
dxvk = true

[integration.desktop]
enabled = true
name = "Example App"
categories = ["Game"]
```

## Directory Structure

```
~/.config/ghostpak/
└── ghostpak.toml           # Global configuration

~/.local/share/ghostpak/
├── runtimes/               # Wine/Proton runtime payloads
├── apps/
│   └── <app-id>/
│       ├── manifest.toml
│       ├── prefix/         # WINEPREFIX
│       └── logs/
└── logs/                   # Daemon and CLI logs
```

## Building

```bash
cargo build --release
```

## Target Platforms

- Arch Linux
- Fedora / Nobara
- Pop!_OS
- Other systemd-based distributions with Wayland or X11

## Status

This project is in early development. See `TODO.md` for the current roadmap.

## License

MIT
