# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repository Is

This is the official Claude Code plugin marketplace directory. It tracks both **internal** (Anthropic-developed) plugins in `plugins/` and **external** (third-party/community) plugins in `external_plugins/`. Users install plugins via:

```
/plugin install {plugin-name}@claude-plugins-official
```

or by browsing `/plugin > Discover` in Claude Code.

---

## Repository Structure

```
plugins/             # Internal plugins — maintained by Anthropic
external_plugins/    # External plugins — partner/community submissions
```

Internal plugins each follow this layout:

```
plugin-name/
├── .claude-plugin/
│   └── plugin.json      # Required: name, description, version, author
├── skills/              # SKILL.md files (preferred for commands + context)
├── commands/            # Legacy slash command .md files
├── agents/              # Agent definition .md files
├── hooks/               # Hook scripts
├── .mcp.json            # MCP server wiring (optional)
└── README.md
```

See `plugins/example-plugin/` for a canonical reference implementation covering all extension types.

---

## Plugin Components

**Skills** (preferred) — `SKILL.md` files with YAML frontmatter. Used for both model-invoked contextual guidance and user-invoked slash commands. Set `description` as trigger conditions for model-invoked; add a `/` prefix convention in name for user-invoked.

**Commands** — Legacy `.md` files in `commands/`. Still supported but new work should use skills.

**Agents** — `.md` files in `agents/` defining specialized sub-agents with their own system prompts and tool restrictions.

**Hooks** — Scripts in `hooks/` that fire on Claude Code lifecycle events (`SessionStart`, `PreToolUse`, `PostToolUse`, `Stop`).

---

## Internal Plugin Conventions

- `plugin.json` must include `name`, `description`, `version`, and `author`
- Plugin name in `plugin.json` must match the folder name
- README.md should document all commands, agents, and skills with usage examples
- Hooks must handle errors gracefully — use exit code 1 for non-blocking errors, exit code 2 to feed errors back to Claude
- When adding a new internal plugin, update the root `directory.json` in the `sjbrenchley89/claude-code` repo with a new entry pointing to `"source": "./plugins/<name>"`

---

## External Plugins

`external_plugins/` contains third-party plugins linked by subdirectory name. Each external plugin has its own source repo. External plugins are submitted via the plugin directory submission form and must meet quality/security standards.

Do not modify external plugin content directly — changes must go through the upstream repo and a SHA bump PR.

---

## Notable Internal Plugins

| Plugin | What it does |
|--------|-------------|
| `code-review` | Multi-agent PR review with confidence scoring |
| `feature-dev` | 7-phase guided feature development workflow |
| `hookify` | Creates custom hooks from conversation patterns |
| `plugin-dev` | 8-phase guided plugin creation workflow |
| `pr-review-toolkit` | Specialized PR review agents (tests, types, simplification) |
| `security-guidance` | PreToolUse hook warning on 9 security patterns |
| `agent-sdk-dev` | Scaffolds and validates Agent SDK apps |
| `commit-commands` | Git commit/push/PR slash commands |
