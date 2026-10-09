# Diagnosis and lessons

Read for specific visual complaints about an existing animation. Reproduce the reported version, time, and region. Judge the problem in the full composition before enlarging it. Separate artwork, setup pose, rig, timing, occlusion, export, and player causes.

## From symptom to a small useful check

| Symptom | Compare or isolate first | Fix first |
| --- | --- | --- |
| Deformed, abstract, or no longer the same character | Master versus static assembly, then extreme poses; isolate deformation and constraints | Proportions, silhouette, pivots, weights; recover the last accepted complete artwork |
| Stiff or puppet-like | Force and timing at normal speed, joint synchronization in slow motion | Primary timing, joint coordination, arcs, follow-through; amplitude alone is insufficient |
| Ball-like joints or thin, shortened limbs | Full original limbs versus cut parts, including the area hidden by the torso | Muscle volume and continuous contours; reconsider mesh versus separate pieces for the material |
| Detached joints, tearing, exposed gaps | Attachment edges and weights at extremes | Hidden regions, overlap, parent coordinates, or matching weights before changing the whole action |
| Sudden reversal or twitch | Interpolation on both sides of the fault; isolate IK, Transform, and angle tracks | Reachability, bend branch, constraint order, angle wrapping, tangent overshoot |
| Duplicate limbs or fragments emerging from the belly | Display the body and moving limbs separately | Remove old limbs baked into the body; repaint or adjust the mesh contour appropriately |
| An earring or accessory piercing or leaving its anchor | Anchor, front/back arc, and head movement together | Pivot and inheritance; separate occlusion layers where needed |
| Effects stuck to a face, crossing a nose, or drifting at the source | Emitter, transparent padding, and visible attachment tip in the same local space | Anchor, region origin, hierarchy, occlusion; then spread and fade |
| Correct in the editor, misplaced or missing on the web | Runtime data and atlas from the same project state | Matched resources, PMA, versions, player settings before reauthoring motion |
| Passing metrics but rejected visuals | Original feedback against normal-speed full-view and close-up playback | Reidentify the visual cause; metrics only exclude the faults they measure |

Preserve unrelated approved tracks during local repairs. Resolve references by stable names and rebuild affected indices after changing bones or meshes. Array positions are not stable identities. Retain failed candidates and comparisons; isolate major changes so their effect is visible.

## Attached effects: choose the space first

For breath, steam, muzzle flashes, or eye effects, decide between a continuous shape following the emitter and particles that detach after birth. A continuous effect keeps its root at the source while its outer end spreads. A detached effect captures the source transform at birth and then moves in its intended space. Translating one entire smoke image usually cannot provide both a fixed source and a dissipating trail.

Character-root space still follows the character and is not host-world space. A detached effect needs an explicit owner for its birth transform, trajectory updates, stopping, and recycling. Without a host, a review page can demonstrate detachment from the head; label world-space behavior as awaiting integration.

Place the source inside the opening and aim along its outward direction. A narrow or tapered root should widen outside. Check the source through head or weapon translation and rotation, then inspect onset, peak, and dissipation. Give separate openings separate emitters instead of drawing one cloud across the surface between them.

Transparent padding is not the visible root. For an unrotated region attachment at unit scale emitting along local +X from its left edge, that edge is `region.x - width/2`. After aligning this boundary to the source, calibrate against the actual alpha tip. Use the complete parent transform for rotated or scaled hierarchies; this formula is not a world-space shortcut.

Where needed, cover the buried root with a small opening-rim patch that deforms with the body, or use appropriate clipping. Keep the patch small and match color and lighting. Triangulate non-convex meshes correctly instead of using an arbitrary triangle fan. Inspect translucent overlaps on light and dark backgrounds; raising opacity cannot correct a misplaced source.

## Validated case: Hog Rider v008

Source: iterative production and user feedback on October 9, 2026. After v008 delivery and LAN preview access, the user explicitly accepted the result. This is one mounted quadruped case, not a parameter preset for other actions.

The failure sequence was deformation, then stiffness after restricting motion, then loss of shoulder and hip volume from short segmented limbs and an enlarged torso used to hide joints. Zero triangle flips and small endpoint errors did not prevent rejection.

The effective repair restored the master image's substantial shoulders, hips, and complete leg artwork. Repositioned hind-leg joints and continuous weighted meshes coordinated torso and limb roots. Old limb fragments were removed from the body, and recovery poses and local topology were revised. This shot ultimately used FK pose tracks while retaining earring physics; more constraints were not the solution.

The original breath attachment straddled its source and drew a white streak across the nose. The repair aligned each visible starting tip inside its own nostril, removed root translation drift, adjusted the two directions and spread, and added small nostril-rim occluders. A close-up exposed a far-nostril tip still sitting on the edge; it was moved inward before delivery.

Evidence covered 33 native-export gait detail frames, five enlarged breath phases, reopening and playing a copied native project, and browser playback. A 1,536-frame no-flip check was supporting structural evidence. Desktop performance near 120 FPS did not establish mobile-device performance. Host movement speed and world-space foot sliding were not scene-integration tested.

The reusable lesson is to preserve form, design motion relationships, then refine local details. Acceptance belongs to that version and request. Transfer the judgment process, not its 0.64-second cycle, angles, mesh coordinates, or sample counts.
