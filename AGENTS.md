# AGENTS.md — blender-mcp

Project-local instructions. Read `../../AI-Workspace/AGENTS.md` first for workspace-wide rules — this file only covers what's specific to this project.

## What this project owns
- Local clone and configuration for the Blender MCP server (`ahujasid/blender-mcp`).
- Python server implementation and Blender addon (`addon.py`) for AI-driven 3D modeling and scene manipulation.

## What it does not own
- 3D model assets for specific projects (e.g. `cad-projects`, `home-floorplan-3d`).
- Antigravity / Claude / Codex MCP client configuration (managed in `~/.gemini/config/mcp_config.json`, `~/.claude/claude_desktop_config.json`, etc.).

## Files to read first
1. `README.md`
2. This file
3. `TASKS.md`
4. `pyproject.toml`
5. `addon.py`

## Allowed tools
Standard Python toolchain (`uv`, `pytest`), git.

## Relevant MCP servers
`blender` (self-referential MCP server).

## Testing rules
Run test suite with `uv run pytest`.

## Deployment rules
See `../../AI-Workspace/00-AI-OS/Rules/DEPLOYMENT-STANDARD.md`.
Addon installation in Blender via Preferences -> Add-ons.

## Documentation update requirements
Update `TASKS.md`/`DECISIONS.md`/`CHANGELOG.md` as work happens — see `../../AI-Workspace/00-AI-OS/Rules/DOCUMENTATION-STANDARD.md`.

## Safety restrictions
Verify Blender RPC connection status before issuing bulk transform or delete operations.
