<overview>
Core AI/ML concepts with intuitive explanations and structural analogies. Reference for connecting new papers to established foundations. Not exhaustive - grow this as needed.
</overview>

<foundational>

<concept name="gradient-descent">
## Gradient Descent
**What it is**: Optimization algorithm that finds minimum of a function by following the slope downward.

**Analogy**: Navigating a foggy mountain to find the lowest valley - you can only feel the slope under your feet, so you step downhill repeatedly.

**Key variants**:
- Stochastic (SGD): Steps based on small random samples
- Mini-batch: Steps based on medium batches
- Adam: Adaptive learning rates per parameter

**Connects to**: Backpropagation (how gradients are computed), Loss functions (what we're minimizing)
</concept>

<concept name="loss-function">
## Loss Function
**What it is**: Mathematical measure of how wrong the model's predictions are.

**Analogy**: A scoreboard that tells you how badly you're losing - optimization tries to improve this score.

**Key types**:
- Cross-entropy: For classification (measures probability mismatch)
- MSE: For regression (measures distance from target)
- Contrastive: For embeddings (similar things close, different things far)

**Connects to**: Gradient descent (loss gradients guide updates), Overfitting (training loss vs. validation loss)
</concept>

<concept name="backpropagation">
## Backpropagation
**What it is**: Algorithm to compute gradients of loss with respect to every parameter by working backward through the network.

**Analogy**: Blame assignment - when the output is wrong, backprop figures out which earlier decisions contributed to the mistake and by how much.

**Key insight**: Chain rule of calculus, applied efficiently by caching intermediate values during forward pass.

**Connects to**: Gradient descent (backprop provides gradients), Computational graphs (structure enables backprop)
</concept>

<concept name="overfitting">
## Overfitting
**What it is**: Model performs well on training data but poorly on new data - it memorized rather than learned.

**Analogy**: Student who memorized test answers vs. student who understood the subject. First fails when questions change.

**Symptoms**: Training loss decreasing while validation loss increases

**Remedies**: Regularization, dropout, early stopping, more data, smaller model

**Connects to**: Generalization (the opposite goal), Regularization (techniques to prevent)
</concept>

</foundational>

<architectures>

<concept name="transformer">
## Transformer
**What it is**: Architecture that processes sequences using self-attention, enabling parallel computation and direct connections between any positions.

**Analogy**: A group discussion where everyone can hear everyone simultaneously, rather than passing notes one person at a time (RNN).

**Key components**:
- Multi-head attention: Multiple parallel attention patterns
- Feed-forward networks: Per-position transformations
- Layer normalization: Stabilizes training
- Residual connections: Helps gradients flow

**Why it dominates**: Parallelizable (faster training), handles long-range dependencies, scales with compute

**Connects to**: Attention mechanism (core operation), BERT/GPT (specific transformer configurations)
</concept>

<concept name="attention">
## Attention Mechanism
**What it is**: Mechanism that computes weighted combinations where weights depend on content - lets model focus on relevant parts.

**Analogy**: A searchlight that can highlight multiple things at once with different intensities, where what gets highlighted depends on what you're looking for.

**The formula**: Attention(Q,K,V) = softmax(QK'/√d)V
- Q (query): What are we looking for?
- K (keys): What does each position offer?
- V (values): What content to retrieve?

**Why √d**: Prevents dot products from getting too large as dimension increases (would make softmax too sharp)

**Connects to**: Transformer (attention is the core), Self-attention (Q,K,V all from same sequence)
</concept>

<concept name="embeddings">
## Embeddings
**What it is**: Dense vector representations where similar items are close in vector space.

**Analogy**: GPS coordinates for concepts - items that are related are in the same neighborhood.

**Key properties**:
- Learned, not designed: Network discovers useful dimensions
- Compositional: King - Man + Woman ≈ Queen (sometimes)
- Transfer well: Embeddings trained on one task often useful for others

**Types**:
- Token embeddings: Words/subwords → vectors
- Position embeddings: Sequence position → vectors
- Learned vs. fixed: Some are trainable, some computed

**Connects to**: Representation learning (embeddings are learned representations), Similarity search (embeddings enable)
</concept>

</architectures>

<training-concepts>

<concept name="pre-training">
## Pre-training
**What it is**: Training on large general data before specializing on specific task.

**Analogy**: General education before specialization - learn to read, write, think before focusing on medicine or law.

**Common objectives**:
- Language modeling: Predict next token
- Masked language modeling: Fill in blanks
- Contrastive learning: Match related items

**Why it works**: General patterns (grammar, facts, reasoning) transfer to specific tasks

**Connects to**: Fine-tuning (specialization after pre-training), Transfer learning (broader concept)
</concept>

<concept name="fine-tuning">
## Fine-tuning
**What it is**: Additional training on specific task/domain after pre-training.

**Analogy**: Medical student doing residency - general training adapts to specialty.

**Variants**:
- Full fine-tuning: Update all parameters
- Parameter-efficient (LoRA, adapters): Update small subset
- Prompt tuning: Only tune prompt embeddings

**Risk**: Catastrophic forgetting (lose general knowledge), overfitting to small dataset

**Connects to**: Pre-training (what comes before), Transfer learning (fine-tuning is a form)
</concept>

<concept name="in-context-learning">
## In-Context Learning
**What it is**: Model adapts to new task from examples in the prompt, without parameter updates.

**Analogy**: Showing a skilled person examples of what you want, and they figure out the pattern without explicit instruction.

**Requirements**: Large model, diverse pre-training, well-formatted examples

**Not**: Fine-tuning (no weight changes), retrieval (not looking up answers)

**Why mysterious**: Emerges at scale, mechanism not fully understood - possibly implicit Bayesian inference or gradient descent in activation space

**Connects to**: Emergent capabilities (ICL emerges with scale), Prompt engineering (how to elicit ICL)
</concept>

</training-concepts>

<emergent-phenomena>

<concept name="scaling-laws">
## Scaling Laws
**What it is**: Predictable relationship between compute/data/parameters and model performance.

**Key insight**: Performance improves as a power law with each factor (roughly log-linear on plots)

**Chinchilla insight**: Compute-optimal training balances parameters and data - most models were over-parameterized, under-trained

**Connects to**: Emergent capabilities (appear at scale thresholds), Pre-training (scale enables better pre-training)
</concept>

<concept name="emergent-capabilities">
## Emergent Capabilities
**What it is**: Abilities that appear suddenly at scale, not present in smaller models.

**Examples**:
- Chain-of-thought reasoning
- In-context learning
- Code generation
- Instruction following

**Analogy**: Water suddenly boiling at 100°C - gradual temperature increase, sudden phase change

**Debate**: Are they truly emergent or measurement artifacts? (Metric choice affects apparent emergence)

**Connects to**: Scaling laws (capabilities track with scale), In-context learning (example emergent capability)
</concept>

</emergent-phenomena>

<recent-concepts>

<concept name="rlhf">
## RLHF (Reinforcement Learning from Human Feedback)
**What it is**: Training method where model is optimized to produce outputs humans prefer.

**Process**:
1. Collect human comparisons (A vs. B, which is better?)
2. Train reward model on preferences
3. Use RL to optimize policy against reward model

**Why needed**: Language modeling objective (predict next token) doesn't directly optimize for helpful/harmless

**Challenges**: Reward hacking, distribution shift, scalable oversight

**Connects to**: Fine-tuning (RLHF is a fine-tuning approach), Alignment (RLHF is an alignment technique)
</concept>

<concept name="chain-of-thought">
## Chain of Thought
**What it is**: Prompting technique where model shows reasoning steps before final answer.

**Analogy**: Showing your work in math - intermediate steps make complex reasoning tractable

**Why it helps**: Model has limited "working memory" - CoT externalizes intermediate computation

**Variants**:
- Zero-shot: "Let's think step by step"
- Few-shot: Examples with reasoning traces
- Self-consistency: Generate multiple chains, vote on answer

**Connects to**: Emergent capabilities (CoT is emergent), In-context learning (CoT works via ICL)
</concept>

<concept name="retrieval-augmented-generation">
## RAG (Retrieval-Augmented Generation)
**What it is**: Combining LLM with external retrieval - fetch relevant documents then generate based on them.

**Analogy**: Open-book exam - you can look things up before answering

**Why needed**: LLMs have fixed knowledge cutoff, limited context, can hallucinate

**Components**:
- Retriever: Find relevant documents (often embedding similarity)
- Generator: LLM that conditions on retrieved content

**Connects to**: In-context learning (retrieved docs become context), Embeddings (retrieval uses embedding similarity)
</concept>

</recent-concepts>

<usage-note>
This reference is intentionally not exhaustive. Add concepts as they become relevant to your learning. The goal is building connected understanding, not encyclopedic coverage.

When adding a new concept:
1. What it is (1-2 sentences)
2. Key analogy (intuitive handle)
3. Why it matters (role in the field)
4. Connects to (links to existing concepts)
</usage-note>
