<p align="center">
  <img src="assets/logo.png" alt="Spine Animation: articulated S-shaped motion emblem" width="116">
</p>

<h1 align="center">Spine Animation</h1>

<p align="center"><strong>Expressive motion. Convincing form. Editable delivery.</strong></p>

<p align="center">An agent skill for creating and repairing Spine 2D skeletal animation.</p>

<p align="center"><strong>English</strong> · <a href="README.zh-CN.md">简体中文</a></p>

<p align="center">
  <a href="#install">Install</a> ·
  <a href="#use-it">Usage</a> ·
  <a href="#example-hog-rider">Example</a> ·
  <a href="skills/spine-animation/SKILL.md">Read the skill</a>
</p>

## Example: Hog Rider

<p align="center">
  <img src="examples/hog-rider/run.gif" alt="Hog Rider running with a war hammer, coordinated rider motion, and breath emitted from the boar's nostrils" width="553">
</p>

<p align="center"><em>User-approved v008 · Native Spine export · 25 FPS looping GIF</em></p>

This project shaped the skill through real revision cycles: distorted limbs, stiff movement, lost shoulder and hip volume, and breath that crossed the nose instead of emerging from its openings. The accepted repair restored the character's form, coordinated the limb roots, refined timing, and anchored each breath stream inside its nostril.

The [case study](skills/spine-animation/references/diagnosis-and-lessons.md#validated-case-hog-rider-v008) records the fixes and their verification limits. The GIF is a compact showcase; its binary transparency does not reproduce the runtime's full translucent smoke edges. This example demonstrates one branch of the workflow, not a universal gait preset.

## What it covers

| Animation family | Focus |
| --- | --- |
| Character acting | Poses, emotion, gaze, expressions, dialogue, purposeful holds |
| Combat and reactions | Anticipation, action beats, recoil, recovery, hits, death, interruption |
| Locomotion and jumps | Support, propulsion, arcs, balance, landing, rider and prop response |
| Machinery | Pivots, rigid parts, transmission, contact, travel limits |
| Flexible forms | Cloth, hair, tentacles, inertia, propagation, controlled deformation |
| Turns and transformations | Replacement views, attachment changes, silhouette, occlusion |
| Effects | Source alignment, local/world space, emission, detachment, dissipation |

The skill guides reference analysis, asset preparation, rig selection, pose and timing design, visual diagnosis, native-editor checks, and delivery. It chooses FK, IK, meshes, constraints, and secondary motion according to the shot. Smoothness includes deliberate acceleration, impact holds, and mechanical stops.

**This repository contains agent instructions.** It does not install Spine, provide a runtime SDK, or guarantee one-click animation generation. An agent needs the appropriate tools and access to perform the requested production work.

## Install

Choose one method. The installable package is `skills/spine-animation/`; its references travel with it. The README artwork, example GIF, and optional full Chinese manual are outside the installed skill.

### 1. Command line

With Node.js and npm/npx available, run:

```sh
npx skills add zxzAndyMAC/Spine-SKILL --skill spine-animation
```

The installer offers agent and scope selection. To target a user-level installation explicitly:

```sh
# Codex
npx skills add zxzAndyMAC/Spine-SKILL --skill spine-animation --agent codex --global

# Claude Code
npx skills add zxzAndyMAC/Spine-SKILL --skill spine-animation --agent claude-code --global
```

Omit `--global` for a project-level install. Inspect discovery before installing with:

```sh
npx skills add zxzAndyMAC/Spine-SKILL --list
```

These commands use the [Skills CLI](https://github.com/vercel-labs/skills), not a custom installer. Local discovery and installation were checked with CLI 1.7.1. Other agents supported by that CLI can use the interactive selection; actual animation capabilities depend on their available tools.

### 2. Natural language in your agent

Paste this into an agent that can install skills and access GitHub or a local checkout:

```text
Install the spine-animation skill from https://github.com/zxzAndyMAC/Spine-SKILL
for this agent at user scope. The skill is in skills/spine-animation/.
Install that entire directory, including references and agents metadata.
Use your supported skill installer or the Skills CLI. If a skill with the same
name already exists, show the difference before replacing it. Verify that the
skill is discoverable afterward. Do not install Spine or run the example project.
```

A chat interface without filesystem or installation tools cannot perform the installation. After installing, refresh skill discovery or start a new session if your agent requires it.

### 3. Clone and install locally

```sh
git clone https://github.com/zxzAndyMAC/Spine-SKILL.git
cd Spine-SKILL
npx skills add . --skill spine-animation --agent codex --global
```

For a manual installation without Node.js, copy the **entire** `skills/spine-animation` folder to your agent's skill directory:

| Agent | User-level destination |
| --- | --- |
| Codex | `$CODEX_HOME/skills/spine-animation` when set; otherwise `~/.codex/skills/spine-animation` |
| Claude Code | `~/.claude/skills/spine-animation` |
| Other agents | Their documented skill directory, preserving the complete folder |

For example, from the cloned repository, this Python 3 command installs for Codex and refuses to overwrite an existing destination:

```sh
python3 - <<'PY'
import os
import shutil
from pathlib import Path

codex_dir = Path(os.environ.get("CODEX_HOME") or Path.home() / ".codex").expanduser()
destination = codex_dir / "skills" / "spine-animation"
destination.parent.mkdir(parents=True, exist_ok=True)
shutil.copytree("skills/spine-animation", destination)
print(f"Installed: {destination}")
PY
```

If the destination already exists, compare it and back up your edits before choosing how to update. Copying only `SKILL.md` leaves the reference links broken.

## Use it

In Codex, invoke `$spine-animation`. In other agents, use their skill invocation syntax or ask them to use the installed Spine Animation skill. Requests can be in English, Chinese, or another language even though the instructions are in English.

```text
Use $spine-animation to animate this character casting a heavy spell:
anticipation, release, recoil, and recovery. Preserve the silhouette and deliver
an editable Spine project plus a playable preview.
```

```text
Use $spine-animation to repair this mechanical arm. Its gripping point slips
and the elbow snaps during retraction. Preserve the accepted timing and artwork.
```

```text
Use $spine-animation to create a curious-to-startled facial performance, with
coordinated eyes, brows, and head movement. Keep the acting readable at game size.
```

For useful results, supply the current project or artwork, motion intent or reference, features that must stay unchanged, and the target view or playback environment. For repairs, identify the affected version, region, and time interval when possible.

## Production workflow

1. **Read the current work.** Separate identity, style, and motion references; preserve accepted results.
2. **Design the action.** Establish intent, key poses, contacts, timing, and start/end behavior.
3. **Preserve form.** Reassemble the artwork and test rig extremes before adding polish.
4. **Build motion.** Refine the main action, then arcs, curves, overlap, and effects.
5. **Inspect playback.** Review full-view motion, problem areas, in-betweens, and boundaries in the editor and target player.
6. **Deliver editable work.** Save the authoritative project, verify paths, and distinguish tested results from pending feedback.

Typical full-production outputs are an editable `.spine` project, relative-path source images, runtime exports, a playable review HTML, and a concise `PROJECT.md`. A diagnosis-only or local-repair request retains its narrower scope.

## Requirements and compatibility

- An agent supporting folder-based skills, with filesystem and shell access for project work.
- Spine Editor and an appropriate license/edition for the features and saving operations you need. Installation of this skill does not provide either.
- Native UI automation or human assistance for editor operations and visual verification; image-generation/editing tools when creating or repairing raster artwork.
- A browser and compatible Spine runtime/player when browser delivery is requested. Check editor/runtime compatibility for each project.

The example was authored in **Spine 4.3.26**. The skill is not pinned to that version and instructs agents to verify the target environment. Desktop playback does not establish mobile performance; a preview transition does not establish game-engine integration.

## Repository layout

```text
skills/spine-animation/
├── SKILL.md                         # Workflow and reference routing
├── agents/openai.yaml               # Codex UI metadata
└── references/
    ├── motion-and-rig.md            # Action design, rig choices, transitions
    ├── diagnosis-and-lessons.md     # Visual faults, effects, accepted case
    ├── editor-delivery.md            # Versions, round trips, playback, handoff
    └── spine-4.3-feature-index.md   # Route uncommon editor features
assets/                             # README logo and generation prompt
examples/hog-rider/                  # Approved animation showcase
docs/Spine-4.3-complete-manual.zh-CN.md # Optional full 4.3 reference
README.md                           # English, default
README.zh-CN.md                      # Simplified Chinese
```

The full manual stays under `docs/` for on-demand lookup. It is intentionally not loaded with the skill by default; the feature index points to the sections that matter for a particular request.

## Contributing

Open an issue or pull request with the observed failure, editor/runtime versions, expected result, and reproducible evidence. Keep fixes scoped to a demonstrated problem, preserve multi-action coverage, and update both READMEs when installation or public behavior changes. Do not submit proprietary artwork without permission.

## License and credits

The skill instructions and repository documentation are under the [MIT License](LICENSE). Artwork and animation files are excluded from that license; see [asset notes](ASSET-NOTICES.md).

This is an independent community project, unaffiliated with Esoteric Software or Supercell. Spine is a product of Esoteric Software. The Hog Rider example is an AI-assisted fan-art animation study inspired by *Clash Royale*; character and game references belong to their respective owners. It is shown as a workflow example, not as a reusable game-asset license.
