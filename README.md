# MindFrameOS

[![TypeScript](https://img.shields.io/badge/TypeScript-5.9-3178c6?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![React](https://img.shields.io/badge/React-19-61dafb?logo=react&logoColor=111111)](https://react.dev/)
[![Three.js](https://img.shields.io/badge/Three.js-WebXR-111111?logo=three.js&logoColor=white)](https://threejs.org/)
[![Fastify](https://img.shields.io/badge/Fastify-control_server-111111?logo=fastify&logoColor=white)](https://fastify.dev/)
[![NVIDIA](https://img.shields.io/badge/NVIDIA-CUDA%20%2F%20NVENC-76b900?logo=nvidia&logoColor=white)](https://developer.nvidia.com/cuda-zone)

MindFrameOS is an AR-first, WebXR-based Linux workspace for Quest-class headsets and RTX-backed desktop/server machines.

> This public repository is the MindFrameOS landing and release channel. It intentionally exposes the README and public releases only; the active source repository is private while the project is still experimental.

The headset owns the things that must never stutter: passthrough, tracking, controller input, local terminal overlays, emergency controls, and final WebXR composition. The PC owns the expensive work: RTX remote render layers, Linux app surfaces, NVENC video paths, CUDA workloads, and future noVNC/virtual display capture.

The result is not a bootable operating system yet. It is an OS-like spatial workspace that runs on top of an existing desktop/server OS today, with a clear path toward a future bootable MindFrame appliance.

## The Idea

MindFrameOS is for vibe coding in VR/AR.

Instead of treating VR as a monitor replacement, MindFrameOS treats the workspace as a living scene:

- a local Quest-owned shell for reliable controls
- floating RTX-rendered app surfaces
- a terminal hatch that stays usable when streams fail
- Notions for places, tools, characters, services, and app surfaces
- Johnny AFK, an idle companion/screensaver layer that can queue or visualize work
- a Drag Bus for moving Notions and content between floating targets
- a three-tier involvement toggle: Guide, Co-Create, Autopilot

The tone target is part Professor X Cerebro room, part Johnny AFK screensaver, part practical Linux workstation. The system should feel dreamy, but the control plane stays boring on purpose.

## What Works Now

MindFrameOS currently includes:

- Fastify server with local password auth
- React control shell
- Three.js/WebXR scene
- WebXR display mode controls:
  - full VR-style opaque world
  - 50 percent AR hybrid
  - passthrough AR
- local terminal/control overlay
- emergency disconnect that dims remote layers
- MindFrame Notions:
  - Johnny AFK
  - Johnny AFK Island
  - Terminal
  - RTX Renderer
  - Media Bridge
  - Current Experience
- Notion Index power-user panel
- Notion Event Bus and Recent Activity feed
- Drag Bus for typed drops between Notions and floating targets
- Johnny AFK state contracts, hover rituals, return cards, and hidden-code prompt scaffolding
- prompt-to-experience scene planning without exposing generated source by default
- scratch/pinned experience scaffolding for compatibility checks and rollback
- distributed render-layer manifests
- authenticated WebRTC signaling hub
- MediaMTX bridge metadata for RTSP publish and WHEP egress
- WHEP receive helper for attaching remote video streams to Three.js textures
- `mindframe-renderd` daemon scaffold for RTX/NVENC producer planning
- guarded Ubuntu NVIDIA/CUDA installer
- NVIDIA/CUDA/NVENC verification scripts
- systemd, Cloudflare Tunnel, and MediaMTX example configs
- Vitest coverage across shared contracts, server routes, signaling, render daemon planning, client projection, and WebXR state

## Architecture

MindFrameOS uses a headset-owned compositor.

```mermaid
flowchart LR
  Quest["Quest Browser<br/>WebXR compositor"] --> Shell["Local shell<br/>terminal, menus, Drag Bus"]
  Quest --> AR["Passthrough AR<br/>immersive-ar"]
  Quest --> Layers["Remote video layers<br/>Three.js planes or WebXR layers"]

  Server["MindFrameOS Fastify server"] --> Auth["Local password session"]
  Server --> API["MindFrame APIs"]
  Server --> Signal["Authenticated WebRTC signaling"]

  Renderd["mindframe-renderd<br/>Ubuntu RTX host"] --> NVENC["ffmpeg NVENC<br/>H.264 / AV1 / HEVC"]
  Renderd --> Apps["Linux app surfaces<br/>future virtual panels"]
  NVENC --> MediaMTX["MediaMTX<br/>RTSP in, WHEP out"]
  MediaMTX --> Layers

  Shell <--> API
  Signal <--> Renderd
  Signal <--> Quest
```

Composition order:

1. Quest passthrough camera from `immersive-ar`
2. local WebXR shell and opacity-controlled world
3. RTX remote render layers
4. local terminal/control overlays
5. controller affordances and emergency disconnect UI

Remote layers are never trusted for safety-critical controls. If an RTX stream drops, the shell remains local.

## Quest And WebXR Model

Quest Browser is the target headset runtime.

The app requests `immersive-ar` with optional WebXR features:

- `layers`
- `local-floor`
- `bounded-floor`
- `hand-tracking`
- `hit-test`

The three display modes are implemented as composition behavior inside one AR session, because WebXR cannot freely switch between `immersive-vr` and `immersive-ar` mid-session.

Native WebXR composition layers are intended when available. The fallback path renders remote streams as textures on Three.js planes.

## Distributed Rendering Model

MindFrameOS does not stream the Quest passthrough camera to the PC.

The Quest owns the final frame. The RTX server sends remote scene/app/video layers into the Quest, and the headset/browser composes them with local passthrough and local controls.

The v1 control plane exposes:

- `GET /api/render/layers`
- `POST /api/render/layers/:id/start`
- `WS /api/render/signaling`

The browser registers as a render-layer `consumer`. `mindframe-renderd` registers matching RTX-backed layers as a `producer`. The signaling hub relays WebRTC `offer`, `answer`, and `ice-candidate` messages only between opposite roles on the same layer.

The daemon can build low-latency `ffmpeg` commands:

- AV1: `av1_nvenc`
- HEVC: `hevc_nvenc`
- H.264 compatibility fallback: `h264_nvenc`
- RTP packet size defaults to `1200`

Layer start responses include MediaMTX bridge metadata:

- `publishUrl`: RTSP input for renderd/ffmpeg
- `whepUrl`: browser-visible WebRTC egress URL
- `localWhepUrl`: local development URL

## Notions

MindFrameOS distinguishes separate programs as Notions.

A Notion can be a:

- place
- tool
- character
- service
- app surface
- experience

Notions declare abilities, needs, anchors, surfaces, permissions, memory policy, safety tier, and links. They integrate through approved links between typed needs and typed abilities. Protected Notion memory is persisted in `.mindframe/notion-memory.json` and can only be changed by the owning Notion or by another Notion with an active approved `allowsMemoryWrite` link. Accepted and rejected writes emit audit events in Recent Activity.

The Notion Index is the power-user view. The immersive shell can still present them as places, tools, characters, and world objects first.

## Drag Bus

The Drag Bus is the first local drag/drop layer for floating windows and Notions.

Supported v1 drag item kinds:

- `text`
- `file-ref`
- `notion`

Supported target kinds:

- floating panel
- terminal
- app surface
- Notion
- remote layer

Drops are typed and permission-aware. Accepted drops emit `drag.drop.completed`; rejected drops emit `drag.drop.rejected`. Raw payload contents stay out of Recent Activity.

The current browser UI is a desktop fallback for the same contracts Quest controller/hand interaction will bind to later.

## Johnny AFK

Johnny AFK is the idle/dream layer of MindFrameOS.

In the active chamber, rest the pointer over the system clock for about five seconds to trigger sleepy viewport eyelids and enter Johnny's island. On the island, rest over the thatched hammock to put Johnny back to sleep and return to the chamber.

Johnny can visualize assigned work and bounded invented work as return cards. Invented changes to pinned/protected experiences are preview branches until approved. Code, diffs, and terminal details stay hidden unless the console is toggled or details are requested.

## Involvement Tiers

MindFrameOS includes a three-tier involvement model:

| Tier | Meaning |
| --- | --- |
| Guide | The system suggests and explains, but waits for the user. |
| Co-Create | The system drafts, revises, and queues work with visible approval points. |
| Autopilot | The system can prepare preview changes and return cards, while protected experiences still need approval before promotion. |

## Local Development

Requirements:

- Node.js 20 or newer
- npm
- modern browser with WebGL/WebXR support for development

Install dependencies:

```bash
npm install
```

Create local env:

```bash
cp .env.example .env
```

Start the Fastify server:

```bash
npm run dev:server
```

In another terminal, start Vite:

```bash
npm run dev
```

Open the Vite URL for desktop development. The default development password is `mindframe` unless `MINDFRAME_PASSWORD` is set.

The server defaults to:

```text
http://127.0.0.1:8787/
```

## Scripts

| Command | Purpose |
| --- | --- |
| `npm run dev` | Start Vite client dev server on `0.0.0.0:5173`. |
| `npm run dev:server` | Start the Fastify control server. |
| `npm run dev:renderd` | Start the RTX render daemon scaffold. |
| `npm test` | Run Vitest. |
| `npm run build` | Type-check and build the client. |
| `npm run check` | Run tests and build together. |

## API Surface

Authentication:

- `POST /api/login`

Render layers:

- `GET /api/render/layers`
- `POST /api/render/layers/:id/start`
- `WS /api/render/signaling`

System:

- `GET /api/system/gpu`

MindFrame:

- `GET /api/mindframe/state`
- `POST /api/mindframe/afk`
- `POST /api/mindframe/prompt`
- `POST /api/mindframe/experiences/pin`
- `GET /api/mindframe/evolve/tasks`
- `POST /api/mindframe/console`
- `GET /api/mindframe/capabilities`
- `GET /api/mindframe/linux-apps`

Notions:

- `GET /api/mindframe/notions`
- `GET /api/mindframe/notions/events`
- `GET /api/mindframe/notions/memory/:notionId`
- `POST /api/mindframe/notions/links`
- `POST /api/mindframe/notions/memory`

Drag Bus:

- `GET /api/mindframe/drop-targets`
- `POST /api/mindframe/drop`

## Ubuntu NVIDIA/CUDA Setup

MindFrameOS includes a guarded native Ubuntu installer for RTX hosts.

Preview first:

```bash
bash scripts/install-nvidia-cuda-ubuntu.sh --dry-run
```

Apply from the intended Ubuntu host:

```bash
sudo bash scripts/install-nvidia-cuda-ubuntu.sh --apply
```

The installer:

- only runs on native Ubuntu
- refuses WSL2
- checks for NVIDIA hardware through `nvidia-smi` or `lspci`
- installs CUDA repository keyring
- installs `cuda-drivers`, `cuda-toolkit`, and `ffmpeg`
- installs Docker and NVIDIA Container Toolkit unless `--no-docker` is used
- configures Docker with `nvidia-ctk`
- runs `scripts/verify-gpu.sh`
- writes `.mindframe/nvidia-setup-report.txt`

## WSL2 NVIDIA/CUDA Note

Do not run the native Ubuntu installer inside WSL2.

WSL2 gets NVIDIA/CUDA support from the Windows NVIDIA driver. Installing a Linux NVIDIA display driver inside WSL is the wrong path and can create driver/toolkit confusion.

For WSL2:

```powershell
wsl --update
```

Then inside WSL:

```bash
nvidia-smi
```

For container GPU verification:

```bash
docker run --rm --gpus all nvidia/cuda:13.3.1-base-ubuntu24.04 nvidia-smi
```

WSL2 is useful for CUDA development. Native Ubuntu is the cleaner target for the full MindFrameOS render appliance, especially for NVENC, virtual displays, Linux app panels, capture, systemd services, and long-running RTX render daemon work.

## GPU Verification

Native Ubuntu:

```bash
bash scripts/verify-gpu.sh
```

Windows/PowerShell:

```powershell
powershell -ExecutionPolicy Bypass -File scripts/verify-gpu.ps1
```

Expected checks:

- `nvidia-smi` sees the RTX GPU
- ffmpeg exposes NVENC encoders
- Docker can run `nvidia-smi` inside an NVIDIA CUDA container

## Deployment Sketch

1. Copy the repo to `/opt/mindframe-os`.
2. Put secrets in `/etc/mindframe-os/mindframe.env`.
3. Install `deploy/mindframe-os.service`.
4. Install `deploy/mindframe-renderd.service`.
5. Start MediaMTX with `deploy/mediamtx.mindframe.example.yml`.
6. Configure Cloudflare Tunnel from `deploy/cloudflared-config.example.yml`.
7. Run `sudo bash scripts/install-nvidia-cuda-ubuntu.sh --apply` or `bash scripts/verify-gpu.sh`.
8. Start the services.
9. Open the tunneled HTTPS URL in Quest Browser.

## Private Source Layout

The private source repository is organized as:

```text
deploy/                  systemd, MediaMTX, Cloudflare examples
docs/                    architecture notes, UAT plans, design specs
scripts/                 NVIDIA/CUDA install and GPU verification scripts
src/client/              React shell, WebXR scene, WebRTC/WHEP clients
src/renderd/             RTX render daemon scaffold and NVENC command planning
src/server/              Fastify app, auth, signaling, state, registries
src/shared/              Zod contracts and pure shared state helpers
tests/                   Vitest coverage for contracts, routes, UI state, renderd
```

## Testing

Run the test suite:

```bash
npm test
```

Build:

```bash
npm run build
```

The current build may report Vite's large chunk warning because Three.js and the WebXR client are bundled together. That warning is expected for now.

## Current Limitations

MindFrameOS is an active prototype.

Current limitation TODOs:

- [ ] Build a bootable OS installer.
- [ ] Decide whether to add a GitHub Pages/static landing app, separate from the live Fastify-backed workspace.
- [ ] Run Quest headset UAT on physical hardware.
- [ ] Verify RTX 5060 Ti CUDA/NVENC success on the target Ubuntu host.
- [ ] Implement real Linux app panel streaming.
- [ ] Replace noVNC/virtual display planning hooks with a finished app surface stack.
- [ ] Complete full PTY-backed shell session behavior for the terminal.
- [ ] Replace push-to-talk scaffolding with a full speech recognition/input pipeline.
- [ ] Add optional remote depth/alpha sidecars.
- [ ] Run headset UAT for full controller/hand Drag Bus mechanics.

## Roadmap

Near-term:

- [ ] Real PTY-backed terminal sessions.
- [ ] Linux app panel streaming through virtual displays.
- [ ] Quest controller ray and hand pinch Drag Bus binding.
- [ ] First real `mindframe-renderd` producer loop.
- [ ] MediaMTX/WHEP end-to-end remote layer UAT.
- [ ] Headset comfort pass for mode switching, Johnny AFK, and terminal focus.

Mid-term:

- [x] Persistent Notion memory with approval boundaries.
- [ ] Daily self-revision plans with preview branches.
- [ ] Richer Johnny AFK task invention and return cards.
- [ ] App surface docking and spatial window layout persistence.
- [ ] RTX-rendered 3D background layers.
- [ ] Better adaptive quality telemetry.

Long-term:

- [ ] Bootable Ubuntu-based MindFrameOS appliance image.
- [ ] First-boot NVIDIA/CUDA provisioning.
- [ ] Automatic tunnel/service setup.
- [ ] Local speech-to-experience loop.
- [ ] Stereo/foveated/depth-aware remote layer research.

## Safety Philosophy

The local shell wins.

MindFrameOS should remain usable when remote rendering stutters, the RTX service is offline, or a stream gets corrupted. Terminal focus, mode toggles, emergency disconnect, controller affordances, and critical menus stay local to the Quest/browser compositor.

Protected experiences and Notion memory should require explicit approval before promotion or mutation. Autopilot can prepare work, but it should not silently overwrite the user-owned world.

## License

MindFrameOS is currently a personal freeware prototype. A formal reuse/contribution license has not been selected yet.

Until a license file lands, treat the repository as source-available for review and experimentation, not as a finalized open-source grant.

## Name

MindFrameOS grew out of the CerebroOS idea: a spatial coding room where the active workspace can drift into a Johnny AFK dream layer and back again.

The core question is simple:

Can coding in VR feel less like wearing monitors on your face and more like stepping into a useful, imaginative place?
