# Skill Bank Marketplace

A collection of plugins that extend [Claude Code](https://claude.ai/code) with new capabilities, commands, and autonomous agents.

## Available Plugins

| Plugin | Category | Description |
|--------|----------|-------------|
| [zettelkasten-assistant](./plugins/zettelkasten-assistant/) | Productivity | Comprehensive assistant for Zettelkasten knowledge management in Obsidian. Provides intelligent capture, processing workflows, and autonomous agents for maintaining your knowledge base. |

## Installation

Install plugins from this marketplace using Claude Code's plugin system:

```bash
# Install a specific plugin
claude plugins add rogarcia/skill-issue:zettelkasten-assistant
```

Or install from the plugin's directory URL:

```bash
claude plugins add https://github.com/rogarcia/skill-issue/tree/main/plugins/zettelkasten-assistant
```

## Plugin Architecture

Each plugin in this marketplace follows the Claude Code plugin structure:

```
plugin-name/
├── .claude-plugin/
│   └── plugin.json      # Plugin manifest
├── commands/            # Slash commands (/command-name)
├── agents/              # Autonomous task agents
├── skills/              # Domain knowledge definitions
└── README.md            # Plugin documentation
```

### Components

- **Commands** - User-invocable slash commands for guided workflows
- **Agents** - Autonomous agents that trigger on natural language requests
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
- Document when agents should trigger
- Test all commands and agents before submitting

## License

MIT
