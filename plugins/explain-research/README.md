# explain-research

A Claude Code plugin for building intuitive understanding of research papers through analogical reasoning.

## Overview

This plugin helps you:
- **Explain papers** with intuition-first analogies before technical detail
- **Track concepts** with persistent SQLite storage
- **Build connections** between ideas across papers
- **Review understanding** with spaced repetition

## Installation

1. Install the plugin:
```bash
claude plugins add explain-research
```

2. Install the CLI tool (for knowledge persistence):
```bash
# Copy the kr CLI to your PATH
cp ~/.claude/plugins/cache/explain-research/*/bin/kr ~/.local/bin/
chmod +x ~/.local/bin/kr
```

## Usage

### Explain a Paper

```bash
/explain-research https://arxiv.org/abs/1706.03762
```

Or paste paper content directly and run `/explain-research`.

### CLI Commands

The `kr` CLI manages your knowledge graph:

```bash
# Concepts
kr concept add "Transformer" --understanding "..." --confidence high --analogy "..."
kr concept show "Transformer"
kr concept list
kr concept list --confidence low

# Connections
kr connect "Transformer" "Attention" --relation "uses" --note "Core mechanism"

# Papers
kr paper add "Attention Is All You Need" --url "..." --summary "..."
kr paper list

# Insights (mental model updates)
kr insight add --insight "Key learning" --before "Old thinking" --after "New thinking"

# Questions
kr question add "Why does X work?" --context "From paper Y"
kr question list
kr question resolve 1 --resolution "Because..."

# Spaced Repetition
kr review due
kr review done "Transformer" --quality 4

# Exploration
kr search "attention"
kr graph "Transformer" --depth 2
kr stats
kr export
```

## Workflows

| Workflow | Purpose |
|----------|---------|
| explain-paper | Full paper explanation with intuition building |
| update-model | Deep dive on specific concepts |
| explore-connections | Surface links between ideas |
| review-understanding | Spaced repetition review |

## Core Principles

1. **Intuition First**: Lead with analogies before jargon
2. **Explicit Model Updates**: "Before you thought X, now you understand Y because Z"
3. **Analogies Are Bridges**: Use them to transport intuition, then refine toward accuracy
4. **Connections Compound**: Every paper links to existing knowledge

## Data Storage

Knowledge persists in SQLite at `~/.claude/skills/explain-research/data/knowledge.db`:

- **concepts**: Name, understanding, confidence, analogies
- **connections**: Typed relationships between concepts
- **papers**: Title, URL, summary, read date
- **insights**: Before/after model updates
- **questions**: Open questions and resolutions
- **reviews**: Spaced repetition tracking

## Example Session

```
User: /explain-research [pastes paper abstract about RLHF]

Claude: Let me explain this paper...

CORE QUESTION: How can we train language models to produce outputs humans actually prefer?

GROUNDING ANALOGY:
RLHF is like training a chef by having food critics rate dishes instead of following recipes.
The chef learns to make food people enjoy, not just technically correct food.
This breaks when: Critics might have biases, and optimizing too hard for ratings can lead to "junk food" outputs.

[continues with concepts, mechanism, model update...]

# Persisting to knowledge graph
kr concept add "RLHF" --understanding "Training via human preference comparisons" --confidence medium
kr paper add "Training language models with human feedback" --summary "Introduced RLHF for LLM alignment"
kr insight add --insight "Reward models can be gamed" --before "Thought RLHF directly optimizes helpfulness" --after "Understand RLHF optimizes a proxy that can diverge"
```

## License

MIT
