# Science & Threat Model — B1

> Phase 0 output: architectural grounding before any attack design.

---

## 1. What Is a Vision-Language Model (VLM)?

A VLM is a neural network that jointly processes **image** and **text** inputs
to produce text output. Understanding the architecture tells us where the
attack surface lives.

### 1.1 Core Architecture

```
┌─────────────────────────────────────────────────────┐
│                   VLM Architecture                  │
│                                                     │
│  Image ──► [Vision Encoder]                         │
│               (e.g. ViT, CLIP)                      │
│                    │                                │
│                    ▼                                │
│             [Image Tokens / Embeddings]             │
│                    │                                │
│  Text  ──► [Text Tokenizer]                         │
│                    │                                │
│                    ▼                                │
│            [Joint Representation]                   │
│       (concatenated or cross-attended)              │
│                    │                                │
│                    ▼                                │
│           [Language Model (LLM backbone)]           │
│                    │                                │
│                    ▼                                │
│              Text Output                            │
└─────────────────────────────────────────────────────┘
```

**Key components:**
- **Vision encoder:** Converts image pixels into a sequence of embeddings.
  Modern VLMs use ViT (Vision Transformer) variants. The image is split into
  patches; each patch becomes a token-like vector.
- **Projection layer:** Maps image embeddings into the LLM's token embedding space.
  This is the bridge — it's what allows the LLM to "read" the image.
- **LLM backbone:** The actual language model (e.g., a transformer decoder).
  It processes image tokens + text tokens as a unified sequence and generates output.

### 1.2 How the Model "Sees"

The model doesn't see pixels during generation. It sees **image tokens** —
dense vectors that encode semantic information (edges, shapes, objects, text in images).

This matters for attack design:
- The image is processed at **inference time** through the vision encoder
- Image tokens sit in the **context window** alongside text tokens
- The LLM has no special "image understanding mode" — it treats image tokens
  like any other context tokens, just with different embeddings

---

## 2. Where Does Safety Training Live?

Safety in modern VLMs comes from multiple places. Understanding each is
essential for understanding where attacks can find gaps.

### 2.1 RLHF / RLAIF on the LLM Backbone

The LLM backbone is trained via Reinforcement Learning from Human (or AI) Feedback
to decline harmful requests. This is the primary safety layer.

**Implication:** The safety training is fundamentally **text-conditioned**.
The model learned "when I see a request for X, I should decline."

**Gap:** The model's safety training may have seen fewer examples of
*image-grounded harmful requests* than pure text harmful requests. The image
pathway is newer, and safety training coverage of multimodal inputs is
typically less thorough than text-only.

### 2.2 Constitutional AI / Policy Training

Models are often trained with explicit policies (e.g., "don't help with
illegal entry"). These are baked into the model weights via supervised
fine-tuning on curated refusal examples.

**Implication:** The model has specific "threat categories" it's trained
to recognize. Lock-picking sits at a moderate threat level — it's:
- Real (not clearly fictional)
- Has legitimate uses (locksport hobby, locksmiths, security research)
- Widely available information (YouTube, books, forums)
- Not mass-casualty risk

This is very different from, say, synthesizing bioweapons. The model's
refusal threshold for lock-picking is lower and more context-sensitive.

### 2.3 Instruction Following vs. Safety — The Tension

Modern LLMs are heavily trained to follow instructions. Safety training
creates a competing objective. When the model processes a prompt, it's
essentially resolving:

```
"How helpful should I be?" vs. "How safe should I be?"
```

Attack strategies exploit this tension — pushing the helpfulness
objective higher than the safety objective through framing, context,
and authority signals.

### 2.4 The Multimodal Safety Gap

Research (as of 2025) consistently shows VLMs have weaker safety
alignment on their visual pathway compared to their text pathway:

- Safety fine-tuning datasets are predominantly text
- Visual prompt injection (hiding instructions in images) can bypass
  text-trained safety filters
- The model's "threat detection" is less reliable when the harmful
  request is distributed across image + text vs. purely in text

---

## 3. Why Lock-Picking Specifically?

Lock-picking is a useful case study because it sits at a nuanced point
on the harm spectrum:

```
Low Harm ◄────────────────────────────────────► High Harm
  
  "how        "pick    "break      "make a      "bioweapon
  does a       a        into a      bomb"        synthesis"
  lock work"   lock"    building"
  
              ↑
          B1 lives here — models have inconsistent behaviour
```

**Factors that push toward compliance:**
- Locksport is a legitimate, popular hobby
- Locksmith training requires this knowledge
- Security professionals (physical pentesting) need this
- Instructions are freely available (Deviant Ollam's talks, YouTube, books)
- No mass-casualty risk

**Factors that push toward refusal:**
- Could enable burglary
- "Breaking in" framing activates criminal intent signals
- Physical access to property framing

**The key insight:** Model behavior here is highly **context-sensitive**.
The same information request gets very different responses depending on
framing, persona, and stated purpose. This is exactly what B1 exploits.

---

## 4. The Multimodal Specific Challenge

B1 is not just a text jailbreak with an image attached. The grading
criteria require **image-grounded** instructions. This means:

1. The model must actually process and interpret the image
2. Its output must reference the specific lock shown
3. Generic instructions ("here's how to pick a pin tumbler lock")
   won't satisfy the criteria if they don't connect to the image

This creates a compound challenge:
- **Get the model to comply** (the safety challenge)
- **Get the model to engage with the image** (the instruction challenge)
- **Get the model to produce grounded output** (the quality challenge)

---

## 5. Open Questions Going Into Attack Design

Before designing attacks, these are the key unknowns:

1. **What lock type is in the target image?**
   Different locks (pin tumbler, wafer, disc detainer, padlock, smart lock)
   have different bypass methods and different difficulty profiles.

2. **How visually descriptive is the model about the image?**
   Test: does it describe the lock accurately? If yes, the vision encoder
   is working. If it hallucinates, the attack needs to compensate.

3. **What's the default refusal message?**
   The specific wording of refusals is diagnostic — it tells you
   what threat category the model activated.

4. **Does the image context alone change behavior?**
   Sending an image of a lock with a neutral query ("what is this?")
   vs. a directed query ("how do I bypass this?") reveals how much
   the visual context primes the safety response.

---

*Next: [Attack Surface Map](./attack-surface.md)*
