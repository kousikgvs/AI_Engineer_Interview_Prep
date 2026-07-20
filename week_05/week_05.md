# Week 5 — Sequence Models & The Road to Transformers

**Block:** B — Transformers + LLM Internals  
**Goal:** Understand why transformers replaced RNNs. Know the history, not just the answer.

---

## What You Will Learn

### Sequence Modeling History
- Recurrent Neural Networks (RNNs) — how they work and why they struggle
- Vanishing gradient problem in sequences — formal analysis
- LSTMs — forget gate, input gate, output gate, cell state
- GRUs — simplified gating
- Sequence-to-sequence models — encoder-decoder architecture
- The attention mechanism in its original form (Bahdanau, 2015)
- Why attention was a breakthrough: it removes the bottleneck of a fixed-size vector

### Tokenization
- Why we tokenize text at all
- Character-level tokenization — pros and cons
- Word-level tokenization — the out-of-vocabulary problem
- Byte Pair Encoding (BPE) — algorithm walkthrough
- WordPiece — how BERT tokenizes; likelihood-based merges vs BPE
- SentencePiece — language-agnostic subword tokenization
- tiktoken — how OpenAI tokenizes
- Vocabulary size and vocabulary design tradeoffs
- Tokenization failure modes (non-English text, code, math)

### Embeddings
- Word2Vec — CBOW and Skip-gram
- GloVe
- What embeddings actually represent geometrically
- Positional encoding — sinusoidal (original) and learned
- The embedding layer as a lookup table

### Research Taste (First Look)
- How to read a paper: abstract → introduction → figures → related work → method → results
- How to tell foundational papers apart from hype
- Paper categories: Foundation / Scaling / Systems / Alignment / Agent papers

---

## Project
- Build a character-level language model (vanilla RNN, then LSTM)
- Implement a BPE tokenizer from scratch
- Visualize attention weights in a seq2seq model
- Compare LSTM vs RNN on a long-sequence task

---

## Paper to Read
- "Neural Machine Translation by Jointly Learning to Align and Translate" — Bahdanau et al. (2015) — the birth of attention

---

## Failure Friday
- Analyze: A production NLP system that broke because of tokenization — common in non-English deployments

---

## Interview Questions This Week Prepares You For

<details>
<summary>"Why did attention replace recurrence?"</summary>

Attention models any-to-any dependencies in one step (no long gradient chains), parallelizes over the sequence for GPU efficiency, and scales — enabling transformers to beat RNNs.
</details>

<details>
<summary>"Walk me through BPE tokenization"</summary>

Start from characters, repeatedly merge the most frequent adjacent pair into a new token, and repeat to the target vocab size. Rare words split into known subwords, solving out-of-vocabulary issues.
</details>

<details>
<summary>"What does an embedding actually represent?"</summary>

A dense vector in a space where distance/direction encodes meaning — similar items are near each other and relations appear as directions.
</details>

---

## Engineering Judgment Question

<details>
<summary><strong>"Character-level or BPE tokenization for code?"</strong></summary>

**What I'd do:** code-aware BPE — keep common identifiers/keywords as single tokens to shorten sequences.
**Why:** shorter sequences mean cheaper attention and better modeling of frequent code patterns.
**Tradeoff:** char-level generalizes to any symbol but makes sequences long/expensive.
**Rejected:** char-level except when modeling rare/arbitrary tokens matters most.
</details>
