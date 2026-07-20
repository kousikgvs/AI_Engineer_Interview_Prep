# Week 5 — Interview Questions: Sequence Models & the Road to Transformers

> Click a question (▸) to expand the answer.

---

## Sequence Models

<details>
<summary>1. How does an RNN work; why struggle with long sequences?</summary>

It processes tokens one at a time, carrying a hidden state. Long sequences cause vanishing/exploding gradients through many time steps, so it forgets distant context and can't parallelize over time.
</details>

<details>
<summary>2. Formal reason RNNs vanish over time?</summary>

Backprop through time multiplies the recurrent Jacobian repeatedly; if its spectral radius <1 gradients shrink exponentially (>1 they explode), so early-step gradients vanish.
</details>

<details>
<summary>3. LSTM gates and cell state?</summary>

The cell state is a gradient highway. Forget gate drops old info, input gate writes new info, output gate exposes state — additive updates preserve gradients over long ranges.
</details>

<details>
<summary>4. GRU vs LSTM tradeoff?</summary>

GRU merges gates (reset/update) and drops the separate cell state — fewer parameters, faster, often comparable accuracy, but slightly less expressive on some long-range tasks.
</details>

<details>
<summary>5. What is a seq2seq (encoder-decoder) model?</summary>

An encoder compresses the input into a representation; a decoder generates the output from it — used for translation and summarization.
</details>

<details>
<summary>6. Original Bahdanau attention — what bottleneck did it remove?</summary>

Fixed-size context vectors lost information for long inputs. Attention lets the decoder look back at all encoder states with learned weights (soft alignment), removing the bottleneck.
</details>

<details>
<summary>7. Why did attention replace recurrence?</summary>

It models any-to-any dependencies in one step (no long gradient chains), parallelizes over the sequence (fast on GPUs), and scales — enabling transformers.
</details>

## Tokenization

<details>
<summary>8. Why tokenize; char vs word vs subword?</summary>

Models need discrete units. Char: tiny vocab, long sequences. Word: huge vocab, out-of-vocabulary problem. Subword (BPE/WordPiece): balances vocab size and coverage, handling rare/unknown words gracefully.
</details>

<details>
<summary>9. Walk through the BPE algorithm.</summary>

Start from characters; repeatedly find the most frequent adjacent pair and merge it into a new token; repeat until the target vocab size. Rare words split into known subwords.
</details>

<details>
<summary>10. WordPiece vs BPE; SentencePiece?</summary>

WordPiece merges the pair that most increases corpus likelihood (not just frequency). SentencePiece treats text as raw bytes/unicode (no pre-tokenization), so it's language-agnostic and reversible.
</details>

<details>
<summary>11. How does tiktoken work; vocab size tradeoff?</summary>

A fast byte-level BPE tokenizer. Larger vocab = shorter sequences (cheaper attention) but a bigger embedding table and rarer tokens; smaller vocab = opposite.
</details>

<details>
<summary>12. Tokenization failure modes?</summary>

Poor handling of non-English scripts, code whitespace/indentation, numbers (split oddly), and emojis — causing longer sequences and degraded performance.
</details>

## Embeddings

<details>
<summary>13. How does Word2Vec (CBOW vs Skip-gram) learn?</summary>

CBOW predicts a word from its context; Skip-gram predicts context from a word. Training pushes co-occurring words' vectors together, yielding semantic geometry.
</details>

<details>
<summary>14. What does an embedding represent geometrically?</summary>

A point in a dense space where distance/direction encodes meaning; similar items are near, and directions can capture relations (king−man+woman≈queen).
</details>

<details>
<summary>15. Word2Vec/GloVe vs contextual embeddings?</summary>

Word2Vec/GloVe give one static vector per word; contextual (BERT/GPT) embeddings depend on the sentence, so "bank" differs by context.
</details>

<details>
<summary>16. What is positional encoding; sinusoidal vs learned?</summary>

Attention is order-agnostic, so we inject position. Sinusoidal uses fixed sine/cosine patterns (extrapolates to longer lengths); learned uses trainable position vectors (flexible but fixed max length).
</details>

<details>
<summary>17. Why is the embedding layer a "lookup table"?</summary>

Each token id indexes a row in a trainable matrix — a one-hot × matrix = row selection, i.e., a lookup.
</details>

## Coding

<details>
<summary>18. Implement a BPE tokenizer from scratch.</summary>

Count adjacent symbol-pair frequencies, merge the most frequent into a new symbol, record the merge rule, and repeat to the target vocab; apply learned merges at encode time.
</details>

<details>
<summary>19. Build a char-level LM (RNN then LSTM) and compare.</summary>

Train each to predict the next character; the LSTM handles longer dependencies better, giving lower loss and more coherent samples on long sequences.
</details>

<details>
<summary>20. Visualize attention weights in a seq2seq model.</summary>

Plot the decoder-to-encoder attention matrix as a heatmap to see soft alignments (e.g., which source words each output word attends to).
</details>

## Failure / Judgment

<details>
<summary>21. NLP system broke due to tokenization (non-English)?</summary>

The tokenizer over-splits non-Latin scripts, inflating token counts, truncating context, and degrading quality/cost. Fix with a multilingual/byte-level tokenizer trained on representative data.
</details>

<details>
<summary>22. "Character-level or BPE tokenization for code?"</summary>

BPE (code-aware) usually — it keeps common identifiers/keywords as single tokens, shortening sequences. Tradeoff: char-level generalizes to any token but makes sequences long; rejected it for cost/length reasons unless modeling rare symbols.
</details>
