<overview>
Patterns for building mental models that support intuitive reasoning. A good mental model lets you predict, explain, and create - not just recall.
</overview>

<model-types>

<type name="mechanism">
Answer: "How does it work?"

**Structure:**
- Inputs → Process → Outputs
- What transforms into what
- Why each step exists

**Template:**
```
MECHANISM: [Name]
Input: [What goes in]
Process: [What happens - sequential or parallel]
Output: [What comes out]
Key insight: [Why this design works]
```

**Example:**
```
MECHANISM: Self-Attention
Input: Sequence of token embeddings
Process:
  1. Each token creates query (what am I looking for?)
  2. Each token creates key (what am I about?)
  3. Each token creates value (what do I contribute?)
  4. Queries match against keys → attention weights
  5. Values aggregated by attention weights
Output: Context-aware token representations
Key insight: Every token can directly attend to every other token in O(n²)
```

Good for: Algorithms, processes, transformations
</type>

<type name="relational">
Answer: "How does it connect to other things?"

**Structure:**
- Central concept
- Related concepts (what it's similar to, different from, part of, composed of)
- Relationship types

**Template:**
```
RELATIONAL: [Name]
Is a type of: [parent category]
Is composed of: [components]
Is similar to: [analogous concepts] because [reason]
Is different from: [contrasts] because [reason]
Is used by: [things that depend on it]
```

**Example:**
```
RELATIONAL: Transformer
Is a type of: Sequence-to-sequence model
Is composed of: Multi-head attention + Feed-forward networks + Layer norm + Residual connections
Is similar to: RNN (sequence processing) but parallel not sequential
Is different from: CNN (no locality bias, global context)
Is used by: GPT, BERT, T5, and most modern LLMs
```

Good for: Taxonomies, architectures, frameworks
</type>

<type name="causal">
Answer: "Why does it happen?"

**Structure:**
- Effect (what you observe)
- Causes (what produces it)
- Mechanisms (how causes produce effects)
- Moderators (what changes the relationship)

**Template:**
```
CAUSAL: [Effect]
Caused by: [Primary causes]
Because: [Mechanism linking cause to effect]
Stronger when: [Moderators that amplify]
Weaker when: [Moderators that diminish]
Can be confused with: [Alternative explanations]
```

**Example:**
```
CAUSAL: In-context learning emerges
Caused by: Large-scale pretraining on diverse text
Because: Model learns meta-patterns - how to extract task structure from examples
Stronger when: More examples, clearer patterns, tasks similar to pretraining
Weaker when: Tasks require knowledge not in training, formats highly novel
Can be confused with: Memorization (but generalizes to new examples)
```

Good for: Phenomena, behaviors, emergent properties
</type>

<type name="constraint">
Answer: "What limits it?"

**Structure:**
- What you want to achieve
- What prevents it
- Trade-offs between goals
- Boundaries of possibility

**Template:**
```
CONSTRAINT: [Goal]
Limited by: [Hard constraints]
Trades off against: [Competing goals]
Optimal when: [Sweet spot conditions]
Fails when: [Boundary conditions]
```

**Example:**
```
CONSTRAINT: Model performance
Limited by: Compute, data, architectural expressiveness
Trades off against:
  - Size ↔ Inference speed
  - Generality ↔ Specialization
  - Capability ↔ Safety/alignment
Optimal when: Scaling laws followed, compute-optimal training
Fails when: Data quality degrades, distribution shift, adversarial inputs
```

Good for: Trade-offs, engineering decisions, system design
</type>

</model-types>

<building-patterns>

<pattern name="anchor-bridge-destination">
Start from known, build to unknown.

**Anchor**: What you already understand well
**Bridge**: How the new concept relates
**Destination**: The new understanding

**Example:**
- Anchor: You know how autocomplete predicts the next word
- Bridge: GPT is autocomplete trained on the entire internet, at massive scale
- Destination: Scale + diverse data → emergent capabilities beyond prediction

**Why it works**: New knowledge attaches to existing neural structures
</pattern>

<pattern name="progressive-refinement">
Start coarse, add detail.

**Level 1**: Core intuition (one sentence)
**Level 2**: Key components (3-5 parts)
**Level 3**: Mechanisms (how parts work)
**Level 4**: Edge cases and nuances

**Example - Transformers:**
1. "Architecture that processes sequences in parallel using attention"
2. "Encoder (understand input) + Decoder (generate output) + Attention (relate tokens)"
3. "Attention: Query-Key matching produces weights, weights aggregate Values"
4. "Multi-head gives different relationship types; layer norm stabilizes; residuals help gradients"

**Why it works**: Each level is self-contained; you can stop when sufficient
</pattern>

<pattern name="contrast-frame">
Define by what it's not.

**Template:**
"Unlike [contrast], [concept] does [difference] because [reason]."

**Example:**
"Unlike RNNs that process tokens sequentially, Transformers process all tokens in parallel. This is possible because attention directly connects any two positions, removing the need for information to flow step-by-step."

**Why it works**: Differences are more memorable than similarities
</pattern>

<pattern name="extreme-cases">
Understand by pushing to limits.

**Questions:**
- What happens with zero [input/parameter]?
- What happens with infinite [input/parameter]?
- What's the simplest case where it works?
- What's the simplest case where it fails?

**Example - Attention:**
- Zero attention → Token isolated, no context
- Uniform attention → Average of all tokens (lossy bag-of-tokens)
- Single attended token → Copy operation
- All self-attention → Token unchanged

**Why it works**: Extremes reveal structure that normal cases hide
</pattern>

</building-patterns>

<quality-checks>

<check name="prediction">
A good mental model lets you predict outcomes.

**Test**: Given a new scenario, what does your model say will happen?
**Failure mode**: Model that only describes past, can't project to future

**Example test**: "If we double the context length, what happens to attention computational cost?"
Good model predicts: "Quadratic in context length, so 4x compute."
</check>

<check name="explanation">
A good mental model generates explanations.

**Test**: When you see a result, can your model explain why?
**Failure mode**: Model that matches patterns but can't provide reasons

**Example test**: "Why do larger models show emergent capabilities?"
Good model explains: "More parameters → better compression → captures more patterns → threshold where meta-patterns sufficient for new capabilities."
</check>

<check name="generation">
A good mental model enables creation.

**Test**: Can you design something new using the model?
**Failure mode**: Model that only recognizes, can't produce

**Example test**: "Design a modification to attention for very long contexts."
Good model generates: "Sparse attention patterns (local + strided + global) to reduce quadratic cost while maintaining key connections."
</check>

<check name="connection">
A good mental model links to other knowledge.

**Test**: What other concepts does this relate to? How?
**Failure mode**: Isolated knowledge that doesn't integrate

**Example test**: "How does attention relate to human cognition?"
Good model connects: "Both are selective - limited capacity requires focusing resources. But human attention is sequential; transformer attention is parallel."
</check>

</quality-checks>

<evolution>
Mental models mature through stages:

**Fragile**: Can recall but easily confused
→ Strengthen with: More examples, explicit structure

**Rigid**: Accurate but narrow
→ Loosen with: Variations, edge cases, counterexamples

**Flexible**: Adapts to contexts but might overextend
→ Refine with: Constraints, failure cases, boundaries

**Robust**: Accurate, adaptable, knows its limits
→ This is the goal

Track where each concept is and what it needs.
</evolution>
