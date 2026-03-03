# Memory Bank — Reference Guide

## Agent Rules File Mapping

The Memory Bank protocol must be injected into the agent's rules file so it reads memory bank files at every session start. Below is the complete mapping of agents to their rules files.

**IMPORTANT:** These files are NOT always in the project root. Always search the ENTIRE project tree (excluding `node_modules/`, `.git/`, `dist/`, `build/`, `.next/`, `vendor/`) to find them.

| Agent | File Name(s) | Common Locations | Notes |
|-------|-------------|-----------------|-------|
| Claude Code | `CLAUDE.md` | Project root, subdirectories | May exist at multiple levels |
| Cursor | `.cursorrules`, `.cursor/rules/*.mdc` | Project root, `.cursor/` directory | Legacy: `.cursorrules` at root. New: `.cursor/rules/` with `.mdc` files |
| Windsurf / Codeium | `.windsurfrules` | Project root, subdirectories | |
| Cline | `.clinerules` | Project root, subdirectories | |
| GitHub Copilot | `copilot-instructions.md` | `.github/` directory | Usually at `.github/copilot-instructions.md` |
| Roo Code | `rules.md` | `.roo/` directory | Verify parent directory is `.roo/` |
| Aider | `.aider.conf.yml`, `CONVENTIONS.md` | Project root | Either file |
| Generic / Other | `AGENTS.md` | Project root | Fallback for unknown agents |

### Search Strategy

Use glob patterns to search the entire project tree:
```
**/.cursorrules
**/.cursor/rules/*.mdc
**/.windsurfrules
**/.clinerules
**/CLAUDE.md
**/copilot-instructions.md
**/.roo/rules.md
**/.aider.conf.yml
**/CONVENTIONS.md
**/AGENTS.md
```

Exclude directories: `node_modules`, `.git`, `dist`, `build`, `.next`, `vendor`, `.cache`

For file names that may be ambiguous (e.g., `rules.md`, `CONVENTIONS.md`), verify by checking the parent directory or file contents to confirm it's an agent rules file.

When scanning, check for ALL of these patterns — a project may have multiple agents configured.

## Memory Bank File Descriptions

| File | Purpose | When to Update |
|------|---------|----------------|
| `RULES.md` | Universal rules for all agents. Immutable after creation. | **NEVER** — created once during setup |
| `projectbrief.md` | Project scope, vision, core requirements, goals | Only when user explicitly requests changes to project scope |
| `productContext.md` | Problem definition, target user, user flows, UX decisions | When product direction or user flows change |
| `systemPatterns.md` | Architecture overview, design patterns, component relationships, decision log | When architecture decisions are made or patterns change |
| `techContext.md` | Tech stack, dependencies, dev environment, project structure, import aliases | When dependencies change, new tools adopted, structure changes |
| `activeContext.md` | Current focus, recent changes, active decisions, next steps | Most frequently updated — after each significant work session |
| `progress.md` | Completed features, work in progress, known issues, blockers | When features complete, new issues found, or WIP status changes |

## Update Triggers

| Event | Files to Update |
|-------|----------------|
| New feature completed | `activeContext.md` + `progress.md` |
| Architecture decision made | `systemPatterns.md` |
| New dependency added | `techContext.md` |
| Bug fix / error resolution | `activeContext.md` |
| User preference learned | `activeContext.md` |
| Branch / task changed | `activeContext.md` |
| Project scope changed | `projectbrief.md` (with user approval) |
| Product direction changed | `productContext.md` |
| Dev environment changed | `techContext.md` |
| Session ended | `activeContext.md` + `progress.md` |

## Protocol Block

The protocol block that gets injected into agent rules files is stored in `templates/rules-protocol.md`. It instructs the agent to:

1. Read memory bank files at session start
2. Update relevant files during work
3. Respond to memory bank commands (update, status, read)
4. Never modify `RULES.md`
5. Never write secrets to memory bank files

## Rules for Updates

1. **NEVER** modify `RULES.md` — it is immutable after initial creation
2. **NEVER** modify `projectbrief.md` unless the user explicitly requests it
3. **NEVER** write sensitive information (API keys, tokens, passwords, secrets) to any memory bank file
4. **ALWAYS** use date stamps in format `[YYYY-MM-DD]` for temporal entries
5. **ALWAYS** keep each file under 500 lines — summarize older entries if needed
6. **NEVER** duplicate the same information across multiple files — reference instead
7. **ALWAYS** preserve existing content when updating — append or modify sections, don't overwrite entire files
8. **ALWAYS** get user confirmation before writing or modifying files during setup
9. When language is detected from existing files, respond in the same language
10. During updates, show what changed before writing

## Project Scanning Strategy

When filling templates during setup, scan these project files in order:

1. **`package.json`** — name, description, version, dependencies, devDependencies, scripts
2. **`README.md`** — project description, vision, features, usage
3. **`tsconfig.json` / `jsconfig.json`** — paths (import aliases), compiler options, target
4. **Folder structure** (top 2-3 levels) — architecture patterns, module organization
5. **Git log** (last 10-20 commits) — recent work, active development areas
6. **Git branch** — current focus
7. **Existing rules files** — conventions, preferences, coding standards
8. **`.env.example`** — environment variables (names only, never values)
9. **`docker-compose.yml` / `Dockerfile`** — infrastructure patterns
10. **CI/CD configs** (`.github/workflows/`, `.gitlab-ci.yml`) — deployment patterns
