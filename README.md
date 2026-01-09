# Codebase Skill Generator

A Claude Code plugin that analyzes codebases with parallel agents and generates tailored skills for security, performance, architecture, and more.

## Overview

This plugin orchestrates multiple specialized analyzer agents that examine different aspects of your codebase in parallel. Each analyzer:

1. Focuses on a specific domain (security, performance, architecture, etc.)
2. Extracts patterns and conventions from your actual code
3. Generates a skill file tailored to your codebase

The generated skills help Claude understand your codebase's established patterns, making future code generation more consistent and aligned with your project's conventions.

## Installation

Add the plugin to Claude Code by adding the following to your `.mcp.json` or running:

```bash
claude plugins add /path/to/codebase-skill-generator
```

Or manually add to your Claude Code settings:

```json
{
  "plugins": [
    "/Users/zero/Desktop/FayBurn/codebase-skill-generator"
  ]
}
```

## Usage

```
/analyze-codebase [skill-prefix]
```

**Arguments:**
- `skill-prefix` (optional): Prefix for generated skill names. Default: "codebase"

**Example:**
```
/analyze-codebase myproject
```

This generates skills like `myproject-security`, `myproject-performance`, etc.

## Generated Skills

The analyzer generates skills based on what it detects in your codebase:

### Always Generated (5 skills)

| Skill | Description |
|-------|-------------|
| `{prefix}-security` | Security best practices for your detected stack |
| `{prefix}-performance` | Performance optimization patterns |
| `{prefix}-architecture` | Codebase structure and conventions |
| `{prefix}-dependencies` | Dependency management practices |
| `{prefix}-code-quality` | Linting, formatting, quality standards |

### Conditionally Generated

| Skill | Generated When |
|-------|----------------|
| `{prefix}-react` | React is detected |
| `{prefix}-backend` | Backend framework detected (Express, FastAPI, etc.) |
| `{prefix}-frontend` | Non-React frontend detected (Vue, Angular, Svelte) |
| `{prefix}-database` | Database/ORM detected |
| `{prefix}-testing` | Testing framework detected |

## Architecture

### Phase 1: Tech Stack Detection (Sequential)

The `tech-stack-detector` agent scans your codebase to identify:
- Programming languages
- Frontend frameworks and libraries
- Backend frameworks
- Databases and ORMs
- Testing frameworks
- Build tools and dev dependencies

This determines which conditional analyzers to run.

### Phase 2: Parallel Analysis

Based on Phase 1 detection, multiple analyzer agents run in parallel:

```
┌─────────────────────────────────────────────────────┐
│                 Orchestrator                         │
│              (analyze-codebase)                      │
└────────────────────┬────────────────────────────────┘
                     │
    ┌────────────────┼────────────────┐
    │                │                │
    ▼                ▼                ▼
┌─────────┐   ┌─────────────┐   ┌──────────────┐
│Security │   │Performance  │   │Architecture  │   ...
│Analyzer │   │Analyzer     │   │Analyzer      │
└────┬────┘   └──────┬──────┘   └──────┬───────┘
     │               │                 │
     ▼               ▼                 ▼
┌─────────┐   ┌─────────────┐   ┌──────────────┐
│ Skill   │   │   Skill     │   │    Skill     │
│Creation │   │  Creation   │   │   Creation   │
│ Agent   │   │   Agent     │   │    Agent     │
└────┬────┘   └──────┬──────┘   └──────┬───────┘
     │               │                 │
     ▼               ▼                 ▼
   SKILL.md       SKILL.md          SKILL.md
```

Each analyzer:
1. Scans relevant code patterns
2. Documents conventions and best practices
3. Spawns a skill-creation agent with findings
4. The skill-creation agent follows the `skill-creation/SKILL.md` workflow

### Skill Creation Delegation

Analyzer agents delegate skill creation to specialized `skill-creation` agents. This keeps:
- Analyzer context windows focused on code analysis
- Skill creation agents focused on following best practices
- Generated skills consistent and well-structured

## Plugin Structure

```
codebase-skill-generator/
├── .claude-plugin/
│   └── plugin.json           # Plugin manifest
├── commands/
│   └── analyze-codebase.md   # Main slash command
├── agents/
│   ├── tech-stack-detector.md
│   ├── security-analyzer.md
│   ├── performance-analyzer.md
│   ├── architecture-analyzer.md
│   ├── dependency-analyzer.md
│   ├── code-quality-analyzer.md
│   ├── react-analyzer.md
│   ├── backend-analyzer.md
│   ├── frontend-analyzer.md
│   ├── database-analyzer.md
│   ├── testing-analyzer.md
│   └── skill-creation/       # Skill creation workflow
│       ├── SKILL.md
│       ├── workflows/
│       ├── references/
│       └── templates/
└── README.md
```

## Output

Generated skills are saved to:

```
.claude/skills/
├── {prefix}-security/SKILL.md
├── {prefix}-performance/SKILL.md
├── {prefix}-architecture/SKILL.md
├── {prefix}-dependencies/SKILL.md
├── {prefix}-code-quality/SKILL.md
└── [conditional skills based on stack]
```

Each skill contains:
- YAML frontmatter with name and description
- Codebase-specific guidelines extracted from your code
- Actionable instructions for consistent code generation
- File path references to patterns found in your codebase

## Customization

### Adding New Analyzers

1. Create a new agent file in `agents/`
2. Follow the pattern of existing analyzers
3. Add to `plugin.json` agents array
4. Update `analyze-codebase.md` to include the new analyzer

### Modifying Skill Templates

The skill-creation workflow in `agents/skill-creation/` controls how skills are structured. Modify the templates and workflows there to change generated skill format.

## Requirements

- Claude Code with plugin support
- Task tool enabled for parallel agent execution

## License

MIT
