# Claude Cowork Plugins

A collection of plugins for [Claude Code](https://docs.anthropic.com/en/docs/claude-code) that bundle MCP server integrations with custom skills.

## Available Plugins

| Plugin                 | Description                                                                   |
|------------------------|-------------------------------------------------------------------------------|
| [lucid](plugins/lucid) | Connect to Lucid (Lucidchart & Lucidspark) for creating and managing diagrams |

## Installation

These plugins work with both [Claude CoWork](https://claude.com/cowork) and [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (CLI).

### Claude CoWork

**Step 1.** Open the **Customize** panel, select **Connectors**, then click **Browse plugins**.

<img src="resources/01-connectors-browse-plugins.png" alt="Open Connectors and Browse plugins" width="600">

**Step 2.** Switch to the **Personal** tab, click the **+** button, and select **Add marketplace from GitHub**.

<img src="resources/02-personal-add-marketplace.png" alt="Personal tab, add marketplace" width="600">

**Step 3.** Enter `Rosetree-Solutions/claude-plugins` and click **Sync**.

<img src="resources/03-add-marketplace-dialog.png" alt="Add marketplace dialog" width="500">

**Step 4.** The **Lucid** plugin appears in the marketplace. Click **Install**.

<img src="resources/04-install-plugin.png" alt="Install the Lucid plugin" width="600">

### Checking for updates

To refresh available plugins after new versions are published, click the **...** menu next to the marketplace name and select **Check for updates**.

<img src="resources/05-marketplace-options.png" alt="Check for updates" width="600">

### Claude Code (CLI)

1. **Add the marketplace:**
   ```
   /plugin marketplace add Rosetree-Solutions/claude-cowork-plugins
   ```

2. **Install a plugin:**
   ```
   /plugin install lucid@rosetree-lucid-plugin
   ```

### After installation

The first time you use a plugin's MCP tools, you'll be prompted to connect your account (e.g., sign in to Lucid). Once authenticated, the plugin's skills are available automatically — ask Claude to "create a flowchart" or "find my Lucid diagrams" and the appropriate skill will activate.

## Repository Structure

```
.claude-plugin/
└── marketplace.json            # Marketplace catalog (registers plugins for distribution)
plugins/
└── <plugin-name>/
    ├── .claude-plugin/
    │   └── plugin.json         # Plugin manifest (name, description, version, author)
    ├── .mcp.json               # MCP server configuration
    ├── .claude/
    │   └── settings.local.json # Permission allowlists for MCP tools
    └── skills/
        └── <skill-name>/
            └── SKILL.md        # Skill definition with instructions for Claude
```

- **marketplace.json** — Lives at the repo root in `.claude-plugin/`. Registers all plugins in this repository for distribution via `/plugin marketplace add`.
- **plugin.json** — Identifies an individual plugin and provides metadata (name, description, version, keywords).
- **.mcp.json** — Registers MCP servers (HTTP or stdio) that the plugin connects to.
- **settings.local.json** — Whitelists specific MCP tools and restricts external access (e.g., domain-scoped `WebFetch`).
- **SKILL.md** — Defines a skill with YAML front matter (`name`, `description`) and markdown instructions that guide Claude through a workflow using the plugin's MCP tools.

## Contributing a New Plugin

1. Create a directory: `plugins/<your-plugin-name>/`
2. Add a `.claude-plugin/plugin.json` with at minimum:
   ```json
   {
     "name": "your-plugin-name",
     "description": "What this plugin does",
     "version": "1.0.0",
     "author": { "name": "Your Name" }
   }
   ```
3. Add `.mcp.json` if your plugin uses an MCP server.
4. Add skills under `skills/<skill-name>/SKILL.md` for guided workflows.
5. Add `.claude/settings.local.json` to configure tool permissions.
6. Add your plugin to the root `.claude-plugin/marketplace.json` so it's discoverable.
7. Update the **Available Plugins** table in this README.
