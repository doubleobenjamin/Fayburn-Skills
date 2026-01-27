<required_reading>
**Query knowledge graph for context:**
```bash
~/.claude/skills/explain-research/bin/kr concept list
~/.claude/skills/explain-research/bin/kr stats
```

**Read domain reference:**
1. references/ai-ml-concepts.md (for domain context)
</required_reading>

<objective>
Surface hidden connections between concepts, papers, or ideas. Find the links where insight lives. Enable creative thinking by making the implicit explicit.
</objective>

<process>

<step name="1-gather-inputs">
What are you connecting?

Options:
- **Two concepts**: How does X relate to Y?
- **Paper to knowledge**: How does this paper connect to what I know?
- **Open exploration**: What in my concept graph connects unexpectedly?
- **Problem framing**: How do my concepts illuminate this problem?

Get the specific items to explore.
</step>

<step name="2-map-existing-connections">
Pull relevant entries from knowledge graph:

```bash
~/.claude/skills/explain-research/bin/kr concept show "ConceptA"
~/.claude/skills/explain-research/bin/kr concept show "ConceptB"
~/.claude/skills/explain-research/bin/kr graph "ConceptA" --depth 2
```

For each item:
- Current understanding
- Known analogies
- Existing connections
- Domain context

**Output format:**
```
MAPPING:

[Item A]:
- Understanding: [summary]
- Analogies: [key mental handles]
- Currently connects to: [list]

[Item B]:
- Understanding: [summary]
- Analogies: [key mental handles]
- Currently connects to: [list]
```
</step>

<step name="3-find-structural-similarities">
Look beyond surface features. Find structural parallels:

**Process similarity**: Do they work the same way?
**Problem similarity**: Do they solve similar problems?
**Trade-off similarity**: Do they face similar constraints?
**Component similarity**: Do they share building blocks?
**Evolution similarity**: Did they develop similarly?

**Output format:**
```
STRUCTURAL PARALLELS:

[Type of similarity]:
[Item A] [similarity description] [Item B]
Because: [why this parallel exists]
Implication: [what this means for understanding]
```
</step>

<step name="4-test-analogy-transfer">
Can an analogy from one concept illuminate the other?

Take your best analogy for A. Apply it to B.
- What does it explain?
- What does it miss?
- What does the mismatch reveal?

**Output format:**
```
ANALOGY TRANSFER:

Using [A's analogy] to understand [B]:
- Explains: [what maps well]
- Misses: [what doesn't transfer]
- Reveals: [insight from the mismatch]
```
</step>

<step name="5-identify-generative-connections">
Which connections enable new thinking?

A generative connection lets you:
- Predict something new
- See a problem differently
- Import solutions across domains
- Ask better questions

**Output format:**
```
GENERATIVE CONNECTIONS:

[Connection]: [A] ↔ [B] via [link type]
Enables: [new capability or question]

Value: [high/medium/low]
Confidence: [strong/tentative/speculative]
```
</step>

<step name="6-surface-creative-possibilities">
What does this connection make possible?

- **Research directions**: What could you investigate?
- **Applications**: What could you build?
- **Questions**: What should you ask?
- **Papers to read**: What would deepen this?

**Output format:**
```
POSSIBILITIES:

Research: [potential investigation]
Build: [potential application]
Ask: [new question enabled]
Read: [related work to explore]
```
</step>

<step name="7-persist-connections">
Add discovered connections to the knowledge graph:

```bash
# Create connection
~/.claude/skills/explain-research/bin/kr connect "ConceptA" "ConceptB" \
  --relation "analogous-to" \
  --note "Both use weighted aggregation for selection"

# Record insight about the connection
~/.claude/skills/explain-research/bin/kr insight add \
  --insight "Connection between A and B reveals common pattern of X"

# Add questions that emerged
~/.claude/skills/explain-research/bin/kr question add "Could technique from A apply to B?" \
  --context "Connection exploration"
```
</step>

</process>

<connection-types>

<type name="causal">
A influences or causes B.
Example: Attention mechanism enables transformer performance
</type>

<type name="compositional">
A is built from B (or vice versa).
Example: Transformer = attention + feedforward + residual
</type>

<type name="analogical">
A and B share structure but different domains.
Example: Neural attention ~ human selective focus
</type>

<type name="oppositional">
A and B represent different choices on a trade-off.
Example: Model size vs. inference speed
</type>

<type name="evolutionary">
A developed into B over time.
Example: RNN → LSTM → Transformer
</type>

<type name="problem-space">
A and B both address the same underlying challenge.
Example: Dropout, batch norm, weight decay all fight overfitting
</type>

</connection-types>

<success_criteria>
Connection exploration complete when:
- [ ] Input items mapped with existing knowledge
- [ ] Structural parallels identified
- [ ] Analogy transfer tested
- [ ] Generative connections surfaced with confidence levels
- [ ] Creative possibilities articulated
- [ ] Knowledge graph updated via `kr` CLI
- [ ] At least one "aha" connection that enables new thinking
</success_criteria>
