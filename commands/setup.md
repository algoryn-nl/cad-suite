---
description: Install cad-suite Python and Node dependencies into the current project.
---

You are setting up the `cad-suite` plugin for the project at the user's current
working directory. The plugin itself lives at `${CLAUDE_PLUGIN_ROOT}`.

Perform the following steps in order, reporting each one. Stop and ask the user
if any step needs a decision.

1. Verify Python 3.11 or newer is available:
   `python3.11 --version` (fall back to `python3 --version` and refuse if < 3.11).
   If unavailable, stop and ask the user to install it.

2. If `.venv/` does not exist in the project root, create it:
   `python3.11 -m venv .venv`

3. Upgrade pip in the venv:
   `./.venv/bin/python -m pip install --upgrade pip`

4. Install CAD skill Python dependencies:
   `./.venv/bin/pip install -r ${CLAUDE_PLUGIN_ROOT}/skills/cad/requirements.txt`

5. Ask: "Do you also need URDF tooling?" If yes:
   `./.venv/bin/pip install -r ${CLAUDE_PLUGIN_ROOT}/skills/urdf/requirements.txt`

6. Ask: "Do you need robot-motion (ROS 2 / MoveIt) on this machine?" If yes,
   point the user at `${CLAUDE_PLUGIN_ROOT}/skills/robot-motion/SKILL.md` and
   `${CLAUDE_PLUGIN_ROOT}/skills/robot-motion/environment.yml`. Do NOT
   auto-install — ROS 2 / MoveIt setup is environment-specific and the user
   needs to make decisions about conda vs apt vs source builds.

7. Install CAD Explorer Node dependencies:
   `npm --prefix ${CLAUDE_PLUGIN_ROOT}/skills/cad/explorer install`

8. Verify the venv can import the core CAD libraries:
   `./.venv/bin/python -c "import build123d, OCP; print('ok')"`
   If this fails, surface the error and suggest re-running step 4 or
   checking the user's Python build (OCP wheels require macOS 11+ / glibc 2.34+).

9. Print a summary:
   - Where the venv is (`./.venv`)
   - Whether URDF deps were installed
   - Where to read about robot-motion setup
   - How to launch CAD Explorer:
     `npm --prefix ${CLAUDE_PLUGIN_ROOT}/skills/cad/explorer run dev`
     then open http://localhost:4178

Do not modify project files outside of `.venv/` and the plugin's own
`node_modules`. Do not commit anything in the user's project on their behalf.
