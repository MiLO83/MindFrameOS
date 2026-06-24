# DreamFrame Radio - Episode 01 Public Cut

Title: The Computer Room That Dreams Back

Runtime target: 5 to 7 minutes

Speakers:

- Mara, host and curious technical listener
- Cody, the DreamFrameOS collaborator inside the spherical room
- Beta Tester, outside tester and AI/GPU friend
- Johnny AFK, the island worker who wakes up when you step away

Transcript:

Mara: Welcome to DreamFrame Radio. Today we are talking about DreamFrameOS, a freeware spatial coding workspace that opens in a Quest browser, talks to an RTX-backed PC, and tries to make coding feel less like fighting little windows and more like stepping into a living computer room.

Cody: The simple version is this: DreamFrameOS is a WebXR workspace for vibe coding. The headset owns the things that must stay immediate, like tracking, passthrough, controller input, local controls, terminal focus, and emergency disconnect. The PC owns the expensive work, like RTX render layers, Linux app panels, CUDA jobs, NVENC streams, and heavier generated assets.

Beta Tester: That split matters. If a remote render stream hiccups, the local shell should not disappear with it. You still need the controls. You still need the terminal. You still need the way out.

Mara: So this is not classic PC VR streaming, where the whole final picture comes from the computer.

Cody: Right. The Quest is the final compositor. The RTX machine contributes useful layers. That means passthrough AR, tracking, local UI, mode switching, and safety controls stay on the headset. The server sends layers that can be placed into the world.

Mara: And the public name is DreamFrameOS.

Cody: Yes. DreamFrameOS is the public name. MindFrame remains an internal compatibility name where older services and contracts still use it. Publicly, the idea is not that someone frames your mind. It is that you open a dream frame and build inside it.

Beta Tester: The vibe is surprisingly specific. There is a spherical computer room. There is a main display. Johnny AFK is visible there by default, asleep until the screensaver idea wakes him up. It feels playful, but it is also a workflow.

Johnny AFK: Aloha. I was not idle. I was researching very slowly with my eyes closed.

Mara: Johnny, that is exactly what an idle worker would say.

Johnny AFK: Then my cover story is working.

Mara: What is actually implemented right now?

Cody: The current repo has a Fastify backend, a React shell, and a Three.js WebXR scene. It has the DreamFrame spherical startup state, Guide mode by default, a hidden console by default, and a backquote cycle that moves between shell, console, and Johnny AFK.

Cody: It also has local controls for display modes, postures for desk, swivel chair, and bed use, 360 desktop panning, mode switching, a local terminal and control overlay, setup gates for missing RTX or Pixal3D services, and a Notion system for connecting tools and places together.

Beta Tester: The Notion interop piece is the one I keep watching. The claim is that you could build a basketball game as one Notion, an open city as another Notion, and then ask DreamFrameOS to run the basketball Notion at the basketball court in the city. That is the kind of integration that makes the project more than a launcher.

Mara: And the voice side?

Cody: Voice is central. The project is moving toward text and speech to experience, with hidden implementation unless the user asks to see code. The voice pipeline can separate local control from broader generation. So "make the text bigger" or "switch to bed mode" stays local and safe, while "make me a new coding island" becomes an experience draft that can be reviewed.

Mara: That feels important for accessibility too.

Cody: It is. DreamFrameOS is intentionally moving away from tiny, precise, mouse-only control. The design favors big forgiving controls, debounced input, undo, clarification, voice-first workflows, and local safety overlays that remain usable even when remote streams fail.

Beta Tester: The tester files are not pretending everything is done. They show a lot of pass and retest-pass items, but the honest blockers are still physical hardware validation. A real Quest has to enter WebXR and post fresh immersive frame telemetry. A real native Ubuntu RTX host has to prove CUDA and NVENC end to end.

Mara: So what needs more development time?

Cody: First, Quest headset UAT. Desktop tests can prove the shell is nonblank, routes respond, and layers attach, but a Quest 2 or Quest 3S has to prove the real immersive path. Second, native Ubuntu RTX validation on the target GPUs, especially RTX 5060 Ti and RTX 5090.

Cody: Third, the remote render layer path needs full MediaMTX or WebRTC UAT from server to headset. Fourth, Pixal3D and ComfyUI generation need real host install, config, and proof that generated models can be placed, rigged, and used. Fifth, headset comfort needs iteration for mode switching, Johnny AFK, terminal focus, and 360 use in a chair or bed.

Beta Tester: That is the right place for the project to be honest. It is not finished. It is also not fake. It has architecture, routes, UI, tests, installer work, and proof clips. Now it needs the hardware pass that turns prototype confidence into user confidence.

Mara: What is the big dream if those proof points land?

Cody: A freeware OS-like workspace where someone can sit down, put on a headset, speak what they want to build, see Cody turn that into an experience, connect tools as Notions, let Johnny AFK research improvement notes while they are away, and use the whole thing across VR, mixed AR, passthrough AR, desktop, and eventually mobile 360 mode.

Johnny AFK: I would also like to confirm that the island is a serious productivity environment.

Beta Tester: With a hammock.

Johnny AFK: Especially with a hammock.

Mara: That is DreamFrameOS in one sentence: a serious productivity environment with a hammock, an RTX machine, a Quest browser, and a computer room that dreams back.

Cody: Not done. Not fake. Very much awake.

Mara: This has been DreamFrame Radio. The next proof point is waiting inside the headset.


