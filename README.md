# cad-suite

A Claude Code plugin that bundles three skills for parametric CAD,
robot-description, and robot-motion work, plus the CAD Explorer viewer.

The skills come from [text-to-cad](https://github.com/earthtojake/text-to-cad);
this repository repackages them as an installable Claude Code plugin so they
can be used in any project without cloning the harness.

## What's inside

| Skill          | Purpose                                                                            |
| -------------- | ---------------------------------------------------------------------------------- |
| `cad`          | STEP-first build123d/Python CAD generation, inspection, and `@cad[...]` references |
| `urdf`         | URDF generation, joint/link validation, mesh references                            |
| `robot-motion` | ROS 2 / MoveIt 2 inverse kinematics, path planning, motion server tooling          |

The CAD skill ships with the **CAD Explorer** Vite/React viewer under
`skills/cad/explorer/` for browsing generated geometry.

## Install

Add the marketplace once:

```
/plugin marketplace add https://github.com/algoryn-nl/cad-suite
```

Then install the plugin:

```
/plugin install cad-suite
```

## First-run setup

After installing, run the bundled setup command from inside the project where
you want to author CAD:

```
/cad-suite:setup
```

This creates `.venv/` in your project, installs the Python CAD dependencies,
optionally installs URDF dependencies, and runs `npm install` for the CAD
Explorer. Robot-motion (ROS 2 / MoveIt) is not auto-installed — see
`skills/robot-motion/SKILL.md`.

## Manual setup

If you'd rather install dependencies by hand:

```bash
# Python deps (CAD)
python3.11 -m venv .venv
./.venv/bin/pip install --upgrade pip
./.venv/bin/pip install -r ${CLAUDE_PLUGIN_ROOT}/skills/cad/requirements.txt

# Optional: URDF
./.venv/bin/pip install -r ${CLAUDE_PLUGIN_ROOT}/skills/urdf/requirements.txt

# CAD Explorer (Node)
npm --prefix ${CLAUDE_PLUGIN_ROOT}/skills/cad/explorer install
```

## Launch CAD Explorer

```bash
npm --prefix ${CLAUDE_PLUGIN_ROOT}/skills/cad/explorer run dev
```

Then open http://localhost:4178.

For multi-project workflows that share a single Explorer:

```bash
npm --prefix ${CLAUDE_PLUGIN_ROOT}/skills/cad/explorer run dev:ensure -- \
  --file STEP/sample_part.step
```

## Layout

```
cad-suite/
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── skills/
│   ├── cad/           # CAD skill + explorer/ + scripts/
│   ├── urdf/
│   └── robot-motion/
├── commands/
│   └── setup.md       # /cad-suite:setup
└── README.md
```

## License

MIT. Each bundled skill keeps its own LICENSE under `skills/<name>/LICENSE`.

## Source

Skills are sourced from
[earthtojake/text-to-cad](https://github.com/earthtojake/text-to-cad). For
upstream development, file issues there. For plugin-packaging issues, file
them here.
