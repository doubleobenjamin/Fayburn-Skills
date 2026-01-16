<required_reading>
**Read these files NOW:**
1. references/analogy-techniques.md
2. references/mental-model-patterns.md

**Check current knowledge state:**
```bash
kr stats
kr concept list
```
</required_reading>

<objective>
Transform a research paper into intuitive understanding through analogical reasoning. Build mental models that let you think creatively with the concepts, not just recite them.
</objective>

<process>

<step name="1-extract-input">
## Step 1: Get Paper Content

**PDF file**: Read the file, extract key sections (abstract, intro, method, results, discussion)
**Pasted text**: Work with provided excerpts
**URL/arXiv**: Fetch content with WebFetch

Focus on: abstract, introduction, core method, key results, discussion/implications.
</step>

<step name="2-identify-core-question">
## Step 2: Identify the Core Question

Every paper answers a question. Find it.

Ask yourself:
- What problem drove this research?
- What gap in understanding does it fill?
- What would change if this work didn't exist?

**Output format:**
```
CORE QUESTION: [One sentence - what this paper is really asking]
```
</step>

<step name="3-build-grounding-analogy">
## Step 3: Build Grounding Analogy

Before any technical detail, create an analogy that captures the essence.

**Good analogies:**
- Map structure, not just surface features
- Use familiar domains (cooking, sports, navigation, building)
- Are wrong in specific, learnable ways

**Output format:**
```
GROUNDING ANALOGY:
[Familiar situation] is like [paper's approach] because [structural similarity].

This analogy breaks down when: [specific limitations]
```
</step>

<step name="4-extract-key-concepts">
## Step 4: Extract Key Concepts (3-5 max)

Identify the concepts essential to understanding. For each:

1. **Technical name** - what experts call it
2. **Plain meaning** - what it actually does
3. **Mini-analogy** - intuitive handle
4. **Why it matters** - role in the bigger picture

**Check if concept exists:**
```bash
kr search "concept name"
```

**Output format:**
```
CONCEPT: [Name]
Plain meaning: [One sentence]
Analogy: [Familiar comparison]
Matters because: [Role in paper's contribution]
```
</step>

<step name="5-explain-mechanism">
## Step 5: Explain the Mechanism

Walk through HOW the paper's approach works. Use the grounding analogy as scaffolding.

Structure:
1. Start with inputs (what goes in)
2. Explain transformation (what happens)
3. End with outputs (what comes out)
4. Note key design choices and why they matter

Use visuals if helpful:
```
[Input] → [Process A] → [Process B] → [Output]
           ↑              ↑
     [Why this way]  [Key insight]
```
</step>

<step name="6-explicit-model-update">
## Step 6: Make Mental Model Update Explicit

State clearly how understanding should change.

**Output format:**
```
MENTAL MODEL UPDATE:

BEFORE: You might have thought [common misconception or gap]
AFTER: Now you understand [new insight]
BECAUSE: [The evidence/reasoning from this paper]

This changes how you think about: [related concepts or problems]
```
</step>

<step name="7-surface-connections">
## Step 7: Surface Connections

Query existing knowledge to find links:
```bash
kr concept list
kr search "related term"
```

Link this paper to existing knowledge:
- What concepts does this relate to?
- What papers or ideas does it build on or challenge?
- What new questions does it open?

**Output format:**
```
CONNECTIONS:
- Relates to: [existing concept] - [how]
- Builds on: [prior work] - [what it adds]
- Opens questions about: [new territory]
```
</step>

<step name="8-identify-exploration">
## Step 8: Identify Exploration Directions

What could you explore next based on this paper?

- Concepts to go deeper on
- Related papers to read
- Applications to consider
- Questions that emerged

**Output format:**
```
EXPLORE NEXT:
- Deep dive: [concept needing more attention]
- Related: [papers or topics to investigate]
- Apply: [potential applications or experiments]
- Questions: [things you're now curious about]
```
</step>

<step name="9-persist-knowledge">
## Step 9: Persist to Knowledge Graph

Record the paper and learnings:

```bash
# Add paper
kr paper add "Paper Title" --url "URL" --summary "Brief summary"

# Add/update concepts
kr concept add "ConceptName" \
  --understanding "What it is and does" \
  --confidence medium \
  --analogy "Intuitive comparison" \
  --limits "Where analogy breaks"

# Connect concepts
kr connect "ConceptA" "ConceptB" --relation "uses" --note "How they relate"

# Record key insight
kr insight add \
  --insight "The key learning" \
  --concept "RelatedConcept" \
  --before "What you thought before" \
  --after "What you understand now"

# Add open questions
kr question add "Question that emerged" --context "From this paper"
```
</step>

</process>

<output_formats>

<format name="conversational">
## Conversational Mode (Default)

Explain interactively, checking understanding as you go. Pause after major sections. Invite questions. Adjust depth based on responses.
</format>

<format name="structured-summary">
## Structured Summary

When requested, produce a markdown document:

```markdown
# [Paper Title]

## Core Question
[What this paper asks]

## Grounding Analogy
[The intuitive frame]

## Key Concepts
[Concept list with analogies]

## How It Works
[Mechanism explanation]

## Mental Model Update
[Before/after framing]

## Connections
[Links to existing knowledge]

## Explore Next
[Future directions]
```
</format>

<format name="quick-take">
## Quick Take

For fast understanding:
- Core question (1 sentence)
- Grounding analogy (2-3 sentences)
- Key insight (1-2 sentences)
- Model update (before/after)
</format>

</output_formats>

<success_criteria>
Paper explanation complete when:
- [ ] Core question clearly identified
- [ ] Grounding analogy created and limitations noted
- [ ] Key concepts extracted with mini-analogies
- [ ] Mechanism explained using analogy scaffolding
- [ ] Mental model update explicitly stated
- [ ] Connections to existing knowledge surfaced
- [ ] Exploration directions identified
- [ ] Knowledge persisted via `kr` CLI
- [ ] User can explain core idea back using analogies
</success_criteria>
