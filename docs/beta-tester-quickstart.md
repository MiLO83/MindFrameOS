# DreamFrameOS Beta Tester Quickstart

DreamFrameOS is a freeware preview for testing a spatial, Notion-based coding framework. It is not a finished consumer OS installer yet.

Public credit wording:

> Created by Miles Cameron Johnston with Cody Meridian Vale, an OpenAI Codex collaborator. Built for Ubuntu-compatible RTX hosts and Quest-class WebXR devices.

## What You Are Testing

DreamFrameOS should demonstrate three things:

- A Quest/desktop-accessible WebXR workspace that starts in the DreamFrame spherical room.
- Notions as interoperable places, tools, characters, surfaces, games, workflows, and memories.
- Johnny AFK and Cody-style improvement loops that propose advice-first changes instead of silently shipping user code.

The flagship interoperability idea is:

> A basketball Notion can run at a basketball-court anchor inside an open-city Notion.

That sample proves the framework shape. It is not claiming that a complete basketball game or open-city game ships in this preview.

## Recommended Test Path

Use a native Ubuntu RTX host when testing the intended runtime.

```bash
bash install-dreamframeos-rtx-ubuntu.sh --dry-run
```

If the dry run looks right:

```bash
sudo bash install-dreamframeos-rtx-ubuntu.sh --apply
```

Then run:

```bash
bash runtime/scripts/verify-ubuntu-rtx-stack.sh --strict
```

Expected host proof:

- native Ubuntu, not WSL
- NVIDIA GPU visible through `nvidia-smi`
- CUDA available
- ffmpeg reports NVENC encoders
- Docker and NVIDIA Container Toolkit available
- DreamFrameOS services installed
- MediaMTX/WHEP and Pixal3D/ComfyUI checks report honestly

## Quest Browser Test

Open the DreamFrameOS HTTPS URL in Quest Browser.

If testing through a temporary tunnel:

```bash
cloudflared tunnel --url http://localhost:8787
```

Expected headset proof:

- Quest Browser loads the login/portal.
- DreamFrameOS opens into the spherical DreamFrame room.
- Johnny AFK is visible in the main display.
- `~` / Backquote cycles shell, shell plus console, and Johnny AFK.
- Display modes cycle full VR-style opacity, 50 percent AR hybrid, and passthrough AR.
- Fresh WebXR frame telemetry appears after entering immersive mode.
- Local controls remain responsive even if a remote layer is missing or degraded.

## Optional Windows Preview

The Windows path is only a shell preview.

```powershell
powershell -ExecutionPolicy Bypass -File .\install-dreamframeos-preview.ps1 -Start
```

Use it to inspect the interface, Notion panels, Johnny AFK, setup blockers, voice/text prompt flow, and desktop 360 behavior. Do not use it as proof that the native RTX producer stack is complete.

## What To Report

Please report:

- host GPU model and driver version
- Ubuntu version or Windows version
- Quest model and browser version if available
- whether the installer was dry-run or applied
- whether `verify-ubuntu-rtx-stack.sh --strict` passed
- whether Quest entered immersive WebXR and posted fresh frame telemetry
- screenshots of setup blockers or degraded states
- any comfort issues in Desk, 360 Chair, or Bed Skyview posture

## Known Preview Boundaries

- Quest headset UAT must be run on physical hardware.
- Native Ubuntu RTX validation must be run on the target RTX host.
- Pixal3D/ComfyUI generation and auto-rig UAT still need the RTX host.
- MediaMTX/WHEP remote layer UAT still needs the real producer path.
- Johnny Advancement Notes are advice-first; sharing code or patches requires explicit approval.

