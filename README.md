# Justin's Claude Code Plugins

Personal collection of plugins for [Claude Code](https://docs.anthropic.com/en/docs/claude-code).

## Installation

Add the marketplace to Claude Code:

```bash
/plugin marketplace add justinwlin/justin-claude-plugins
```

Then install specific skills:

```bash
/plugin install <skill-name>@justin-tools
```

## Adding Skills

1. Create skill folder in appropriate category (`skills/dev/`, `skills/infra/`, or `skills/productivity/`)
2. Add `SKILL.md` with instructions and patterns
3. Optionally add `config.json` for environment variables
4. Register in `.claude-plugin/marketplace.json`

### Skill Structure

```
skills/
├── dev/           # Developer tools
├── infra/         # Infrastructure tools
└── productivity/  # Productivity tools
```

Each skill directory contains:
- `SKILL.md` - Documentation, usage instructions, API patterns
- `config.json` (optional) - Environment variables and configuration

## Usage

After installing, skills are automatically available. Claude will use them when relevant.

## License

MIT
