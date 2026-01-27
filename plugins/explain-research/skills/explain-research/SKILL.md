---
name: explain-research
description: Explains research papers for intuitive understanding through analogical reasoning. Builds mental models, tracks evolving knowledge, and surfaces connections. Use when reading papers, updating understanding, or exploring research connections.
---

<objective>
Transform complex research into intuitive understanding through analogical reasoning. Build mental models you can think with, not facts you memorize. Track knowledge evolution and surface connections where insight lives.
</objective>

<quick_start>
1. Select option 1 to explain a paper, or provide paper content directly
2. For concept deep-dives, choose option 2
3. Run `~/.claude/skills/explain-research/bin/kr stats` to see your knowledge state
</quick_start>

<essential_principles>

<principle name="intuition-first">
Lead with intuition, not jargon. Every technical concept maps to something familiar. Find that mapping before introducing formal notation. The goal is understanding you can think with, not facts you memorize.
</principle>

<principle name="mental-model-explicit">
Make model updates explicit. Say "Before: you might think X. After: you'll understand Y because Z." This makes learning visible and correctable.
</principle>

<principle name="analogies-are-bridges">
Analogies are bridges, not destinations. Use them to transport intuition, then refine toward technical accuracy. A good analogy is wrong in specific, learnable ways.
</principle>

<principle name="connections-compound">
Knowledge compounds through connections. Every new paper links to what you already know. Surface these links explicitly - they're where insight lives.
</principle>

<principle name="use-the-cli">
Knowledge persists in SQLite via the `kr` CLI. Run commands to query, update, and explore the knowledge graph. The CLI is at `~/.claude/skills/explain-research/bin/kr`.
</principle>

</essential_principles>

<intake>
What would you like to do?

1. **Explain a paper** - Build intuitive understanding of research
2. **Update my mental model** - Deep dive on specific concept
3. **Explore connections** - Find links between ideas
4. **Review understanding** - Check and reinforce learning
5. **Something else**

**Wait for response before proceeding.**
</intake>

<routing>
| Response | Workflow |
|----------|----------|
| 1, "explain", "paper", "read", "understand" | `workflows/explain-paper.md` |
| 2, "update", "model", "concept", "deep dive" | `workflows/update-model.md` |
| 3, "connect", "link", "relate", "explore" | `workflows/explore-connections.md` |
| 4, "review", "check", "reinforce", "quiz" | `workflows/review-understanding.md` |
| 5, other | Clarify intent, then route |

**If user provides paper content without selecting:**
→ Default to `workflows/explain-paper.md`

**After reading the workflow, follow it exactly.**
</routing>

<input_handling>
The skill accepts multiple input formats:

**PDF files**: Use Read tool to extract content
**Pasted text**: Process inline text/excerpts directly
**URLs/arXiv**: Use WebFetch to retrieve paper content

When input is provided, detect format and extract content before proceeding with workflow.
</input_handling>

<knowledge_system>
Knowledge persists in SQLite via the `kr` CLI tool.

**CLI Location:** `~/.claude/skills/explain-research/bin/kr`

**Core Commands:**
```bash
# Concepts
kr concept add "Name" --understanding "..." --confidence high --analogy "..." --limits "..."
kr concept show "Name"
kr concept update "Name" --understanding "..." --confidence medium
kr concept list
kr concept list --confidence low

# Connections
kr connect "From" "To" --relation "uses" --note "..."

# Papers
kr paper add "Title" --url "..." --summary "..."
kr paper list
kr paper show <id|title>

# Insights (model updates)
kr insight add --insight "Key learning" --concept "Name" --before "Old thinking" --after "New thinking"
kr insight list

# Questions
kr question add "Open question" --context "From paper X"
kr question list
kr question resolve <id> --resolution "Answer"

# Spaced Repetition
kr review due
kr review done "Concept" --quality 4

# Exploration
kr search "query"
kr graph "Concept" --depth 2
kr graph "Concept" --format json
kr stats
kr export
```

**At session start:** Run `kr stats` to see current knowledge state.
**After learning:** Update concepts, add insights, create connections.
</knowledge_system>

<reference_index>
All domain knowledge in `references/`:

**Learning:** analogy-techniques.md, mental-model-patterns.md
**Domain:** ai-ml-concepts.md
</reference_index>

<workflows_index>
| Workflow | Purpose |
|----------|---------|
| explain-paper.md | Full paper explanation with intuition building |
| update-model.md | Deep dive on specific concepts |
| explore-connections.md | Surface links between ideas |
| review-understanding.md | Check and reinforce learning |
</workflows_index>

<success_criteria>
The skill succeeds when:
- You can explain the core idea to someone else using analogies
- Your mental model has explicitly updated
- You see connections to things you already know
- You've identified new areas to explore
- Knowledge graph updated via `kr` CLI
</success_criteria>
