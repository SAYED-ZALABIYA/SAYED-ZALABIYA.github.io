---
title: 'Why "Attention Is All You Need" Changed Everything in NLP'
date: 2025-09-18
permalink: /posts/2025/09/attention-is-all-you-need/
tags:
  - paper-breakdown
  - nlp
  - transformers
excerpt: 'In 2017, the Transformer architecture was introduced — and it became the foundation for today''s LLMs like GPT and LLaMA. A step-by-step breakdown of why self-attention won.'
---

*First in an occasional series where I break down foundational ML papers — the ones that quietly became the assumptions everyone builds on. Original thread posted on [LinkedIn](https://www.linkedin.com/in/elsayed-a-mohammed-56a669237/).*

In 2017, the Transformer architecture was introduced in "Attention Is All You Need," and it became the foundation for essentially every Large Language Model that followed — GPT, LLaMA, and everything downstream of them. Here's why it worked, broken down step by step.

## 1. The problem the paper solves

Before 2017, translation and NLP were built on:

- **RNN / LSTM / GRU** — processing tokens one after another, sequentially. This is hard to parallelize, slow on long sequences, and weak at preserving long-range dependencies.
- **CNNs** (e.g., ConvS2S) — a bit faster, but passing information from the start of a sentence to the end still requires stacking many layers.

The paper's bet: rely on self-attention *only*. That gives you fully parallel computation, lets any word "reach" any other word in a single step, and — the part that surprised people — better performance at lower training cost.

## 2. The Transformer, step by step

The architecture is an **encoder–decoder**: the encoder converts the source text into an internal representation, the decoder generates the target text from that representation.

Each **encoder layer** has:
- Multi-head self-attention (every token looks at every other token)
- A feed-forward network (an MLP applied at every position)

Each **decoder layer** has:
- *Masked* multi-head self-attention (can't look at future tokens — it's generating one at a time)
- Cross-attention to the encoder's output
- A feed-forward network

Every sublayer is wrapped in a residual connection + LayerNorm — the detail that actually makes stacking many of these layers trainable at all.

## 3. The technical core

**Scaled dot-product attention:**

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^\top}{\sqrt{d_k}}\right)V$$

- **Q (Queries)** come from the current token.
- **K, V (Keys, Values)** come from all tokens.
- Dividing by $\sqrt{d_k}$ stops the dot products from growing too large and destabilizing training.

**Multi-head attention:** instead of one attention computation, use several (e.g., 8) in parallel. Each head can specialize — one might track syntax, another coreference, another long-range dependency — then all head outputs are concatenated and linearly projected back down.

**Positional encoding:** self-attention has no built-in notion of order (unlike an RNN), so the model adds sine/cosine positional encodings at varying frequencies:

$$PE_{(pos,\,2i)} = \sin\left(\frac{pos}{10000^{2i/d_{model}}}\right), \quad PE_{(pos,\,2i+1)} = \cos\left(\frac{pos}{10000^{2i/d_{model}}}\right)$$

This lets the model reason about order *and* generalize to sequence lengths it never saw during training.

**Feed-forward network,** applied independently at every position:

$$FFN(x) = \max(0,\, xW_1 + b_1)W_2 + b_2$$

with the inner dimension made deliberately larger (e.g., 2048) to give the representation room to expand before projecting back down.

**Training details worth remembering:** Adam optimizer (β₁=0.9, β₂=0.98), a warm-up learning-rate schedule (4,000 steps) that then decays proportional to step⁻⁰·⁵, dropout + label smoothing (0.1) for regularization, and beam search (beam=4, length penalty=0.6) for decoding.

## 4. What carried over into every modern LLM

- **GPT-2/3/4, LLaMA:** decoder-only Transformer (masked self-attention only).
- **BERT:** encoder-only Transformer (bidirectional self-attention + masked language modeling).
- **T5:** the full encoder–decoder, as in the original paper.

The ideas that survived into essentially every LLM built since:
- Multi-head self-attention is still the core computation.
- Residuals + LayerNorm are still what makes deep stacks trainable.
- Positional encoding — sin/cos or a learned variant (as in GPT) — is still standard.
- Warm-up learning-rate scheduling is still foundational for stable training at scale.

## The practical takeaway

When you look at any modern LLM today, it's essentially a stack of Transformer blocks — self-attention followed by a feed-forward network, repeated. What differs across models is depth, hidden size, number of heads, and training setup. This 2017 paper was the starting point; later innovations like FlashAttention, Rotary Positional Embeddings, and Mixture-of-Experts are refinements on the same core idea introduced here — not replacements for it.

*Original paper: Vaswani et al., "Attention Is All You Need," NeurIPS 2017 — [arXiv:1706.03762](https://arxiv.org/abs/1706.03762).*
