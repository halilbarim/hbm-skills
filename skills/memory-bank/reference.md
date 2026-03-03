# Memory Bank — Reference Guide

## Agent Rules File Mapping

The Memory Bank protocol must be injected into the agent's rules file so it reads memory bank files at every session start. Below is the complete mapping of agents to their rules files.

**IMPORTANT:** These files are NOT always in the project root. Always search the ENTIRE project tree (excluding `node_modules/`, `.git/`, `dist/`, `build/`, `.next/`, `vendor/`) to find them.

| Agent | File Name(s) | Standard Location | Notes |
|-------|-------------|-------------------|-------|
| Claude Code | `CLAUDE.md`, `.claude/rules/*.md` | Project root or `.claude/` | `CLAUDE.md` at root or `.claude/CLAUDE.md`. Rules directory `.claude/rules/` for modular rules |
| Cursor | `.cursor/rules/*.mdc` | `.cursor/rules/` directory | `.mdc` files in `.cursor/rules/`. Legacy `.cursorrules` at root is deprecated |
| Windsurf | `.windsurf/rules/*.md` | `.windsurf/rules/` directory | `.md` files in `.windsurf/rules/`. Legacy `.windsurfrules` at root is deprecated |
| Cline | `.clinerules/` (dir) or `.clinerules` (file) | Project root | Directory with `.md` files preferred. Single file also supported |
| GitHub Copilot | `.github/copilot-instructions.md` | `.github/` directory | Additional: `.github/instructions/*.instructions.md` for granular rules |
| Roo Code | `.roo/rules/*.md` | `.roo/rules/` directory | `.md` files in `.roo/rules/`. Legacy `.roorules` at root is deprecated |
| Aider | `CONVENTIONS.md`, `.aider.conf.yml` | Project root | `CONVENTIONS.md` for rules, `.aider.conf.yml` for config with `read` param |
| Antigravity | `.gemini/GEMINI.md` | `.gemini/` directory | Project-level rules. Global: `~/.gemini/GEMINI.md`. Does NOT auto-load `AGENTS.md` |
| OpenAI Codex | `AGENTS.md` | Project root | Walks from git root to cwd. Also supports `AGENTS.override.md` |
| Generic / Other | `AGENTS.md` | Project root | Fallback for unrecognized agents. User must manually copy to their agent's rules file |

### Search Strategy

Use glob patterns to search the entire project tree:
```
# Current standard locations
**/CLAUDE.md
**/.claude/CLAUDE.md
**/.claude/rules/*.md
**/.cursor/rules/*.mdc
**/.windsurf/rules/*.md
**/.clinerules
**/.clinerules/*.md
**/.github/copilot-instructions.md
**/.github/instructions/*.instructions.md
**/.roo/rules/*.md
**/.aider.conf.yml
**/CONVENTIONS.md
**/.gemini/GEMINI.md
**/AGENTS.md

# Legacy files (deprecated but still supported)
**/.cursorrules
**/.windsurfrules
**/.roorules
```

Exclude directories: `node_modules`, `.git`, `dist`, `build`, `.next`, `vendor`, `.cache`

For file names that may be ambiguous (e.g., `CONVENTIONS.md`), verify by checking the parent directory or file contents to confirm it's an agent rules file.

When scanning, check for ALL of these patterns — a project may have multiple agents configured.

### Unrecognized Agent Fallback

When the user's agent is not in the supported list, the skill creates `AGENTS.md` in the project root and injects the Memory Bank protocol into it. The user MUST be clearly warned that they need to manually copy the protocol from `AGENTS.md` into their agent's own rules file for Memory Bank to work correctly. This is critical — without the protocol in the agent's native rules file, the agent will not read memory bank files at session start.

## Memory Bank File Descriptions

### Core Files (Always Created)

| File | Purpose | When to Update |
|------|---------|----------------|
| `RULES.md` | Universal rules for all agents. Immutable after creation. | **NEVER** — created once during setup |
| `projectbrief.md` | Project scope, vision, core requirements, goals | Only when user explicitly requests changes to project scope |
| `productContext.md` | Problem definition, target user, user flows, UX decisions | When product direction or user flows change |
| `systemPatterns.md` | Architecture overview, design patterns, component relationships, decision log | When architecture decisions are made or patterns change |
| `techContext.md` | Tech stack, dependencies, dev environment, project structure, import aliases | When dependencies change, new tools adopted, structure changes |
| `activeContext.md` | Current focus, recent changes, active decisions, next steps | Most frequently updated — after each significant work session |
| `progress.md` | Completed features, work in progress, known issues, blockers | When features complete, new issues found, or WIP status changes |

### Extended Files (Optional — Created with Extended Profile)

| File | Purpose | When to Update |
|------|---------|----------------|
| `businessLogic.md` | Domain rules, business workflows, validation logic, domain glossary | When business rules change, new workflows added, domain logic updated |
| `dataModel.md` | Database schema, entity relationships, data flow, migrations | When schema changes, new entities added, migrations run |
| `dependencies.md` | Detailed dependency map, version constraints, upgrade notes | When packages added/removed/upgraded, incompatibilities found |
| `events.md` | Event-driven architecture, pub/sub topics, event schemas, error handling | When new events added, event schemas change, brokers reconfigured |
| `externalIntegrations.md` | 3rd party APIs, webhooks, service contracts, integration patterns | When new integrations added, API contracts change, services updated |
| `featureToggles.md` | Feature flags, A/B tests, rollout rules, deprecated flags | When flags added/removed, rollout percentages change, tests start/end |
| `observability.md` | Logging, monitoring, tracing, alerting, health checks, error tracking | When monitoring setup changes, new alerts added, dashboards updated |
| `security.md` | Auth flows, authorization model, security policies, vulnerability tracking | When auth changes, new vulnerabilities found, policies updated |
| `technicalDebt.md` | Known debt items, refactoring priorities, cleanup plans | When new debt identified, refactoring completed, priorities shift |
| `contextCoverage.md` | Memory bank completeness tracking, gaps, staleness risk | After each update cycle — tracks how well memory bank covers the project |

## Update Triggers

### Core File Triggers

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

### Extended File Triggers (only if the file exists)

| Event | Files to Update |
|-------|----------------|
| Business rule changed | `businessLogic.md` |
| DB schema migrated | `dataModel.md` |
| Package added/upgraded/removed | `dependencies.md` |
| Event schema changed / new event | `events.md` |
| New 3rd party integration | `externalIntegrations.md` |
| Feature flag added/removed | `featureToggles.md` |
| Monitoring/alerting changed | `observability.md` |
| Auth or security policy changed | `security.md` |
| Technical debt identified/resolved | `technicalDebt.md` |
| Memory bank update cycle completed | `contextCoverage.md` |

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
