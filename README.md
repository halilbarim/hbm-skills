<div align="center">

# hbm-skills

**AI coding agent'lar icin akilli skill koleksiyonu**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Skills: 1](https://img.shields.io/badge/skills-1-brightgreen.svg)](#-available-skills)
[![Platform: skills.sh](https://img.shields.io/badge/platform-skills.sh-black.svg)](https://skills.sh/)

_[skills.sh](https://skills.sh/) ekosistemi icin yeniden kullanilabilir skill paketleri — Claude Code, Cursor, Windsurf, Cline, Copilot ve daha fazlasiyla calisir._

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

Oturumlar arasi baglam surekliligi saglayan Memory Bank sistemi. Proje kokunde `memory-bank/` dizini olusturur ve 7 yapilandirilmis markdown dosyasiyla proje bilgisini, mimari kararlari, ilerlemeyi ve aktif baglami saklar.

### Ozellikler

- **Setup** — Memory bank dizinini ve dosyalarini olusturur, agent rules dosyasina protokol enjekte eder
- **Update** — Proje durumunu tarar, degisiklikleri tespit eder ve memory bank dosyalarini gunceller
- **Status** — Aktif baglam ve ilerleme ozetini gosterir
- **Read** — Tum memory bank dosyalarini organize sekilde sunar

### Desteklenen Agent'lar

| Agent | Rules Dosyasi |
|-------|---------------|
| Claude Code | `CLAUDE.md` |
| Cursor | `.cursorrules`, `.cursor/rules/*.mdc` |
| Windsurf / Codeium | `.windsurfrules` |
| Cline | `.clinerules` |
| GitHub Copilot | `copilot-instructions.md` |
| Roo Code | `.roo/rules.md` |
| Aider | `.aider.conf.yml`, `CONVENTIONS.md` |

### Memory Bank Dosya Yapisi

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
