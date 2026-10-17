# Decisions — blender-mcp

Dated decision records, most recent first.

## 2026-10-17 — Fork audit against the 2026-08-10 upstream compromise window
**Decision:** No removal or revert needed; harden configuration instead (see TASKS).
**Findings:**
- Fork history (198 commits, HEAD `ac26412`) was diffed from `3ab8925` (2026-08-03) onward: 25 files, no new hosts beyond expected integrations (PolyHaven, Sketchfab, Poly Pizza, Hyper3D, fal.run), no `curl|sh`, base64 payloads, or obfuscation; the only `exec` is the existing `execute_blender_code`. Pattern scan, not a line-by-line audit. Commits are unsigned, so authorship cannot be cryptographically verified.
- Telemetry: addon `telemetry_consent` defaults to `True` (despite the 2026-07-21 opt-in commit, reverted by `cc3129c` "modify telemetry"). `src/blender_mcp/config.py` (Supabase endpoint) is gitignored and absent in this clone, so this clone cannot send; the PyPI wheel can.
- Safe mode is opt-in (`BLENDER_MCP_SAFE_MODE=1`) and lacks the upstream #365 fix (only legacy `script_directory` is blocked; `script_directories` and `bpy.ops.extensions.*` are not).
- Gemini/Antigravity config (`~/.gemini/config/mcp_config.json`) runs `uvx blender-mcp`, i.e. PyPI, not this clone. PyPI `blender-mcp` 2.0.0 is a shim for `mcp-for-blender`; cached 2.1.9 code has two Supabase hosts: telemetry, and a paid "premium" image-to-3D API (`PREMIUM_API_URL_DEFAULT`). No other unexpected hosts.
**Alternatives considered:** Resetting the fork to a pre-Aug-10 commit (no evidence of malicious changes, would lose safe mode and Poly Pizza).

## 2026-10-17 — Evaluate alternative Blender MCP servers side by side
**Decision:** Run the official Blender Lab MCP (`blender-official`, port 9877) and seehiong's `blender-mcp-bridge` (`blender-bridge`, addon 127.0.0.1:8888, server 127.0.0.1:8008) alongside this fork (port 9876). Add the `blender-kiln` Claude Code plugin and `gltf-transform`/`gltfpack`. Skip `dcc-mcp-blender`: its installer fetches an unauditable prebuilt CLI and auto-installs a second binary (`dcc-cua`) with `--yes`; it duplicates the servers already installed.
**Why:** Upstream `ahujasid/mcp-for-blender` had a maintainer-account compromise (HN, 2026-08-10), safe-mode gaps (#365), default-on telemetry (#360), and Blender 5.1+/5.2 connection reports (#339). The official server is small, local-only, and Foundation-maintained.
**Notes:**
- Official package is also named `blender-mcp` on PyPI; installed from a reviewed clone at `~/.local/share/blender-lab-mcp/mcp`, never via `uvx blender-mcp`.
- Bridge defaults bind `0.0.0.0` (addon and server); both forced to `127.0.0.1`. Its repo `uv.toml` pins `F:/uv-cache`, so run with `uv --no-config`. Its addon reuses `blendermcp.*` operator ids and displaces this fork's addon operators in a shared session.
- Third-party code was pattern-scanned only (no `curl | sh`, base64 payloads, odd hosts), not fully audited.
**Alternatives considered:** Staying on the fork only; adopting a single replacement.

## 2026-09-04 — Standardize project governance
**Decision:** Standardize repository structure with universal `AGENTS.md` and `@AGENTS.md` in `CLAUDE.md`.
**Why:** Brings `blender-mcp` into uniform compliance across all AI CLI tools.
**Alternatives considered:** Leaving as an untracked vendor clone without workspace governance.
