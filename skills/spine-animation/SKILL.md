---
name: spine-animation
description: Create, edit, and repair Spine 2D skeletal animation for character acting, combat, locomotion, machinery, transformations, and attached effects, from reference and rigging through visual review and editable delivery. Use for Spine animation tasks, not general video editing or CSS motion.
---

# Spine Animation

Aim for readable intent, appealing poses, convincing form, purposeful timing, and an editable result. Smoothness comes from coherent motion and timing: impacts can accelerate abruptly or hold, and mechanisms can stop in stages. Choose interpolation to express the action.

## Start with the current work

Read the current request, any `PROJECT.md`, the authoritative `.spine` project, and its actual preview. Separate identity, style, and motion references. Preserve the parts the user has accepted. Later rejection supersedes earlier delivery claims; later acceptance updates the aesthetic status of that specific version, while untested technical claims remain separate.

In a new environment, prove a small round trip: import an image and an animation, edit/save/reopen in the native editor, then export and actually play the result. Reuse an already demonstrated environment. For version, format, automation, or delivery questions, read [Editor and delivery](references/editor-delivery.md).

Clarify only omissions that materially change the work, such as intent, immutable design features, required views, or transitions. Proceed with routine reversible work. Treat local feedback as an addition to the current task and retain other approved effects.

## 1. Define the action

Record a short motion brief:

- Subject, purpose or emotion, weight and material, camera angle, and intended on-screen size.
- Required actions and their start/end states: looping, one-shot, interruptible, or connected to other actions.
- Essential readable moments, primary motion, contact or attachment points, occlusion, and changes of view.
- Timing references and provisional assumptions; distinguish animation timeline rate, runtime frame rate, and preview export rate.

Choose meaningful phases such as anticipation, action, extremes, follow-through, holds, and recovery. For video references, inspect timestamped poses rather than inferring timing from the title or description. For posing, timing, rig choices, or blending, read [Motion and form](references/motion-and-rig.md).

**Ready to refine:** key poses and primary motion communicate the intent, and the timing has a reason.

## 2. Build assets and a rig that serve the motion

Reconstruct the static character first. Compare silhouette, proportions, lighting, volume, and depth order with the master image, then test the intended extremes. Place pivots at the anatomical or mechanical rotation point, not the crop rectangle's center.

Preserve usable artwork and complete forms. Consider weighted meshes for continuous flexible surfaces, separate rotating pieces for rigid or naturally segmented parts, and replacement attachments or sequences for substantial turns, expressions, or shape changes. Select features for their visual purpose.

- Complete hidden regions and retain joint overlap. Inspect exposed gaps, baked-in old limb fragments, seams, and local lighting during motion.
- Keep coordinate and crop records. Distinguish master-image, world, parent-local, and attachment-center coordinates. Verify actual dimensions and alpha.
- Coordinate the motion of overlapping surfaces that should remain connected. Normalized weights can still flatten muscles, create corners, or drag the abdomen.
- Follow the current environment's image-generation and editing rules. Use programmatic cropping or resizing only when explicitly allowed. Carry existing permission within its original task scope; this case study does not grant permission for future projects.

**Ready to animate:** the assembly preserves the intended identity, and extreme poses retain deliberate volume and connections. Exaggerated deformation must have a performance purpose.

## 3. Establish the main action before polish

Block the key poses and check readability, then refine transitions, arcs, speed changes, action beats, and recovery. Small amplitudes or dense keyframes do not automatically produce better motion. Adjust poses and timing before increasing sampling density.

Once contacts, grips, and motion transfer work, add follow-through, drag, overlap, expressions, cloth, hair, accessories, breathing, and effects. Scale their amplitude and delay to the subject's weight, material, and action intensity.

For loops, inspect direction and velocity across the boundary as well as matching endpoints. For one-shots, inspect entry and settling; for interactive actions, test actual mixing, interruption, and return states. Use Graph, Ghosting, Weights, IK, Transform, Path, Physics, Deform, attachment changes, and draw order as needed. Record only features actually used and verified.

For stiffness, excessive deformation, seams, attached effects, or regressions, read [Diagnosis and lessons](references/diagnosis-and-lessons.md).

## 4. Decide from the rendered motion

Inspect the native editor and final playback environment. Watch the full subject at normal speed first, then slow motion, close-ups, and interpolated poses. Both full-view readability and local integrity must hold. Observe at least three consecutive cycles for loops; watch one-shots from trigger through settling, including adjacent actions when transitions matter.

Cover the entire interval relevant to a complaint. Inspect short loops frame by frame when useful. For long performances, cover each phase and its transitions, sampling problem intervals more closely. Retain timestamps and evidence of what was actually viewed.

Numerical checks help find missing references, non-finite values, flipped triangles, anchor drift, and boundary discontinuities. **Zero flips, small IK error, or high FPS do not establish visual quality.** Continue fixing visible problems in volume, posing, occlusion, force, or timing even when structural checks pass.

If successive edits fail to improve the image, return to the last accepted appearance or pose, isolate the smallest failing interval, and compare one major variable. After rejection, identify the visible problem and its cause instead of repeating earlier success claims.

## 5. Save and deliver

Follow [Editor and delivery](references/editor-delivery.md) for versioning, asset paths, runtime data, previews, and performance. A full production task normally delivers the editable project, relative-path images, runtime package, and a playable review HTML. Reuse an existing delivery convention for local repairs. Honor narrower requests such as diagnosis only.

Keep one current state at the top of `PROJECT.md`: authoritative files, versions, actions, accepted appearance, specific issues, observed evidence, limitations, and next steps. Label history below it so competing “current version” statements cannot accumulate.

Track four statuses separately: file/structural validation, observed visual review, target-device performance, and user aesthetic feedback. Mark a check passed only when performed. Record unreceived feedback as pending while completing the remaining authorized delivery; it does not require an extra formal approval pause.
