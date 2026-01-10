---
name: analyze-codebase
description: Analyze codebase with parallel agents and generate tailored skills for each aspect (security, performance, architecture, etc.)
argument-hint: [skill-prefix]
---

<objective>
Orchestrate parallel sub-agents to comprehensively analyze this codebase and generate tailored skills for optimal code generation.

The workflow:
1. Detect tech stack to determine which analyzers to run
2. Run analyzers in parallel - each writes findings to `.claude/findings/`
3. Read each findings file and invoke skill-creation to generate standardized skills
4. Summary of all generated skills

Optional argument: `$ARGUMENTS` sets a custom prefix for generated skills (default: "codebase").
</objective>

<context>
Skill prefix: $ARGUMENTS (use "codebase" if empty)
Findings directory: .claude/findings/
Skills directory: .claude/skills/
Skill creation workflow: @codebase-skill-generator:skill-creation/SKILL.md
</context>

<process>
## Phase 1: Tech Stack Detection (Sequential)

First, spawn the `tech-stack-detector` agent to identify all technologies in use:

```
Task tool:
  subagent_type: codebase-skill-generator:tech-stack-detector
  prompt: "Analyze this codebase and return a structured inventory of detected technologies. Output as JSON with keys: primary_language, frontend, backend, database, testing, devtools, conditional_analyzers. The conditional_analyzers object should have boolean flags for: react, backend, frontend, database, testing."
```

Wait for completion and capture the tech stack inventory as `{tech_stack_json}`.

Set `{prefix}` to $ARGUMENTS if provided, otherwise "codebase".

## Phase 2: Parallel Analysis

Create the findings directory first:
```bash
mkdir -p .claude/findings
```

Spawn these agents IN PARALLEL using a single message with multiple Task tool calls.

**Always run (5 agents):**

1. `codebase-skill-generator:security-analyzer`
2. `codebase-skill-generator:performance-analyzer`
3. `codebase-skill-generator:architecture-analyzer`
4. `codebase-skill-generator:dependency-analyzer`
5. `codebase-skill-generator:code-quality-analyzer`

**Conditionally run (based on Phase 1 `conditional_analyzers` flags):**

6. `codebase-skill-generator:react-analyzer` - Only if `conditional_analyzers.react == true`
7. `codebase-skill-generator:backend-analyzer` - Only if `conditional_analyzers.backend == true`
8. `codebase-skill-generator:frontend-analyzer` - Only if `conditional_analyzers.frontend == true` (non-React)
9. `codebase-skill-generator:database-analyzer` - Only if `conditional_analyzers.database == true`
10. `codebase-skill-generator:testing-analyzer` - Only if `conditional_analyzers.testing == true`

Each agent prompt should include:
```
Tech stack detected: {tech_stack_json}
Skill prefix: {prefix}

Analyze the codebase for [aspect] patterns and best practices specific to the detected technologies.

Write your findings to: .claude/findings/{prefix}-[aspect].md

Return confirmation when findings file is written.
```

Wait for ALL analyzers to complete.

## Phase 3: Skill Creation (Sequential)

For each findings file in `.claude/findings/{prefix}-*.md`:

1. Read the findings file
2. Invoke the skill-creation skill with a prompt like:

```
Use the skill-creation workflow at @codebase-skill-generator:skill-creation/SKILL.md

I want to create a new skill based on these analysis findings.

Skill details:
- Name: {prefix}-{aspect} (e.g., myapp-security)
- Location: .claude/skills/{prefix}-{aspect}/SKILL.md
- Description: [Aspect] patterns and best practices for this codebase. Use when writing [aspect]-related code.

Analysis findings:
{contents of findings file}

Create a simple skill (not router pattern) that captures the key patterns, conventions, and checklists from these findings.
```

Repeat for each findings file.

## Phase 4: Summary

After all skills are created, provide a summary:
- List all generated skills with their locations
- Note any analyzers that were skipped (and why)
- Explain how to use the generated skills
- Optionally suggest cleanup of `.claude/findings/` directory
</process>

<agent_coordination>
**Critical execution rules:**

1. Phase 1 MUST complete before Phase 2 starts (tech stack informs conditional agents)
2. Phase 2 agents run in PARALLEL (use single message with multiple Task calls)
3. Each analyzer writes findings to `.claude/findings/{prefix}-{aspect}.md`
4. Phase 3 runs SEQUENTIALLY - main conversation invokes skill-creation for each findings file
5. Use the skill prefix from $ARGUMENTS (default "codebase") for all generated skills

**Agent prompts should include:**
- Full tech stack context from Phase 1
- Clear analysis scope
- Skill prefix for naming
- Output path for findings file
</agent_coordination>

<output>
Generated files structure:
```
.claude/
├── findings/                          # Analysis results (can be deleted after)
│   ├── {prefix}-security.md
│   ├── {prefix}-performance.md
│   ├── {prefix}-architecture.md
│   ├── {prefix}-dependencies.md
│   ├── {prefix}-code-quality.md
│   └── [conditional findings]
│
└── skills/                            # Generated skills (permanent)
    ├── {prefix}-security/SKILL.md
    ├── {prefix}-performance/SKILL.md
    ├── {prefix}-architecture/SKILL.md
    ├── {prefix}-dependencies/SKILL.md
    ├── {prefix}-code-quality/SKILL.md
    └── [conditional skills]
```

Each skill contains:
- YAML frontmatter with name and description
- Codebase-specific guidelines and patterns
- Actionable instructions for code generation
</output>

<success_criteria>
- Tech stack detection completed and informed agent selection
- All applicable analyzers ran in parallel and wrote findings files
- All findings files processed through skill-creation workflow
- Skills created following standardized structure
- Skills saved to `.claude/skills/{prefix}-*/SKILL.md`
- Summary provided listing all generated skills
</success_criteria>
