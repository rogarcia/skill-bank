# Skill Bank Marketplace

A collection of plugins that extend [Claude Code](https://claude.ai/code) with new capabilities, commands, and autonomous agents.

## Available Plugins

| Plugin | Category | Description |
|--------|----------|-------------|
| [zettelkasten-assistant](./plugins/zettelkasten-assistant/) | Productivity | Comprehensive assistant for Zettelkasten knowledge management in Obsidian. Provides intelligent capture, processing workflows, and autonomous agents for maintaining your knowledge base. |

## Installation

Install plugins from this marketplace using Claude Code's plugin system:

```bash
claude plugin marketplace add rogarcia/skill-bank
claude plugin install zettelkasten-assistant@skill-bank
```

Inside a session, use `/plugin marketplace add rogarcia/skill-bank` and `/plugin install zettelkasten-assistant@skill-bank`.

## Plugin Architecture

Each plugin in this marketplace follows the Claude Code plugin structure:

```
plugin-name/
├── .claude-plugin/
│   └── plugin.json      # Plugin manifest
├── commands/            # Slash commands (auto-discovered)
├── agents/              # Subagents (auto-discovered)
├── skills/              # Skills (auto-discovered, one dir per SKILL.md)
└── README.md            # Plugin documentation
```

### Components

- **Commands** - User-invocable slash commands for guided workflows
- **Agents** - Subagents Claude delegates to based on their `description`
- **Skills** - Domain knowledge that powers commands and agents

## Contributing

Want to add a plugin to this marketplace?

1. Fork this repository
2. Create your plugin under `plugins/your-plugin-name/`
3. Follow the plugin structure above
4. Add your plugin to `.claude-plugin/marketplace.json`
5. Submit a pull request

### Plugin Guidelines

- Include a comprehensive `README.md` with usage examples
- Use lowercase with hyphens for filenames: `example-name.md`
- Keep command descriptions concise but informative
- Put trigger conditions in each agent and skill `description` (there is no `when` field)
- Declare `version`, `license` and `keywords` in `plugin.json`; omit component arrays when using the default directories
- Test all commands and agents before submitting

## License

MIT
