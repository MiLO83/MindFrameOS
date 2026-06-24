# DreamFrameOS Release Notes

## v0.2.7-preview - 2026-06-23

This preview makes the package tester-ready around the real DreamFrameOS thesis: Notions as interoperable places/tools/characters/surfaces/games/workflows/memories, Quest/WebXR entry, native Ubuntu RTX readiness, and advice-first self-evolution.

### Tester-Ready MVP Framing

- Added `CREDITS.md` with Miles Cameron Johnston as creator/copyright holder and Cody Meridian Vale as an AI collaborator/persona through OpenAI Codex.
- Added legal-safe acknowledgements for OpenAI Codex, Ubuntu-compatible Linux targets, NVIDIA RTX/CUDA/NVENC, and Quest-class WebXR devices without implying official sponsorship or endorsement.
- Added `docs/beta-tester-quickstart.md` as the one-page install/run/report path for a general Beta Tester.
- Tightened the README and installer instructions around the native Ubuntu RTX path first, with Windows clearly marked as a shell preview.
- Added selected release-package docs/media so tester bundles include the quickstart, credits, storybook PDF, public podcast MP3, and podcast transcript without copying private caches or model directories.

### Notion Interop and Self-Evolution

- Reframed DreamFrameOS as a freeware spatial framework, not a normal app launcher.
- Clarified that Notions declare capabilities, anchors, inputs, outputs, surfaces, and safe composition intent.
- Kept the flagship sample claim generic and visible: a basketball Notion can run at a basketball-court anchor inside an open-city Notion.
- Locked Johnny/Cody daily work to advice-first Advancement Notes by default. Notes can describe goals, recipes, compatibility tags, risks, tests, and local adaptation steps; code or patch sharing still requires explicit user approval.
- Kept `mindframe` names documented as internal compatibility plumbing while preserving DreamFrameOS as the public product identity.

### Verification

- `npm test` passes: 79 test files, 417 tests.
- `npm run build` passes with the known Vite large chunk warning.
- `scripts/build-public-preview-installer.ps1 -Version v0.2.7-preview -SkipBuild` produced native Ubuntu RTX and Windows/Quest preview packages from the verified dist output.
- Release-package checks confirm selected tester docs/media are included and `.venv-podcast`, `.cache`, private env files, API keys, Hugging Face cache paths, and model-cache paths are absent from the staged bundles.
- Built-server smoke passes: portal 200, login ok, stock DreamFrame startup, Guide mode, console hidden, Johnny asleep, setup-required blockers visible, one Notion interop plan, and three render layers.
- Hardware UAT remains explicit: native Ubuntu RTX CUDA/NVENC proof, Quest 2/3/3S WebXR entry, fresh frame telemetry, and optional MediaMTX/WHEP remote-layer proof on real tester hardware.

## v0.2.6-preview - 2026-06-21

This preview makes the 360 wrap contract apply to window controls, not only camera motion and background sampling.

### Seam-Aware Window Interaction

- Added shared seam interaction zones for 360 Chair windows.
- Windows near the `-180/+180` panoramic edge can now project a wrap-side interaction proxy.
- The primary window and wrap-side proxy share the same target layer ID, so buttons, focus actions, drops, and future hit tests can resolve to the same underlying window from either side of the seam.
- The desktop layer controls now use the seam-zone projection in 360 Chair mode, so edge proxy controls are real action targets instead of visual-only duplicates.
- Added tests for nearest-window ordering across the rear seam, authored placements around the seam, and same-target wrap-side interaction proxies.

### Verification

- `npm test` passes: 67 test files, 262 tests.
- `npm run build` passes with the known Vite large chunk warning.

## v0.2.5-preview - 2026-06-21

This preview tightens Johnny AFK's daily improvement and community-sharing model, and hardens the current procedural 360 backgrounds against visible horizontal seams.

### Johnny Advancement Notes

- Added advice-only Johnny Advancement Note contracts for daily improvement sharing.
- Johnny may research, prototype, test, and implement locally after approval, but community sharing defaults to plain-text advice rather than code, patches, private paths, terminal logs, prompts, or personal files.
- Advancement Notes include a problem statement, local outcome, implementation recipe, compatibility tags, test checklist, risks, privacy notes, and share score.
- Share scoring now weighs impact, generality, safety, portability, test evidence, accessibility, reversibility, and clarity.
- Private local changes remain non-shareable even when their local score is high; maintainers can manually translate broadly useful notes into stock code later.

### 360 World Seam Check

- Fixed the Johnny island procedural cloud sampling so the horizontal equirectangular edge wraps cleanly.
- Added tests proving the DreamFrame chamber and Johnny island color/depth samples match at `u=0` and `u=1`.
- Documented that the current procedural desktop/Quest 360 maps are seam-safe, while future imported art must still pass horizontal tile/seam verification.
- Added desktop pointer-lock mouse look so horizontal preview rotation wraps continuously around 360 degrees instead of stopping at the browser window edge.
- Added tests for yaw normalization and for ignoring desktop mouse-look while unlocked or inside a WebXR session.

### Verification

- `npm test` passes: 67 test files, 258 tests.
- `npm run build` passes with the known Vite large chunk warning.

## v0.2.4-preview - 2026-06-21

This preview publishes the stock DreamFrame startup and a native RTX-first public installer handoff.

### Public Release Handoff

- Updated the README with a top-level project description, native Ubuntu RTX installer workflow, optional Windows/Quest preview path, and runtime usage checklist.
- Clarified that the public GitHub repo is a freeware README/release channel while the live app ships through installer artifacts.
- Documented the native Ubuntu RTX provisioner as the intended path for RTX 5060/5060 Ti and RTX 5090 Blackwell hosts.
- Documented the runtime defaults: stock DreamFrame spherical room, Guide mode, hidden console, Johnny AFK asleep in the main display, clock/hammock transitions, and the `~` shell cycle.
- Kept the accessibility contract visible by noting debounced shell cycling, repeat-key rejection, and approval/preview boundaries for meaningful changes.

### Release Boundary

- The current compatibility release channel at `MiLO83/MindFrameOS` should stay README-only unless source publication is intentionally requested.
- GitHub releases can publish installer artifacts without publishing the private TypeScript source tree.
- The private `MiLO83/MindFrameOS-Source` repo remains the compatibility source/control repo for implementation commits.
- Public release artifacts now include installer bundles; native Ubuntu RTX is the recommended path.

## v0.1.0.preview Update - 2026-06-21

This preview update hardens the native Ubuntu RTX stack for RTX 50-series/Blackwell testing.

### Stock Startup

- Added the stock DreamFrame spherical computer environment id `stock.mindframe-spherical-environment`.
- Changed first-launch involvement to Guide so freeware users start in a safer approval-oriented mode.
- Added stock setup readiness fields and blockers for the RTX renderer and Pixal3D GGUF stack.
- Made Johnny AFK part of the default main-display experience instead of only an opt-in sample.
- Added a backquote/tilde shell cycle contract: DreamFrame shell, DreamFrame plus console, then Johnny AFK.
- Removed old trademark-risk naming from public README/test copy.

### Ubuntu RTX Stack

- Added a first-boot stack path for RTX 5060/5060 Ti and RTX 5090 hosts with `--blackwell` support.
- Added Xvfb and Docker-backed MediaMTX systemd services.
- Updated `mindframe-renderd` service dependencies so remote render producers start with the virtual display and media bridge.
- Extended first-boot provisioning to install runtime packages, Node.js, virtual display tooling, ffmpeg, Docker/MediaMTX, npm dependencies, and production build output.
- Added Blackwell-friendly Pixal3D/ComfyUI hooks for custom Transformers checkouts and custom PyTorch wheel indexes.
- Added `scripts/verify-ubuntu-rtx-stack.sh` to check native Ubuntu, WSL rejection, NVIDIA visibility, Blackwell driver readiness, CUDA, NVENC, Docker GPU runtime, systemd units, MediaMTX, ComfyUI, and `torch`/`transformers` package versions.
- Added a focused host checklist at `docs/ubuntu-rtx-stack.md`.

### Verification

- `npm test` passes: 66 test files, 246 tests.
- `npm run build` passes with the known Vite large chunk warning.

### Remaining UAT

- Run the installer on the RTX 5060/5060 Ti Ubuntu host.
- Run the installer on an RTX 5090 Ubuntu host.
- Confirm a real RTX remote layer publishes through MediaMTX/WHEP into Quest Browser.

## v0.1.0.preview Update - 2026-06-20

This preview update documents and tests the runtime Notion interop model.

### Runtime Notion Interop

- Added a shared runtime interop planner for composing one Notion inside another at a semantic host anchor.
- Documented the core claim: a basketball game can be one Notion, an open-city game can be another Notion, and DreamFrameOS can plan the basketball Notion to run at the basketball-court anchor inside the open-city Notion.
- Added built-in `Pickup Basketball Demo` and `Open City Demo` Notions so the Windows/Quest preview can show the interop plan without the RTX stack.
- Added a Runtime Interop panel to the control shell for visible host/guest/anchor/status details.
- Preserved the safety model: the host owns world placement, the guest owns its rules and surfaces, local controls stay local, and protected memory remains isolated unless an approved link exists.
- Added degraded-state behavior for missing host anchors or missing guest runtime surfaces.

### Windows/Quest Preview

- Added README steps for trying the preview shell from Windows PC and Quest 2 through a secure tunnel.
- Clarified the preview boundary: Windows can show the shell, Notions, Runtime Interop demo, Johnny AFK, terminal overlay, Drag Bus, and local scene behavior; native Ubuntu RTX is still the target for CUDA/NVENC, remote app panels, Pixal3D/ComfyUI, and full hardware UAT.

### Character Asset Pipeline

- Added `mindframe-humanoid-v1` rig manifests for Pixal3D/Blender-generated characters.
- Added placement guards so auto-character jobs must pass rig validation before becoming living placed characters.
- Added Quest-local logical animation clips for rigged Johnny/assistant characters, including wake, work, think, approval, idle, and doze behaviors.

### Verification

- `npm test` passes: 65 test files, 238 tests.
- `npm run build` passes.

### Remaining UAT

- Quest headset UAT is still required for physical WebXR interaction.
- RTX-host Pixal3D/ComfyUI generation and auto-rig UAT is still required on the target Ubuntu/NVIDIA machine.
- Live arbitrary game/runtime hosting requires per-runtime adapters and approval boundaries before it can be considered complete.
