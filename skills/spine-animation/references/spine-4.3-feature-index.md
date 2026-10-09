# Spine 4.3 feature index

Use this index when a request needs a named Spine editor feature beyond the motion workflow. Read only the relevant section of [`docs/Spine-4.3-complete-manual.zh-CN.md`](../../../docs/Spine-4.3-complete-manual.zh-CN.md); do not load the complete manual for every animation task.

The manual is a project reference compiled for Spine 4.3 and checked against parts of Spine 4.3.26. It is not an official replacement for the current User Guide or API/runtime documentation. Verify version-sensitive behavior in the target editor and runtime before relying on it.

## Route by request

| Request or symptom | Start with | Then verify in the editor |
| --- | --- | --- |
| Create a skeleton, bone hierarchy, slots, draw order | Manual §§2, 4–5, 27/G1 | Setup mode, parent transforms, slot order, visibility and exported attachments |
| Assemble an image character | Manual §§6–7, 27/G3 | Region origin, parent, attachment visibility, alpha, crop and actual silhouette |
| Bend a soft limb or torso | Manual §§8–9, 27/G4–G5 | Mesh topology, UVs, bind pose, weights, extreme and in-between poses |
| Use a rigid part or linked mesh | Manual §§7–8, 27/G3–G4 | Attachment switching, linked-mesh source, pivot, draw order and export |
| Skin, outfit, modular equipment, skin bones | Manual §10, 27/G6 | Placeholder names, skin dependencies, setup pose and runtime skin selection |
| Foot, hand, weapon or target contact | Manual §11.2, 27/T2 | IK bend direction, reachability, mix, stretch, parent inheritance and transitions |
| Map one motion or control to another | Manual §11.3, §11.6, 27/T4/T6 | Transform mapping, Slider input range, local/world meaning and mixing |
| Rope, tentacle, chain, track or curve motion | Manual §11.4, 27/T3 | Path spacing, tangent, position/rotation modes, bone chain and endpoints |
| Hair, cloth, accessory or secondary motion | Manual §11.5, 27/T5 | Physics warm-up, reset, sampling, pause/resume and host movement |
| Key poses, deform, attachment, event or draw-order changes | Manual §12, §14, 28 | Timeline ownership, key interpolation, event timing, sequence exposure and track mixing |
| Make a motion feel smoother or more forceful | Manual §13, §15.1, 28 | Dopesheet spacing, Graph tangents, Ghosting arcs, holds and boundary velocity |
| Dialogue, sound cue or beat synchronization | Manual §14, 28 | Event names, audio timing, runtime event handling and cancellation behavior |
| PSD import or layered artwork | Manual §16, 29 | Import settings, tags, pivots, layer order, hidden regions and reimport authority |
| Export JSON, binary, atlas, image, GIF, video or HTML | Manual §17, 29 | Matching editor/runtime version, actual output names, alpha, duration, loop and resource paths |
| Pack or unpack textures | Manual §18, 29 | Atlas pages, PMA/alpha, trimming, padding, region origins and all referenced files |
| Automate Spine exports or batch work | Manual §19, 29 | CLI version, exit status, config paths, graphical-session limits and output inspection |
| Web player, engine integration or runtime controls | Manual §21, 30 | Runtime compatibility, animation names, mixing, attachment paths, events and real playback |
| Reduce draw cost or diagnose performance | Manual §22, 30 | Target device, Metrics, texture dimensions, draw calls, instances, frame intervals and visual quality |
| Upgrade from 4.2 or interpret a 4.3 change | Manual §§1, 23–24 | Actual editor version, runtime major/minor, license tier, changelog and migration copy |

## High-value 4.3 checks

- **Version first:** editor, launcher, license edition, exported data, and runtime versions are separate facts. The manual's “current” facts are date-bound.
- **Essential versus Professional:** do not reuse old feature comparisons. In 4.3, some older IK and skin-bone restrictions changed; check the target build and license.
- **Setup versus Animate:** structure, mesh, skin, and constraint setup changes have a different authority and risk from animation timeline changes.
- **Runtime evidence:** an editor preview or successful HTTP response does not prove runtime integration, transitions, or mobile performance.
- **Feature availability:** a visible editor control does not prove that a current action uses it. Confirm the active mode, selected object, enabled animation, key timeline, and advancing time.
- **Defaults:** treat undocumented editor defaults and hard limits as unknown until the target build or official documentation verifies them.

## Editor-operation checklist

When the request involves a feature in this index, operate the installed Spine editor or use a user-visible editor session when possible:

1. Confirm the target editor version and license edition.
2. Open a copy or a fresh candidate project before structural experiments.
3. Navigate to the relevant Setup/Animate panel and record the selected object and active animation.
4. Make the smallest change that tests the feature, then save and reopen the project.
5. Export matching runtime data and inspect it in the target player when delivery depends on it.
6. Record what was actually tested, what was inferred from the manual, and what remains unverified.

## Official sources

For current behavior, consult the official [User Guide](https://esotericsoftware.com/spine-user-guide), [Editor documentation](https://esotericsoftware.com/spine-editor-documentation), [4.3 changelog](https://esotericsoftware.com/spine-changelog), [versioning rules](https://esotericsoftware.com/spine-versioning), [CLI documentation](https://esotericsoftware.com/spine-command-line-interface), and [runtime guide](https://esotericsoftware.com/spine-runtimes-guide). Use the local manual to locate a topic, not to override a newer official source or an observed target-editor result.
