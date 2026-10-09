# Editor and delivery

Read for environment setup, automation, data round trips, browser previews, performance, or LAN delivery. Resolve exact commands and fields from the target version's documentation and real exports.

## Capabilities and versions

Distinguish launcher, editor, license edition, and target runtime versions. Confirm the required features can be saved, pin the authoring version, and check runtime compatibility against official rules. The historical case used Spine 4.3.26; that is neither a claim about the latest release nor a universal dependency.

For SDK, API, or CLI documentation, use Context7 when available: resolve the library ID first, then query one concept at a time. Runtime documentation does not establish editor behavior. Fill gaps using local help, version-matched examples, and official sources:

- Editor features: [User guide](https://esotericsoftware.com/spine-user-guide)
- Data and compatibility: [JSON format](https://esotericsoftware.com/spine-json-format), [Versioning](https://esotericsoftware.com/spine-versioning)
- Automation: [Command line](https://esotericsoftware.com/spine-command-line-interface), [Import](https://esotericsoftware.com/spine-import)
- Playback and textures: [Web Player](https://esotericsoftware.com/spine-player), [Texture Packer](https://esotericsoftware.com/spine-texture-packer), [Metrics](https://esotericsoftware.com/spine-metrics)

Use available file and CLI tools for data, and current computer-use documentation for native UI operations. Spine's custom canvas may lack accessible controls; use fresh screenshots for coordinate interactions. After reopening, confirm Setup/Animate mode, enabled animation, playback state, and advancing time. Selecting an animation label may not enable it, and clicking an enable toggle can disable an already active animation.

If binding by application path fails, obtain the real application identifier from the available-app inventory. Reobserve an unresponsive UI before clicking again; avoid historical bundle IDs, resolutions, or coordinates. Record concrete lock-screen, licensing, or tool blockers while progressing on independent work.

## Authoritative project and data

1. Preserve source artwork. Import a new candidate into a fresh `.spine` path and ensure import will not append duplicate skeletons to an existing project.
2. After native adjustments, treat the saved `.spine` file as authoritative. Export from that state for later processing and retain Nonessential editing data when needed for reimport. An older generation script must not replace newer weights or curves.
3. Check relative versus absolute angles, seconds versus frames, default scales, curve encoding, inheritance, and constraint order against same-version examples. Parent-local coordinates require inverse transforms; subtracting translation alone fails for rotated or scaled parents.
4. Rebuild weighted-mesh indices when the bone list changes. Validate references, weights, UVs, triangles, hulls, and bind poses together. Use Setup Pose or an explicit reference as the orientation baseline, rather than assuming the first animation frame is correct.
5. Re-export final runtime data after native saving and inspect changed bones, meshes, constraints, and tracks. JSON, atlas, and images must describe the same final state.

Image or video exports may require a graphical session. Successful JSON or atlas export does not prove image export works. Prefer export settings saved by the current editor, and inspect actual frame count, dimensions, duration, transparency, and looping. GIF timing quantization and binary transparency can alter rhythm and smoke edges; prefer runtime playback or alpha-capable image sequences for precise review. A verified 25 FPS GIF suited the historical case better than an unchecked 30 FPS export, but every project needs its own timing check.

## Playable review

Reuse the project's official player and packaging workflow, pin its version, and preserve license notices. Read actual animation names and atlas page references. Controls must reflect implemented capabilities. A review page should offer play/pause, speed, a timeline, light/dark backgrounds, and an action selector when needed. Include complete action boundaries and usable touch controls on narrow screens.

Keep displayed version, HTML, runtime data, and project consistent. Embedded single-file resources simplify offline sharing, but record their size and startup cost; deployment packages may keep resources external. Open the changed page and inspect actual playback and errors. HTTP 200 alone is not playback validation.

For local faults, add timestamped frame-review evidence when useful. Screenshots support rather than replace motion playback. Cover problematic frames at a scale that exposes seams. If continuous motion cannot be observed, state the evidence limit; tool waiting time is not observation time.

## Performance and boundaries

Separate authoring frame rate, export rate, browser draw rate, and display refresh rate. Set budgets for the requested device, size, DPR, and simultaneous instances; example budgets are not official limits.

Sample the target player's actual drawing path, excluding pause, scrubbing, and hidden-page intervals. A useful starting sample is five seconds of warm-up followed by 60 seconds of foreground playback at normal speed. Record average FPS, P95 frame interval, stall threshold/count, actual canvas pixel dimensions, instance count, system/browser, first-frame timing, and cache conditions. An independent requestAnimationFrame counter does not measure animation draws.

Record texture dimensions and resource bytes. Base RGBA storage is width × height × 4, with additional mipmap, decoding, and runtime costs. Address the measured bottleneck through atlases, transparent padding, meshes, weights, constraints, or rendering resolution, then recheck appearance. Label desktop and mobile-viewport emulation as proxy tests; without the target phone, keep physical-device performance unverified. With a host available, test real transitions, interruptions, background/resume, and sustained playback.

## Reopening, networking, and handoff

Copy the complete delivery to a new directory, open the copied native project, load relative-path images, and play it. Verify every atlas page and review link. An editable archive needs the project and artwork, not just runtime exports. Return to the authoritative project afterward so the user continues editing the correct file.

When LAN access is requested, verify the active LAN address and listen address, serve only the required delivery directory, and test the exact URL. Preserve the working local entry or explain its replacement. A successful request to the machine's own LAN address does not prove access from a second device; report that distinction. LAN sharing does not authorize public hosting or changes to system security settings.

Deliver the current playable entry, editable project/package, concrete changes, checks actually performed, and remaining limits. Keep delivery completion, technical validation, aesthetic acceptance, and physical-device performance separate in `PROJECT.md`.
