<required_reading>
**Read these files NOW:**
1. references/mental-model-patterns.md

**Check existing knowledge:**
```bash
~/.claude/skills/explain-research/bin/kr concept list
~/.claude/skills/explain-research/bin/kr concept show "TargetConcept"
```
</required_reading>

<objective>
Deep dive into a specific concept to strengthen or correct your mental model. Move from surface understanding to intuition you can reason with.
</objective>

<process>

<step name="1-identify-target">
What concept needs work?

Options:
- User specifies directly
- Check concepts needing review: `kr review due`
- Check low-confidence concepts: `kr concept list --confidence low`

Get the concept name and your current understanding level.
</step>

<step name="2-assess-current-state">
What do you currently understand about this concept?

```bash
~/.claude/skills/explain-research/bin/kr concept show "ConceptName"
~/.claude/skills/explain-research/bin/kr graph "ConceptName" --depth 1
```

**Output format:**
```
CONCEPT: [Name]

CURRENT UNDERSTANDING:
- What I think it is: [current mental model]
- Confidence level: [high/medium/low/confused]
- Where it came from: [source of current understanding]
- What feels unclear: [specific confusions or gaps]
```
</step>

<step name="3-identify-model-type">
Different concepts need different model types:

**Mechanism model**: How does it work? (for processes, algorithms)
**Relational model**: How does it connect? (for frameworks, taxonomies)
**Causal model**: Why does it happen? (for phenomena, effects)
**Constraint model**: What limits it? (for trade-offs, boundaries)

Pick the model type that matches the concept and your confusion.
</step>

<step name="4-build-scaffolded-explanation">
Start from something you DO understand. Build a bridge.

Pattern:
1. **Anchor**: Something familiar and well-understood
2. **Bridge**: How the new concept relates
3. **Destination**: The new understanding
4. **Refinement**: Where the analogy breaks and what's actually true

**Output format:**
```
SCAFFOLD:

Anchor: [What you already get]
Bridge: [Name] is like [anchor] but [key difference]
Destination: Which means [new understanding]
Refinement: The analogy breaks because [limitation] - actually [truth]
```
</step>

<step name="5-test-understanding">
Verify the model works by applying it:

**Prediction test**: If the model is right, what should happen in [scenario]?
**Edge case test**: What happens at extremes?
**Connection test**: How does this explain [related thing]?

If tests fail, the model needs refinement.
</step>

<step name="6-crystallize-update">
State the model change explicitly.

**Output format:**
```
MODEL UPDATE:

BEFORE: [Old understanding or confusion]
AFTER: [New understanding]
KEY INSIGHT: [The thing that made it click]

I can now: [new capability this enables]
This connects to: [related concepts that make more sense now]
```
</step>

<step name="7-persist-update">
Update the concept and record the insight:

```bash
# Update concept understanding
~/.claude/skills/explain-research/bin/kr concept update "ConceptName" \
  --understanding "New, refined understanding" \
  --confidence high

# Record the model update as insight
~/.claude/skills/explain-research/bin/kr insight add \
  --insight "The key insight that clarified this" \
  --concept "ConceptName" \
  --before "What I thought before" \
  --after "What I understand now"

# Add new connections discovered
~/.claude/skills/explain-research/bin/kr connect "ConceptName" "RelatedConcept" \
  --relation "enables" \
  --note "Connection discovered during deep dive"

# Mark as reviewed if using spaced repetition
~/.claude/skills/explain-research/bin/kr review done "ConceptName" --quality 4
```
</step>

</process>

<depth-levels>

<level name="surface">
- Can recognize the concept
- Know roughly what domain it's in
- Can use it in conversation

Upgrade by: Finding a grounding analogy
</level>

<level name="functional">
- Can explain what it does
- Know when it applies
- Can identify examples

Upgrade by: Understanding the mechanism
</level>

<level name="mechanistic">
- Know how it works internally
- Can predict behavior
- Understand design choices

Upgrade by: Exploring edge cases and trade-offs
</level>

<level name="generative">
- Can extend to new situations
- See connections others miss
- Can innovate with the concept

This is the goal. You can think creatively with the concept.
</level>

</depth-levels>

<success_criteria>
Model update complete when:
- [ ] Current understanding clearly assessed
- [ ] Right model type identified
- [ ] Scaffolded explanation built from familiar anchor
- [ ] Understanding tested with predictions/edge cases
- [ ] Update crystallized in before/after format
- [ ] Knowledge graph updated via `kr` CLI
- [ ] You can use this concept to think about new problems
</success_criteria>
