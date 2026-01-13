# Cursor Skills

A collection of Agent Skills for [Cursor IDE](https://cursor.com).

## Installation

### Via Cursor Settings (Recommended)

1. Open Cursor Settings (`Cmd+Shift+J`)
2. Navigate to **Rules**
3. Click **Add Rule** → **Remote Rule (Github)**
4. Enter: `https://github.com/YOUR_USERNAME/cursor-skills`

### Manual Installation

Clone into your Cursor skills directory:

```bash
# User-wide (all projects)
git clone https://github.com/YOUR_USERNAME/cursor-skills ~/.cursor/skills

# Or project-specific
git clone https://github.com/YOUR_USERNAME/cursor-skills .cursor/skills
```

## Available Skills

| Skill | Description |
|-------|-------------|
| [subagent-creator](./subagent-creator/SKILL.md) | Create optimal Cursor/Claude Code subagents for automating specialized tasks |

## Usage

Skills are automatically discovered by Cursor. You can also invoke them manually by typing `/` in Agent chat and searching for the skill name.

## Adding New Skills

Each skill is a folder containing a `SKILL.md` file:

```
skill-name/
└── SKILL.md
```

The `SKILL.md` file requires YAML frontmatter:

```yaml
---
name: skill-name
description: Short description of what this skill does and when to use it.
---

# Skill Name

Detailed instructions for the agent...
```

## Requirements

- Cursor IDE (Nightly channel)
- Agent Skills enabled in settings
