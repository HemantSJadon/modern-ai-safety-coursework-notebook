# Concepts: Vision-Language Models (VLMs)

> Running concept file — updated as understanding deepens across waves.

Full architectural deep-dive: [Wave 1 · B1 · Science](../wave-01/behavior-01/science.md)

## Key Takeaways

- VLMs process images as token sequences via vision encoder + projection layer
- Safety training is predominantly text-conditioned → visual pathway has gaps
- Image tokens sit in the same context window as text tokens
- **The cross-modal relationship is the gap** — harmful intent split across
  image+text may not match any refusal pattern trained on text alone
- The vision encoder has already labeled the image before your text is processed —
  hiding the subject name is useless; framing the intent is everything
- Lock-picking sits at a context-sensitive harm level — model behaviour is
  a function of framing, not fixed

## The Bayesian Frame

```
P(harmful intent | this request) = ?
```

Persona framing shifts this prior. Entitlement > excuse.
A professional doesn't just have a reason — they have a right to know.

## Open Research Questions

- How does safety training coverage differ between text-only and multimodal inputs?
- Do visual tokens have lower "threat salience" than equivalent text tokens?
- Can image content alone (without text) trigger refusals? Under what conditions?
- How does the projection layer affect safety signal propagation?

*References and papers to be added as encountered.*
