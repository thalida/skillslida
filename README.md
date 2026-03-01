# skillslida

thalida's personal [Claude Code](https://claude.ai/code) skills plugin.

Custom skills that extend Claude Code's behavior. Skills are model-invoked — Claude automatically uses them based on task context (no manual slash command needed).

## Getting started

```bash
claude plugin marketplace add thalida/skillslida
claude plugin install skillslida@skillslida
```

Start a new Claude Code session — skills will be available immediately.

## Local development

### Setup

1. Clone this repo:

   ```bash
   git clone https://github.com/thalida/skillslida ~/Documents/Repos/skillslida
   ```

2. Register it as a marketplace and install:

   ```bash
   claude plugin marketplace add ~/Documents/Repos/skillslida
   claude plugin install skillslida@skillslida
   ```

3. Start a new Claude Code session — skills will be available immediately.

Since the plugin installs from this local directory, edits to skill files take effect on the next Claude Code session — no reinstall needed.

### Structure

```text
skillslida/
├── .claude-plugin/
│   ├── plugin.json         # Plugin metadata
│   └── marketplace.json    # Marketplace manifest (for local installation)
├── skills/
│   └── <skill-name>/
│       └── SKILL.md        # Skill definition
└── README.md
```

### Adding a new skill

1. Create a new directory under `skills/`:

   ```bash
   mkdir -p skills/<skill-name>
   ```

2. Create `skills/<skill-name>/SKILL.md` with frontmatter:

   ```markdown
   ---
   name: skillslida:<skill-name>
   description: Describe when Claude should use this skill (trigger conditions, keywords, phrases).
   ---

   # Skill Title

   Instructions for Claude...
   ```

3. Bump the version in `.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json`, then commit and push:

   ```bash
   git add skills/<skill-name>/ .claude-plugin/
   git commit -m "feat: add <skill-name> skill"
   git push
   ```

## Security

This is a **public** repo. Do not commit:

- API keys or secrets
- `settings.local.json` or any `.local` files
- Personal file paths (skills should be path-agnostic)
- Any project-specific private data
