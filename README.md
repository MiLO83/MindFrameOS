# DreamFrameOS

[![WebXR](https://img.shields.io/badge/WebXR-Quest%20Browser-4b8cff)](https://immersiveweb.dev/)
[![NVIDIA](https://img.shields.io/badge/NVIDIA-CUDA%20%2F%20NVENC-76b900?logo=nvidia&logoColor=white)](https://developer.nvidia.com/cuda-zone)
[![Three.js](https://img.shields.io/badge/Three.js-Spatial%20Shell-111111?logo=three.js&logoColor=white)](https://threejs.org/)
[![Freeware](https://img.shields.io/badge/license-freeware%20preview-2f855a)](#license-and-source-boundary)

## Illustrated First-Use Storybook

Start here for a large-type, print-friendly introduction to the DreamFrameOS experience:

[**The First DreamFrame Day**](output/pdf/DreamFrameOS-The-First-DreamFrame-Day.pdf) is a 30-page illustrated PDF written and illustrated by Cody Meridian Vale for MiLO83 and DreamFrameOS. It introduces the spherical room, Johnny AFK, notion connecting, Guide / Co-Create / Autopilot, Kids Mode guard rails, the console hatch, 360 chair use, and safe first-session habits.

DreamFrameOS is a freeware preview of an AR-first, WebXR-based Linux workspace for Quest-class headsets and RTX-backed desktop/server machines.

The big idea is simple: vibe code from inside a spatial room instead of staring at flat monitors. The headset owns tracking, passthrough, controller input, local terminal overlays, emergency controls, and final WebXR composition. The PC or Ubuntu RTX host owns expensive work such as remote render layers, Linux app surfaces, CUDA, NVENC video, virtual displays, and Pixal3D/ComfyUI asset generation.

DreamFrameOS is not a finished bootable operating system yet. It is currently an OS-like spatial workspace that runs on top of an existing Windows or Ubuntu machine, with a path toward a future native Ubuntu appliance image.

## Public Preview Status

This public repository is intentionally a README and release channel.

The runnable preview ships through GitHub release installer artifacts. The public repo itself is not a static GitHub Pages app; the native Ubuntu RTX installer is the recommended public path.

Current public release goal:

- explain what DreamFrameOS is
- document the native Ubuntu RTX installer workflow
- document the optional Windows/Quest shell preview
- keep source private unless source publication is intentionally requested
- publish release notes without leaking private implementation files

## What Opens By Default

The stock startup experience opens directly into the DreamFrame spherical computer environment.

Default first-run state:

- DreamFrame spherical room visible
- Guide involvement tier selected
- technical console hidden
- Johnny AFK asleep/idle inside the main display
- setup gate visible when RTX/Pixal3D services are missing
- clock hover transition into Johnny AFK
- `~` / Backquote cycles between shell, console, and Johnny AFK

This means a tester should land in the actual spatial OS shell first, not a marketing page.

## Core Experience

DreamFrameOS is built around:

- a Quest-owned WebXR compositor
- a local terminal/control overlay that stays usable even if streams fail
- three display modes: full VR-style opacity, 50 percent AR hybrid, and passthrough AR
- posture modes for desk use, 360 swivel-chair use, and bed skyview use
- RTX remote layers for app panels, generated scenes, video layers, and optional 3D backgrounds
- Notions, which represent places, tools, characters, services, app surfaces, and experiences
- Johnny AFK, an idle/dream companion layer that can visualize approved tasks while you are away
- Johnny Advancement Notes for sharing plain-text improvement advice without publishing user code
- prompt and voice-to-experience flows that do not expose generated source by default
- a Drag Bus for moving text, file references, and Notions between floating targets
- Pixal3D GGUF Asset Forge for generated GLB assets, optional rigging, and logical animation

The design priority is that critical interaction remains local to the headset/browser. Remote RTX layers are allowed to be beautiful and heavy; they are not allowed to be the only way to control the OS.

## Quick Start For Testers

Download the latest release installer from the [DreamFrameOS releases page](https://github.com/MiLO83/MindFrameOS/releases).

### Recommended: Native Ubuntu RTX

Use this path for the intended DreamFrameOS experience: RTX rendering, CUDA/NVENC checks, virtual Linux app panels, MediaMTX/WHEP remote layers, Pixal3D/ComfyUI, and Quest Browser access.

Extract `DreamFrameOS-*-native-ubuntu-rtx-installer.tar.gz` on the native Ubuntu RTX host, then preview the plan:

```bash
bash install-dreamframeos-rtx-ubuntu.sh --dry-run
```

Apply on the RTX host:

```bash
sudo bash install-dreamframeos-rtx-ubuntu.sh --apply
```

Then verify:

```bash
bash /opt/mindframe-os/scripts/verify-ubuntu-rtx-stack.sh --strict
```

For Quest Browser, open the configured HTTPS URL. If a temporary tunnel is needed:

```bash
cloudflared tunnel --url http://localhost:8787
```

Open the printed `https://...trycloudflare.com` URL in Quest Browser.

Expected target machines:

- RTX 5060/5060 Ti native Ubuntu host
- RTX 5090 native Ubuntu host

### Optional: Windows PC plus Quest Preview

The Windows path is only a shell preview. It is useful for checking the interface, Johnny AFK, Notions, setup gates, and Quest Browser tunneling, but it is not the preferred RTX runtime path.

Extract `DreamFrameOS-*-windows-quest-preview-installer.zip` on Windows, then run:

```powershell
powershell -ExecutionPolicy Bypass -File .\install-dreamframeos-preview.ps1 -Start
```

Open on the PC:

```text
http://localhost:8787
```

For Quest Browser:

```powershell
cloudflared tunnel --url http://localhost:8787
```

Open the printed `https://...trycloudflare.com` URL in Quest Browser.

What this preview path can show:

- stock DreamFrame spherical room
- Johnny AFK default display layer and transition
- WebXR shell and local scene behavior
- display mode controls
- Notion Index
- runtime Notion interop demo
- terminal overlay
- Drag Bus contracts
- Asset Forge panel and blocked/setup-required states

What this preview path does not prove:

- real Quest headset comfort UAT
- RTX 5060/5060 Ti or RTX 5090 CUDA/NVENC success
- real Pixal3D/ComfyUI generation on the target GPU
- production-grade Linux app streaming latency

### Source Checkout Autoinstall Workflow

If you have private source access instead of a release installer, run the lower-level provisioner on native Ubuntu, not inside WSL2.

Preview the plan first:

```bash
bash scripts/first-boot-provision-ubuntu.sh --dry-run --blackwell
```

Apply on the RTX host:

```bash
sudo bash scripts/first-boot-provision-ubuntu.sh --apply --blackwell
```

For RTX 50-series/Blackwell hosts that need custom model-stack packages, pass custom package sources during first boot:

```bash
sudo bash scripts/first-boot-provision-ubuntu.sh --apply --blackwell \
  --transformers-source https://github.com/example/transformers.git \
  --transformers-ref main \
  --torch-index-url https://download.pytorch.org/whl/cu128
```

Then run the host doctor:

```bash
bash scripts/verify-ubuntu-rtx-stack.sh --strict
```

The autoinstall path is designed to:

- refuse WSL/native-driver confusion
- install runtime packages and Node.js
- install Xvfb, Openbox, x11vnc, noVNC, and ffmpeg
- install NVIDIA driver/CUDA/NVENC pieces
- install Docker and NVIDIA Container Toolkit
- install Docker-backed MediaMTX for RTSP/WHEP layer transport
- install DreamFrameOS compatibility systemd services
- install `mindframe-renderd`
- install Pixal3D/ComfyUI and Blender helper pieces
- write environment files and setup reports
- run verification for GPU, NVENC, containers, services, MediaMTX, ComfyUI, and model-stack versions

Expected target machines:

- RTX 5060/5060 Ti native Ubuntu host
- RTX 5090 native Ubuntu host

WSL2 note: WSL2 gets NVIDIA/CUDA support from the Windows NVIDIA driver. Do not install the native Linux NVIDIA display driver inside WSL2.

## Runtime Usage

Once logged in, the runtime is meant to be operated from the scene itself.

Important controls:

- Rest the pointer over the system clock for about five seconds to enter Johnny AFK.
- Rest the pointer over the island hammock to put Johnny back to sleep and return to DreamFrame.
- Press `~` / Backquote to cycle DreamFrame shell, DreamFrame plus console, and Johnny AFK.
- Use the display mode toggle for full VR, 50 percent AR hybrid, and passthrough AR.
- Use posture layout controls for desk, 360 swivel chair, and bed skyview.
- On desktop preview, click the 3D viewport to enter pointer-lock mouse look; horizontal mouse movement wraps continuously around 360 degrees and Escape releases capture.
- In 360 Chair mode, window controls use seam-aware interaction zones: if the same window appears on both sides of the `-180/+180` edge, the wrap-side copy resolves to the same underlying layer target for buttons, focus, drops, and future hit tests.
- The current procedural 360 chamber and island backgrounds are horizontally seam-safe; future imported art still needs an explicit seam/tile check.
- Use the terminal hatch for shell work. It stays local so remote stream failures do not strand the user.
- Use Guide, Co-Create, or Autopilot to choose how much agency the system has.
- Use prompt or voice input to ask for experiences, tools, characters, or scene changes.
- Use the Notion Index for power-user clarity when the scene metaphor gets too abstract.
- Use Drag Bus drops to move text, file references, and Notions between floating panels and targets.
- Use emergency disconnect to dim/drop remote layers while keeping local controls alive.

The interaction model is intentionally forgiving. Shell cycling ignores held-key repeats and debounces rapid accidental double taps. Important changes are designed to go through preview, approval, or undoable paths instead of relying on tiny precise clicks.

## Distributed Rendering Model

DreamFrameOS does not stream the Quest passthrough camera to the PC.

The Quest/browser owns final composition:

1. headset passthrough from WebXR `immersive-ar`
2. local DreamFrame shell
3. remote RTX video/app/scene layers
4. local terminal and control overlays
5. controller/hand affordances and emergency controls

The RTX host supplies remote layers through WebRTC/WHEP-style paths. H.264/NVENC is the compatibility default, with AV1/HEVC as capability-detected upgrades on RTX 50-series hardware and supported browsers.

Optional depth/alpha sidecars are future-facing. The preview remains useful without them by treating remote content as rectangular or curved visual surfaces.

## Notions And Runtime Interop

DreamFrameOS distinguishes separate programs as Notions.

A Notion can be a:

- place
- tool
- character
- service
- app surface
- experience

Notions declare what they can do, what they need, where they can attach, what surfaces they expose, and what permissions they require.

This makes the following design goal valid:

> You could make a basketball game as one Notion, a GTA-like open-world game as another Notion, then ask DreamFrameOS to run the basketball Notion at the basketball court in the GTA-like game.

The host Notion owns the larger world and anchor. The guest Notion owns its own rules, surfaces, state, and rollback boundary. Integration happens through explicit approved links rather than one program silently taking over another.

## Johnny AFK

Johnny AFK is the idle/dream layer of DreamFrameOS.

When the system idles or the clock hover ritual is triggered, Johnny can wake up on a tiny island and visualize what he is working on. He can accept assigned tasks, invent bounded tasks, and return with cards that explain what happened. Protected work should still require approval before promotion.

The intended feel is playful, but the safety model is serious: generated work should be previewed, explained, and approved instead of silently mutating user-owned worlds.

Johnny's public sharing model is advice-only. A user's Johnny may research, prototype, test, and implement locally after approval, but the community artifact should be a plain-text Advancement Note rather than a code patch. Advancement Notes describe the problem, local outcome, implementation recipe, compatibility tags, test checklist, risks, privacy notes, and share score. The score weighs impact, generality, safety, portability, test evidence, accessibility, reversibility, and clarity. This lets users benefit from each other's discoveries while keeping personal prompts, private files, local source, secrets, logs, and machine-specific paths out of the public feed.

## Asset Forge And Pixal3D

Pixal3D GGUF is the required 3D asset backend direction for DreamFrameOS Asset Forge.

Asset Forge jobs can come from:

- direct prompts
- voice prompts
- Johnny AFK tasks
- Drag Bus drops
- Codex/terminal helper commands

The current design targets GLB output, optional Blender auto-rigging, and Quest-local logical animation for rigged humanoid characters. If Pixal3D/ComfyUI is not configured, jobs should remain visible as blocked/setup-required instead of disappearing.

Pixal3D output should be treated as static mesh output unless a separate rigging pass succeeds.

## Current Limits

DreamFrameOS is still an active prototype.

Known limits:

- public repo is not a standalone static web app
- public repo does not currently expose private source code
- source-free preview installer exists, but it is not a finished consumer OS installer
- Quest headset UAT must still be run on physical hardware
- RTX 5060/5060 Ti CUDA/NVENC success must be verified on the target Ubuntu host
- RTX 5090 CUDA/NVENC success must be verified on the target Ubuntu host
- Pixal3D/ComfyUI generation and auto-rig UAT must be run on the RTX host
- MediaMTX/WHEP Quest and RTX-host remote layer UAT is still pending
- full arbitrary game/runtime hosting still needs per-runtime adapters
- bootable appliance image exists as a build path, but target-machine install/UAT remains required

## Roadmap

Near term:

- Quest/RTX UAT for the public preview installer artifact
- Quest headset UAT for stock startup, Johnny AFK, terminal focus, and mode switching
- RTX 5060/5060 Ti and RTX 5090 Ubuntu validation
- Pixal3D/ComfyUI exported workflow replacement after host UAT
- MediaMTX/WHEP end-to-end headset UAT

Mid term:

- richer Johnny AFK task invention and return cards
- community Advancement Notes for advice-only sharing across divergent user forks
- persistent spatial layout polish
- app surface docking and runtime persistence
- stronger adaptive quality telemetry
- more comfortable 360 chair and bed skyview interaction

Long term:

- bootable Ubuntu-based DreamFrameOS appliance image
- first-boot NVIDIA/CUDA provisioning as the normal install path
- automatic tunnel/service setup
- polished source-free public installer
- stereo/foveated/depth-aware remote layer research

## Safety Philosophy

The local shell wins.

DreamFrameOS should remain usable when remote rendering stutters, RTX services are offline, or a stream gets corrupted. Terminal focus, mode toggles, emergency disconnect, controller affordances, and critical menus stay local to the Quest/browser compositor.

Protected experiences and Notion memory should require explicit approval before promotion or mutation. Autopilot can prepare work, but it should not silently overwrite the user-owned world.

Community sharing should prefer plain-text Advancement Notes over code. Johnny can help a local user craft and verify a change, but public sharing should default to advice, recipe, and spec text that another user's own Johnny/Cody can adapt to that user's personal setup.

## License And Source Boundary

DreamFrameOS is currently a personal freeware prototype. A formal reuse/contribution license has not been selected yet.

Until a license file lands, treat this as freeware preview documentation and release information, not a finalized open-source grant.

## Name

DreamFrameOS is about making coding in VR feel less like wearing monitors on your face and more like stepping into a useful, imaginative place.
