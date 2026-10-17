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

## 2026-10-17 — Expand 3D/CAD MCP & Blender 5.2 addon ecosystem
**Decision:** Install and wire the audited 3D/CAD MCP servers and Blender 5.2 addons into `~/.local/share/` and `~/Library/Application Support/Blender/5.2/`, scoped per project:
- **`build123d` MCP (`@casys/mcp-build123d` 0.7.1)**: Installed at `~/.local/share/mcp-build123d` with dedicated Python 3.12 `.venv` (`build123d==0.11.1`, `cadquery-ocp-novtk==7.9.3.1.1`) driven via Deno stdio.
- **`bonsai-mcp` (`JotaDeRodriguez/Bonsai_mcp` 0.1.0 + Blender `bonsai` 0.8.5 BIM extension)**: Installed official `bonsai` extension in Blender 5.2 and patched `Bonsai_mcp` (`~/.local/share/bonsai-mcp`, addon `bonsai_mcp_addon.py`) to port `9878` and `bonsaimcp.*` operator IDs so it does not collide with `blender-mcp` (9876) or `blender-official` (9877).
- **`ncmlabs/fusion360_mcp` (0.1.2) over `Poyraxx/fusion-advanced-mcp-server`**: Selected `ncmlabs/fusion360_mcp` (MIT, 135/135 pytest passing, HTTP port `5001`, zero `eval()`); rejected `Poyraxx` (GPL-3.0, Windows `winreg` installer, raw `eval()`). Installed at `~/.local/share/fusion360-mcp` + Fusion add-in at `~/Library/Application Support/Autodesk/Autodesk Fusion 360/API/AddIns/FusionMCP`, scoped to `Projects/fusion360` alongside official Fusion MCP (`27182`).
- **`DMontgomery40/bambu-printer-mcp` (1.1.11) over `dmikushin/orca-slicer-mcp`**: Installed `bambu-printer-mcp@1.1.11` at `~/.local/share/bambu-printer-mcp` (scoped to `Projects/bambulab-x2d`), which natively wraps macOS `OrcaSlicer` CLI + STL tools + X2D MQTT/FTPS. Skipped `dmikushin/orca-slicer-mcp` (requires Arch Linux `makepkg` C++ source patch, `Xvfb`, `openbox`, `xdotool`, `scrot`, and Linux DBus IPC).
- **`mesh-ai-mcp` (0.1.0 — Part Segmentation & Auto-Retopology)**: Created `~/.local/share/mesh-ai-mcp` backed by local Apple Silicon arm64 wheels (`coacd==1.0.14`, `pyacvd==0.4.0`, `pyvista==0.49.1`, `trimesh==5.1.1`, `manifold3d==3.5.4`) + headless Blender Quadriflow + `gradio-client` cloud bridge for CUDA-only `PartField`, `SAMPart3D`, and `MeshAnythingV2` Spaces.
- **`alexisrolland/ComfyUI-Blender` (4.5.1) + `joenorton/comfyui-mcp-server`**: Installed `comfyui_blender` addon (with vendored `websocket-client`) into Blender 5.2 and `comfyui-mcp-server` into `~/.local/share/comfyui-mcp-server`.
- **Gaussian Splatting & LiDAR-to-Blender**: Installed `Kiri-Innovation/3dgs-render-blender-addon` (5.1.0) and `Projects/iphone-lidar/blender/lidarlink_blender.py` into Blender 5.2 addons.
**Why:** Adds parametric CAD-as-code, OpenBIM/IFC floorplans, part segmentation, isotropic/quad auto-retopology, ComfyUI closed-loop bridges, and 3DGS/LiDAR workflows while preserving port isolation, `mcp<2` SDK compatibility, and macOS arm64 compatibility.
**Alternatives considered:** Installing `Poyraxx/fusion-advanced-mcp-server` (Windows-coupled), `dmikushin/orca-slicer-mcp` (Linux Xvfb/DBus-only), or `Show2Instruct/ifc-bonsai-mcp` (bundles `posthog` telemetry).

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
