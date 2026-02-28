# skillslida

thalida's personal [Claude Code](https://claude.ai/code) skills plugin.

## What's in here

Custom skills that extend Claude Code's behavior. Skills are model-invoked — Claude automatically uses them based on task context (no manual slash command needed).

## Structure

```
skillslida/
├── .claude-plugin/
│   └── plugin.json       # Plugin metadata
├── skills/
│   └── <skill-name>/
│       └── SKILL.md      # Skill definition
└── README.md
```

## Adding a new skill

1. Create a new directory under `skills/`:
   ```bash
   mkdir -p skills/<skill-name>
   ```

2. Create `skills/<skill-name>/SKILL.md` with frontmatter:
   ```markdown
   ---
   name: <skill-name>
   description: Describe when Claude should use this skill (trigger conditions, keywords, phrases).
   ---

   # Skill Title

   Instructions for Claude...
   ```

3. Commit and push:
   ```bash
   git add skills/<skill-name>/
   git commit -m "feat: add <skill-name> skill"
   git push
   ```

## Local installation

This plugin is registered in `~/.claude/plugins/installed_plugins.json` with `installPath` pointing at this cloned directory. Edits are live immediately — no update command needed.

To install fresh on a new machine:
1. Clone this repo to `~/Documents/Repos/skillslida/`
2. Add to `~/.claude/plugins/installed_plugins.json`:
   ```json
   "skillslida@local": [
     {
       "scope": "user",
       "installPath": "/Users/<you>/Documents/Repos/skillslida",
       "version": "local",
       "installedAt": "<ISO timestamp>",
       "lastUpdated": "<ISO timestamp>"
     }
   ]
   ```

3. Enable the plugin in `~/.claude/settings.json`:

   ```json
   {
     "enabledPlugins": {
       "skillslida@local": true
     }
   }
   ```

4. Start a new Claude Code session — skills will be available immediately.

## Security

This is a **public** repo. Do not commit:
- API keys or secrets
- `settings.local.json` or any `.local` files
- Personal file paths (skills should be path-agnostic)
- Any project-specific private data
