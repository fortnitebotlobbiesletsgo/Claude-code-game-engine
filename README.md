# Claude Game Director Plugin

An engine-aware game-development assistant for Claude Code, exposed as a local MCP server. It provides structured tools for visual-fidelity planning, shader briefs, game-feel design, architecture blueprints, and engine capability guidance.

> **Important:** this repository is a Claude Code plugin repository plus an MCP server. Claude Code plugin installation is driven by `.claude-plugin/plugin.json`; MCP server configuration belongs in `.mcp.json`. A standalone `mcp.json` is not a substitute for the Claude Code plugin manifest.

## Requirements

- Node.js 20.11+ (Node 22 LTS recommended)
- npm
- Claude Code with plugin support

## Install from GitHub

1. Publish this repository to GitHub.
2. Clone the repository, or load its plugin directory with Claude Code's `--plugin-dir` option for local testing. A GitHub marketplace install requires a marketplace catalog (`.claude-plugin/marketplace.json`) that points to a plugin source; that catalog is intentionally separate from this single-plugin repository because Claude Code uses different manifests for a plugin and a marketplace.
3. Build the MCP server once in the repository directory:

   ```bash
   npm install
   npm run build
   ```

5. Restart Claude Code or reload the plugin.

The plugin manifest and `.mcp.json` follow Claude Code's plugin layout. The MCP server command expects `dist/index.js`, so dependencies must be installed and the TypeScript project built before first use. No install script is required.

### Local development

```bash
npm install
npm run dev
```

For a production build:

```bash
npm run check
npm test
npm run build
npm start
```

## MCP tools

| Tool | Purpose |
|---|---|
| `visual_fidelity_plan` | Palette, lighting, quality tiers, engine-specific visual guidance |
| `shader_implementation_brief` | Shader requirements and engine-specific implementation route |
| `game_feel_plan` | Input response, anticipation, impact, recovery, accessibility |
| `architecture_blueprint` | Modular architecture and repository layout |
| `engine_capabilities` | Supported engine targets and capabilities |

## Example prompts

- “Create a cinematic exterior visual-fidelity plan for Unreal Engine 5.”
- “Design a stylized toon shader brief for Godot 4.”
- “Plan the game feel for a heavy melee attack in an action RPG.”
- “Propose a large-scale Unity architecture for a co-op survival game.”

## Transport and security

The shipped entrypoint uses MCP **stdio transport**, the most straightforward transport for a local Claude Code plugin process. It does not expose a network listener. The requested legacy SSE transport is intentionally not enabled: current MCP deployments should use the SDK's supported Streamable HTTP transport when a remotely hosted server is actually needed. Add that as a separately secured deployment target rather than exposing an unauthenticated local HTTP endpoint.

The server does not execute generated code, patch project files, invoke an engine, or download assets. It returns plans and implementation briefs. `CodePatcher` is a conservative pure-string helper and is not wired to filesystem writes.

## Project layout

```text
.claude-plugin/plugin.json   Claude Code plugin manifest
.mcp.json                   MCP server configuration
.claude/rules/              Project guidance loaded by Claude Code
src/config/                 Palettes and lighting presets
src/engines/                Engine-specific guidance
src/tools/                  MCP tool implementations
src/utils/                  Safe text patch helpers
tests/                      Unit tests
.github/workflows/          CI build and test
```

## Versioning

Engine guidance deliberately names supported engine families rather than pretending one shader or project setting is compatible with every minor version. For compile-ready engine code, provide the exact engine version, renderer/pipeline, target platform, and project conventions.

## License

MIT. See [LICENSE](LICENSE).
