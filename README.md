<div align="center">

# hbm-skills

**Smart skill collection for AI coding agents**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Skills: 1](https://img.shields.io/badge/skills-1-brightgreen.svg)](#-available-skills)
[![Platform: skills.sh](https://img.shields.io/badge/platform-skills.sh-black.svg)](https://skills.sh/)

_Reusable instruction packs for the [skills.sh](https://skills.sh/) ecosystem — works with Claude Code, Cursor, Windsurf, Cline, Copilot, and more._

</div>

---

## Quick Install

```bash
# Install a single skill
npx skills add halilbarim/hbm-skills@memory-bank
```

---

## Available Skills

| Skill | Install | Description |
|-------|---------|-------------|
| **memory-bank** | `npx skills add halilbarim/hbm-skills@memory-bank` | Cross-session context continuity system. Maintains project knowledge, architecture decisions, and progress across sessions. |

---

## memory-bank

A cross-session context continuity system for AI coding agents. Creates a `memory-bank/` directory in the project root with 7 structured markdown files that capture project knowledge, architecture decisions, progress, and active context.

### Features

- **Setup** — Creates the memory bank directory and files, injects protocol into agent rules files
- **Update** — Scans current project state, detects changes, and updates memory bank files
- **Status** — Shows a concise summary of active context and progress
- **Read** — Presents all memory bank files in an organized format

### Supported Agents

| Agent | Rules File |
|-------|------------|
| Claude Code | `CLAUDE.md` |
| Cursor | `.cursorrules`, `.cursor/rules/*.mdc` |
| Windsurf / Codeium | `.windsurfrules` |
| Cline | `.clinerules` |
| GitHub Copilot | `copilot-instructions.md` |
| Roo Code | `.roo/rules.md` |
| Aider | `.aider.conf.yml`, `CONVENTIONS.md` |

### Memory Bank File Structure

```
memory-bank/
├── RULES.md              # Universal rules (immutable)
├── projectbrief.md       # Project scope & vision
├── productContext.md      # Problem definition & user flows
├── systemPatterns.md      # Architecture & design patterns
├── techContext.md         # Tech stack & dependencies
├── activeContext.md       # Current focus & recent changes
└── progress.md            # Completed features & WIP
```

---

## Repository Structure

```
hbm-skills/
├── skills/
│   └── memory-bank/
│       ├── SKILL.md           # Main instruction file
│       ├── reference.md       # Agent mapping & update triggers
│       └── templates/         # 8 template files
├── README.md
├── LICENSE
└── .gitignore
```

---

## Contributing

1. Create a folder under `skills/<skill-name>/`
2. Add `SKILL.md` with frontmatter:
   ```yaml
   ---
   name: skill-name
   description: What it does and when it should trigger.
   ---
   ```
3. Add supporting folders as needed: `references/`, `templates/`
4. Update this README and submit a PR

---

## License

MIT
