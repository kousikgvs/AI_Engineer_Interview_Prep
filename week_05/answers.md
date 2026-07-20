# Week 5 — Answers with Code: Sequence Models & the Road to Transformers

> Answers with code. Deep dives on attention, BPE, and tokenization.

---

## Sequence Models

### 1. RNN & long-sequence struggle (detailed)
```python
# h_t = tanh(W_h h_{t-1} + W_x x_t)
h = np.zeros(hidden)
for x in sequence:
    h = np.tanh(Wh @ h + Wx @ x)   # sequential -> can't parallelize over time
```
Repeated multiplication vanishes/explodes gradients over long ranges.

### 2. Vanishing gradient formal (tricky)
BPTT multiplies the recurrent Jacobian repeatedly; spectral radius <1 → exponential shrink, >1 → explode.

### 3. LSTM gates (detailed)
```python
# gates: forget f, input i, output o; cell state c is the gradient highway
f = sigmoid(Wf @ concat(h, x)); i = sigmoid(Wi @ ...); o = sigmoid(Wo @ ...)
g = tanh(Wg @ ...)
c = f * c + i * g          # additive update preserves gradients
h = o * tanh(c)
```

### 4. GRU vs LSTM (easy)
GRU merges gates, drops cell state — fewer params, faster, slightly less expressive.

### 5. seq2seq (easy)
Encoder compresses input; decoder generates output (translation/summarization).

### 6. Bahdanau attention (detailed)
Fixed context vectors lost long-range info; attention lets the decoder weight all encoder states (soft alignment).

### 7. Why attention replaced recurrence (detailed)
Any-to-any dependencies in one step (no long gradient chains), parallel over the sequence, scalable.

---

## Tokenization

### 8. Why tokenize / levels (easy)
Char (small vocab, long seq), word (OOV problem), subword (balanced).

### 9. BPE algorithm (detailed + code)
```python
from collections import Counter
def bpe_merges(words, num_merges):
    # words: list of space-separated char sequences e.g. "l o w </w>"
    vocab = Counter(words)
    for _ in range(num_merges):
        pairs = Counter()
        for w, freq in vocab.items():
            sym = w.split()
            for a, b in zip(sym, sym[1:]):
                pairs[(a, b)] += freq
        if not pairs: break
        best = max(pairs, key=pairs.get)          # most frequent pair
        merged = {w.replace(" ".join(best), "".join(best)): f
                  for w, f in vocab.items()}
        vocab = Counter(merged)
    return vocab
```

### 10. WordPiece vs BPE; SentencePiece (detailed)
WordPiece merges the pair that most increases corpus likelihood (not just frequency). SentencePiece works on raw unicode/bytes (language-agnostic, reversible).

### 11. tiktoken / vocab size (easy)
Byte-level BPE; larger vocab → shorter sequences but bigger embedding table.

### 12. Tokenization failures (easy)
Non-English scripts, code whitespace, numbers, emoji → longer sequences, degraded quality.

---

## Embeddings

### 13. Word2Vec (detailed)
CBOW predicts word from context; Skip-gram predicts context from word; co-occurring words end up near each other.

### 14. What embeddings represent (easy)
Points where distance/direction encodes meaning (king−man+woman≈queen).

### 15. Static vs contextual (easy)
Word2Vec/GloVe: one vector per word; BERT/GPT: context-dependent.

### 16. Positional encoding (detailed + code)
```python
def sinusoidal(seq_len, d):
    pos = np.arange(seq_len)[:, None]
    i = np.arange(d)[None, :]
    angle = pos / np.power(10000, (2*(i//2))/d)
    pe = np.zeros((seq_len, d))
    pe[:, 0::2] = np.sin(angle[:, 0::2]); pe[:, 1::2] = np.cos(angle[:, 1::2])
    return pe
```
Sinusoidal extrapolates to longer lengths; learned is flexible but bounded.

### 17. Embedding = lookup table (easy)
Token id indexes a row in a trainable matrix (one-hot × matrix).

---

## Coding

### 18. BPE tokenizer (see Q9).
### 19. Char-LM RNN vs LSTM: LSTM handles longer dependencies → lower loss, more coherent samples.
### 20. Visualize attention: heatmap of decoder→encoder attention weights.

---

## Failure / Judgment

### 21. Tokenization failure (non-English): over-splitting inflates tokens, truncates context. Fix: multilingual/byte-level tokenizer.
### 22. Char vs BPE for code: BPE (code-aware) keeps identifiers/keywords as single tokens; char-level makes sequences long/expensive.
