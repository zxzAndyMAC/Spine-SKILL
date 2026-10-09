# Motion and form

Read when designing poses, choosing a rig, or diagnosing motion that flows smoothly but feels wrong. These are branches to select from, not a feature checklist for every project.

## Organize timing around intent

| Action | Establish first | Misleading substitute for quality |
| --- | --- | --- |
| Attack, spell, throw | Anticipation direction, charge, action beat, balance, recoil, recovery; align hit events with poses | Identical easing throughout, no speed or hold at impact, floating weapons |
| Hit reaction, death | Force source, transmission order, loss of support, recoverability, terminal state | Every part swaying in phase, a death pose bouncing back to the start |
| Jump, landing | Compression against support, takeoff, airborne trajectory, impact absorption, settling | A single vertical sine wave with body extension unrelated to flight |
| Idle, expression, dialogue | Emotion, gaze, breathing and pauses, relationships between mouth, eyes, and brows | Constant motion everywhere, metronomic blinks, stretching the mouth as the only expression |
| Biped or quadruped movement | Support, propulsion, flight, recovery, center of mass, referenced gait | Independent limb swings, reversed knees, constant-speed paddling |
| Machinery | Pivots, rigidity, transmission/contact relationships, locks, travel limits | Arbitrary stretching of rigid pieces, mismatched gears or contacts |
| Soft bodies, tentacles, cloth | Driving end, wave propagation, inertia and decay, volume, material | Every segment moving in phase, accumulated delays collapsing the form |
| Turns, transformations, special performances | Shapes needing another view or topology, switch timing, silhouette, occlusion | Forcing a single front-facing image through extreme perspective or unlimited stretch |
| Attached or independent effects | Emission space, source, direction, continuous versus detached behavior, onset and dissipation | Surface stickers, drifting sources, detached effects still dragged by their parent |

An action can span several rows. Establish readability with a few meaningful poses, then allocate transition time. Let the action determine the number of keys.

## Choose a driving method

| Need | Consider | Inspect directly |
| --- | --- | --- |
| Clear poses, silhouettes, rigid swings | FK rotation/translation and curves | Joint arcs, balance, contact phases; endpoint paths still need design |
| Feet, hands, or tools approaching a defined target | IK, plus endpoint orientation when needed | Reachability, bend branch, inherited transforms, order, overextension, mixing |
| Grips or linked proportions/orientations | Parenting or Transform constraints | Double transforms, local/world semantics, switching pops |
| Continuous torso or soft-limb deformation | Weighted meshes, with limited Deform where useful | Volume, seams, UVs/topology, extremes and in-betweens |
| Movement along a defined curve | Path constraints | Path shape, speed, tangent direction, attachment spacing |
| Inertial accessories or hair | Physics or authored secondary tracks | Warm-up, multiple cycles, pause/resume, teleports, stability after mixing |
| Changing views, expressions, or topology | Attachment swaps, replacement artwork, sequences | Volume and placement across the switch, exposure duration, depth continuity |

FK and IK are tools. A successful FK repair is not a universal reason to avoid IK. Enabling IK alone does not prove a contact is locked.

## Curves and transitions

Inspect position and angle over time together with the actual screen-space trajectory. Where supported, use Graph for tangents and velocity, and Ghosting for spacing and arcs. Brief acceleration, readable holds, delayed follow-through, and sufficient recovery can communicate more force than uniformly smooth interpolation.

Soft continuous motion usually needs natural boundary velocities. Designed collisions, rebounds, and impact holds can deliberately change velocity abruptly. Dense sampling increases editing cost and cannot replace pose design. Retain the authoring basis for baked tracks; compare trajectories again after reducing keys.

Host mixing durations and overlay tracks can change the result. Verify the starting setup pose, attachments, draw order, constraint mixes, physics state, and recovery after interruption. Interrupt during anticipation, on either side of the action beat, and during recovery. Distinguish events not yet fired from independent effects already spawned; cancel, continue, or recycle according to the user's rules. State missing rules as assumptions and clarify those affecting gameplay. Record preview simulations separately from actual host state-machine testing.

## Volume and seams

Identify volumes that must remain stable and those allowed to squash or stretch. Preserve the character of muscles, shoulders, thin ankles, and hooves instead of replacing every form with capsule-shaped pieces for rigging convenience. Exaggeration should express consistent material and intent.

Vertices on overlapping surfaces at the same canvas position can use coordinated weight fields to reduce sliding. Establish consistent bind poses and coordinates before adjusting weight falloff. Spreading limb weights across the whole torso can turn muscle movement into abdominal collapse.

Arrange topology along the intended bending direction; inspect thin or nearly collinear triangles and joint sampling. Check UVs and bind positions together when changing a mesh. Use masking or clipping for real occlusion, preserving the body silhouette instead of expanding it to conceal incorrect limbs. Baked lighting can create visible tonal seams even when geometry stays connected.

## Locomotion branch

Identify the referenced walk, run, sprint, bound, or stylized gait. Record each limb's contact, loading, push-off, and recovery. Choose front/back and near/far phase relationships from reference and readability rather than applying one sine wave or mandatory diagonal pairing.

In an in-place cycle, a supporting foot moves backward relative to the body. Once placed in a moving scene, compare root motion or scene speed with world-space foot movement. Claim no sliding only after scene-level testing. Body bounce and pitch, rider lag, and prop response should follow changes in support.
