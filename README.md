# organize-claude-md

A Claude Code slash command that reorganizes your `CLAUDE.md` into an optimized **lazy-loading reference structure** with framework-specific templates and Mermaid architecture diagrams.

[한국어 README](README.ko.md)

## Why

`CLAUDE.md` is loaded into context at every session start. When it exceeds ~120 lines, the agent loses focus on key instructions and compliance drops. This command:

- Keeps the main `CLAUDE.md` under 120 lines (always-loaded essentials)
- Extracts detailed docs into `.claude/docs/reference/` (on-demand loading)
- Auto-detects your framework & language and generates relevant architecture diagrams
- Uses prohibition-style rules ("NEVER do X") which have higher compliance than recommendations

## Supported Frameworks

| Category | Frameworks |
|----------|-----------|
| JVM | Spring Boot (Java), Spring Boot (Kotlin), Quarkus, Micronaut |
| JS/TS Frontend | React, Next.js, Vue, Nuxt, Angular, Svelte/SvelteKit |
| JS/TS Backend | NestJS, Express, Fastify |
| Mobile | Flutter (Dart), React Native |
| Python | Django, FastAPI, Flask |
| Other | Go (Gin/Echo/Fiber), Rust (Actix/Axum/Rocket) |

## Installation

Copy the command file into your Claude Code commands directory:

```bash
# Global (available in all projects)
cp organize-claude-md.md ~/.claude/commands/

# Project-local (available only in this project)
cp organize-claude-md.md .claude/commands/
```

## Usage

In Claude Code, run:

```
/organize-claude-md
```

### Arguments

| Argument | Description |
|----------|-------------|
| _(empty)_ | Full reorganization (CLAUDE.md + reference docs) |
| `main` | Reorganize CLAUDE.md only |
| `references` | Generate/update reference docs only |
| `module:<name>` | Single module reference doc (e.g., `module:repair`) |
| `scan` | Analysis report only (no changes) |
| `diff` | Show proposed changes vs current |

## What It Does

### Phase 1 — Project Analysis
- Reads existing CLAUDE.md and reference docs
- Auto-detects framework + language (e.g., `Spring Boot 3.3.8 (Java 21)`)
- Deep-scans actual code patterns (no guessing)

### Phase 2 — Content Classification
- Splits content into "always loaded" (main) vs "on demand" (reference docs)
- Skills section always stays in main CLAUDE.md

### Phase 3 — Architecture Diagram
- Generates Mermaid diagram from actual code structure
- Framework-specific templates (Spring Boot, React, Vue, Flutter, NestJS)

### Phase 4 — Main CLAUDE.md
- Rewrites to under 120 lines
- Prohibition-style key rules
- Reference docs index table

### Phase 5 — Reference Docs
- Creates `.claude/docs/reference/` files
- Merges with existing docs (never overwrites)
- Each doc under 200 lines

### Phase 6-7 — Confirmation & Verification
- Shows change plan before executing
- Validates line counts, file paths, and completeness

## Key Principles

- **Never deletes** existing CLAUDE.md information — moves it to main or reference docs
- **Never overwrites** existing reference docs — merges instead
- **Language-accurate** — Java projects get Java patterns, Kotlin gets Kotlin (no mixing)
- **Code-based** — all patterns extracted from actual project code, not generic templates

## License

MIT
