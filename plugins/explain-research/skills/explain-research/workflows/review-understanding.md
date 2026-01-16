<required_reading>
**Check knowledge state:**
```bash
kr stats
kr review due
kr concept list --confidence low
kr question list
```
</required_reading>

<objective>
Check and reinforce your understanding. Identify gaps, strengthen weak areas, and consolidate learning. Move knowledge from fragile to robust.
</objective>

<process>

<step name="1-select-review-mode">
## Step 1: Select Review Mode

How would you like to review?

**Concept quiz**: Test understanding of specific concepts
**Explain back**: Articulate your understanding for feedback
**Gap analysis**: Identify what's weak or missing
**Connection challenge**: Find links between random concepts
**Application test**: Use concepts to analyze a new problem

Ask user preference or suggest based on knowledge state.
</step>

<step name="2-assess-knowledge-state">
## Step 2: Assess Knowledge State

Review the knowledge graph:

```bash
kr stats
kr concept list
kr review due
kr question list
```

**Output format:**
```
KNOWLEDGE STATE:

Strong areas: [concepts with high confidence + connections]
Weak areas: [low confidence or few connections]
Due for review: [concepts from kr review due]
Open questions: [unresolved from kr question list]

Recommended focus: [where to direct review]
```
</step>

<step name="3-execute-review">
## Step 3: Execute Review

Based on selected mode:

**Concept Quiz:**
```
QUESTION: Explain [concept] in your own words.
FOLLOW-UP: How does it connect to [related concept]?
EDGE CASE: What happens when [boundary condition]?
```

**Explain Back:**
"Walk me through [concept/paper]. I'll give feedback on accuracy and gaps."

**Gap Analysis:**
Systematically check each weak area:
- What do you think you know?
- Where does certainty break down?
- What would you need to feel confident?

**Connection Challenge:**
"How does [random concept A] relate to [random concept B]?"
Surface unexpected links.

**Application Test:**
Present a new problem or paper snippet.
"Using what you know about [concepts], analyze this."
</step>

<step name="4-identify-gaps">
## Step 4: Identify Gaps

What did the review reveal?

**Recall gaps**: Forgot specific details
**Understanding gaps**: Can state but can't explain why
**Connection gaps**: Know concepts but don't see links
**Application gaps**: Understand but can't use

**Output format:**
```
GAPS IDENTIFIED:

[Gap type]: [specific gap]
Evidence: [what revealed it]
Priority: [high/medium/low based on importance]
```
</step>

<step name="5-reinforce-and-correct">
## Step 5: Reinforce and Correct

For each gap:

**Recall gaps**: Re-explain the concept, add memorable hooks
**Understanding gaps**: Go deeper with scaffolded explanation
**Connection gaps**: Explicitly work through the connection
**Application gaps**: Walk through application step by step

Use the update-model workflow for significant understanding gaps.
</step>

<step name="6-consolidate">
## Step 6: Consolidate

Summarize what was reinforced:

**Output format:**
```
REVIEW SUMMARY:

Tested: [concepts/areas reviewed]
Confirmed strong: [what held up well]
Gaps found and addressed: [what needed work]
Still need attention: [remaining weak spots]

Confidence change: [overall assessment shift]
```
</step>

<step name="7-persist-review">
## Step 7: Persist to Knowledge Graph

Update knowledge graph with review results:

```bash
# Mark concepts as reviewed with quality rating
# Quality: 1=forgot, 2=struggled, 3=okay, 4=good, 5=perfect
kr review done "Concept1" --quality 4
kr review done "Concept2" --quality 3

# Update confidence for concepts that changed
kr concept update "WeakConcept" --confidence medium

# Resolve any questions that were answered
kr question resolve 1 --resolution "Answer discovered during review"

# Add new questions that emerged
kr question add "New question from review" --context "Review session"
```
</step>

</process>

<spaced-repetition>
## Spaced Repetition System

The `kr review` commands implement spaced repetition:

**Quality ratings determine next review:**
- Quality 1: Review in 1 day (forgot completely)
- Quality 2: Review in 3 days (struggled significantly)
- Quality 3: Review in 7 days (okay with effort)
- Quality 4: Review in 14 days (good recall)
- Quality 5: Review in 30 days (perfect recall)

**Check what's due:**
```bash
kr review due
```

**Regular practice:** Check `kr review due` at start of each session.
</spaced-repetition>

<success_criteria>
Review complete when:
- [ ] Knowledge state assessed via CLI
- [ ] Review mode executed
- [ ] Gaps identified with evidence
- [ ] Weak areas reinforced or corrected
- [ ] Summary of what changed produced
- [ ] Knowledge graph updated with review results
- [ ] Next review priorities clear
</success_criteria>
