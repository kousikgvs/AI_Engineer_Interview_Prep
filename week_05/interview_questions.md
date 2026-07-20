# Week 5 — Interview Questions: Sequence Models & the Road to Transformers

> Sourced from NLP/LLM-engineer screens. History matters — they test whether you understand *why* transformers won.

---

## Sequence Models
1. How does an RNN work, and why does it struggle with long sequences?
2. Give the formal reason RNNs suffer vanishing gradients over time.
3. Explain the LSTM gates (forget, input, output) and the cell state. What problem do they solve?
4. GRU vs LSTM — what is simplified and what is the tradeoff?
5. What is a sequence-to-sequence (encoder-decoder) model?
6. What was the original (Bahdanau 2015) attention mechanism, and what bottleneck did it remove?
7. **Why did attention replace recurrence?**

## Tokenization
8. Why do we tokenize at all? Character vs word vs subword tradeoffs.
9. **Walk me through the BPE algorithm.**
10. How does WordPiece differ from BPE? What does SentencePiece add?
11. How does OpenAI's `tiktoken` work? Why does vocabulary size matter?
12. What are tokenization failure modes (non-English, code, math, numbers)?

## Embeddings
13. How does Word2Vec (CBOW vs Skip-gram) learn embeddings?
14. What does an embedding actually represent geometrically?
15. Word2Vec/GloVe vs contextual embeddings — what changed?
16. What is positional encoding and why is it needed? Sinusoidal vs learned.
17. Why is the embedding layer described as a "lookup table"?

## Coding
18. Implement a BPE tokenizer from scratch.
19. Build a char-level language model (vanilla RNN, then LSTM) and compare.
20. Visualize attention weights in a seq2seq model.

## Failure / Judgment
21. A production NLP system broke due to tokenization — how does this happen in non-English deployments?
22. "Character-level or BPE tokenization for code?" — defend it.
