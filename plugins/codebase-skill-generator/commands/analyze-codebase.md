---
name: analyze-codebase
description: Analyze codebase with parallel agents and generate tailored skills for each aspect (security, performance, architecture, etc.)
argument-hint: [skill-prefix]
---

<objective>
Orchestrate parallel sub-agents to comprehensively analyze this codebase and generate tailored skills for optimal code generation.

Each analyzer agent:
1. Analyzes a specific aspect (security, performance, architecture, etc.)
2. Spawns a skill-creation agent that follows `@codebase-skill-generator:skill-creation/SKILL.md`
3. The skill-creation agent creates a properly structured skill from the findings

This architecture keeps analyzer context windows focused on codebase analysis, while skill-creation agents focus on following the skill creation workflow.

Optional argument: `$ARGUMENTS` sets a custom prefix for generated skills (default: "codebase").
</objective>

<context>
Skill prefix: $ARGUMENTS (use "codebase" if empty)
Target directory: .claude/skills/
Skill creation workflow: @codebase-skill-generator:skill-creation/SKILL.md
</context>

<process>
## Phase 1: Tech Stack Detection (Sequential)

First, spawn the `tech-stack-detector` agent to identify all technologies in use:

```
Task tool:
  subagent_type: tech-stack-detector
  prompt: "Analyze this codebase and return a structured inventory of detected technologies. Output as JSON with keys: frontend, backend, database, testing, build_tools, conditional_analyzers. The conditional_analyzers object should have boolean flags for: react, backend, frontend, database, testing."
```

Wait for completion and capture the tech stack inventory.

## Phase 2: Parallel Analysis (Based on Detection)

Spawn these agents IN PARALLEL using a single message with multiple Task tool calls:

**Always run (5 agents):**
1. `security-analyzer` - Security best practices for detected stack
2. `performance-analyzer` - Performance optimization patterns
3. `architecture-analyzer` - Codebase structure and conventions
4. `dependency-analyzer` - Dependency management practices
5. `code-quality-analyzer` - Linting, formatting, quality standards

**Conditionally run (based on Phase 1 `conditional_analyzers` flags):**
6. `react-analyzer` - Only if `conditional_analyzers.react == true`
7. `backend-analyzer` - Only if `conditional_analyzers.backend == true`
8. `frontend-analyzer` - Only if `conditional_analyzers.frontend == true` (non-React)
9. `database-analyzer` - Only if `conditional_analyzers.database == true`
10. `testing-analyzer` - Only if `conditional_analyzers.testing == true`

Each agent prompt should include:
- The detected tech stack from Phase 1
- The skill prefix to use
- Note: Agents will spawn their own skill-creation agents

Example agent prompt:
```
Tech stack detected: {tech_stack_json}
Skill prefix: {prefix}

Analyze the codebase for [aspect] patterns and best practices specific to the detected technologies.

After analysis, spawn a skill-creation agent via Task tool with your findings. The skill-creation agent should read and follow @codebase-skill-generator:skill-creation/SKILL.md to create a properly structured skill.
```

## Phase 3: Summary

After all agents complete, provide a summary:
- List all generated skills with their locations
- Note any agents that were skipped (and why)
- Explain how to use the generated skills
</process>

<agent_coordination>
**Critical execution rules:**

1. Phase 1 MUST complete before Phase 2 starts (tech stack informs conditional agents)
2. Phase 2 agents run in PARALLEL (use single message with multiple Task calls)
3. Each analyzer agent spawns its own skill-creation agent via Task tool
4. Skill-creation agents follow the workflow at `@codebase-skill-generator:skill-creation/SKILL.md`
5. Use the skill prefix from $ARGUMENTS (default "codebase") for all generated skills

**Agent prompts should include:**
- Full tech stack context from Phase 1
- Clear analysis scope
- Skill prefix for naming
</agent_coordination>

<output>
Generated skills structure:
```
.claude/skills/
├── {prefix}-security/SKILL.md
├── {prefix}-performance/SKILL.md
├── {prefix}-architecture/SKILL.md
├── {prefix}-dependencies/SKILL.md
├── {prefix}-code-quality/SKILL.md
└── [conditional skills based on detected stack]
```

Each skill contains:
- YAML frontmatter with name and description
- Codebase-specific guidelines and patterns
- Actionable instructions for code generation
</output>

<success_criteria>
- Tech stack detection completed and informed agent selection
- All applicable agents ran in parallel
- Each analyzer spawned a skill-creation agent with findings
- Skills created following the workflow at @codebase-skill-generator:skill-creation/SKILL.md
- Skills saved to `.claude/skills/{prefix}-*/SKILL.md`
- Summary provided listing all generated skills
</success_criteria>
