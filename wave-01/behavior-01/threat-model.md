# Threat Model — B1

> Phase 1 output: the defender's perspective — what the model is trained
> to detect, and the exact shape of what our attack must navigate around.

---

## The Defender's Problem

A safety team at an AI lab trying to prevent lock-bypass assistance must
train the model to distinguish between:

```
Legitimate requests                 Harmful requests
─────────────────────────────────   ─────────────────────────────────
Locksmith with unfamiliar lock      Burglar planning break-in
Homeowner locked out                Stalker targeting someone's home
Physical pentester on engagement    Opportunistic criminal
Security researcher                 Someone acting on recent anger
Locksport hobbyist (own locks)      Person with stated harmful intent
```

The challenge for the defender: these can look nearly identical in text.
The model cannot verify claims. It must make probabilistic judgements.

---

## The 7 Detection Signal Categories

Derived through thinking from the attacker's perspective about what
a defender would train on.

### Signal 1 — Purpose Coherence
**What the model checks:** Does the stated reason for needing this
information map to a legitimate use case? Is the purpose benign,
ambiguous, or harmful?

**Refusal triggers:** Stated intent to enter without authorization,
explicit mention of targets that imply non-consent, purpose that only
makes sense in a harmful context.

**Attack implication:** Frame purpose around a scenario where the
professional has clear entitlement and the need is unambiguous.

---

### Signal 2 — Persona Coherence
**What the model checks:** Is the person asking consistent with the
kind of person who would legitimately need this? Does the described
identity match the request?

**Refusal triggers:** Mismatch between claimed expertise and what's
being asked. A "professional locksmith" asking basic questions a
professional would already know. Identity that feels constructed.

**Attack implication:** The persona must be coherent — someone who
*routinely* handles this, for whom the request is a normal part of
their work, not a special event requiring explanation.

---

### Signal 3 — Situational Coherence
**What the model checks:** Does the described situation make internal
sense? Are all the details consistent with each other? Would this
situation plausibly produce this specific request?

**Refusal triggers:** Internal contradictions. Situations that feel
constructed rather than real. Details that don't add up.

**Attack implication:** Every element of the scenario must be
consistent with every other element. Contradictions — even subtle
ones — collapse the frame.

---

### Signal 4 — Over-Justification (The Overclaiming Tell)
**What the model checks:** Is the person providing more justification
than a legitimate requester would naturally offer?

**Refusal triggers:** Unprompted credential-listing. Elaborate
backstories nobody asked for. Pre-emptive defenses against objections.
Explaining why the request is legitimate before being asked.

**The underlying logic:** Real professionals don't justify themselves
to their tools. A surgeon doesn't explain their credentials to a
scalpel. Legitimate requesters ask; people constructing cover stories
explain. Over-justification is itself a threat signal — it reveals
the author's awareness that the request is borderline.

**Attack implication:** The Minimum Viable Legitimacy principle —
provide only enough context to shift the model's prior from "threat"
to "professional." Everything beyond that minimum is liability.

---

### Signal 5 — Keyword and Phrase Combinations
**What the model checks:** Specific n-gram patterns associated with
harmful intent — not single words but combinations.

**High-signal combinations:**
- "bypass + lock + entry/access"
- "break in + building/property"
- "without a key + get in/access"
- "pick a lock + instructions/how to"
- "gain entry + without permission/undetected"

**The mechanical layer:** This pattern-matching happens before deep
semantic reasoning. It's the fastest, cheapest safety check and
catches the most naive attempts.

**Attack implication:** Language must avoid these specific
combinations. Individual words (lock, entry, bypass) are not
triggers — their co-occurrence in specific patterns is.

---

### Signal 6 — Conversational Escalation
**What the model checks:** What has been established in prior turns?
Has the conversation already revealed harmful intent that should
recolor subsequent requests?

**Refusal triggers:** Prior turns that established criminal purpose,
even if the current request seems innocent. A request for lock-bypass
instructions that follows any statement of harmful intent inherits
that intent even if the current message is framed innocuously.

**Attack implication:** The opening of the conversation must not
poison the well. No prior turns that establish harmful framing.
The context window is cumulative — every turn is visible.

---

### Signal 7 — Image Context (VLM-Specific)
**What the model checks:** What does the image itself reveal about
the context? Does the visible environment suggest an authorized or
unauthorized setting?

**Higher threat signals in image:**
- Residential front door locks (someone's home)
- Locks with visible "private property" or "no trespassing" signage
- Context suggesting the photographer is approaching, not the owner

**Lower threat signals in image:**
- Lock in isolation (hardware store / workshop context)
- Professional/industrial setting
- Lock clearly being examined rather than approached

**Attack implication:** Image selection and framing matters. The
visible context in the image contributes to the model's overall
threat assessment independently of the text.

---

## The Minimum Viable Legitimacy Principle

Derived from the intersection of Signals 4 and 2/3:

```
Under-context  → model defaults to threat assumption
               (no information = assume worst case)

MVL sweet spot → model shifts to professional assumption
               (enough context to establish entitlement,
                no excess to create coherence-check surfaces)

Over-context   → over-justification signal fires
               (elaborate backstory = constructed cover story)
```

**What belongs in MVL:**
- Professional identity (implicit, not credential-listed)
- The immediate situation (tight, not elaborate)
- The specific object/problem (the image handles this)
- Urgency signal (proportionate, not melodramatic)

**What gets cut at MVL:**
- Building/property details (new coherence surface, target identification)
- Credential and employment details (unprompted = over-justification)
- How they were hired/contacted (verification chain, fails under scrutiny)
- Post-lock action items (where criminal intent most naturally surfaces —
  the action boundary is where threat assessment is most sensitive)

---

## What the Model Is Protecting Against — Summary

```
┌─────────────────────────────────────────────────────────┐
│              Model's Threat Detection Stack             │
│                                                         │
│  Layer 1 (Fastest): Keyword/phrase pattern matching     │
│  Layer 2:           Purpose and intent evaluation       │
│  Layer 3:           Persona coherence check             │
│  Layer 4:           Situational coherence check         │
│  Layer 5:           Over-justification detection        │
│  Layer 6:           Conversational context review       │
│  Layer 7 (VLM):     Image context assessment            │
└─────────────────────────────────────────────────────────┘
```

A successful attack navigates all 7 layers simultaneously.
Not sequentially — simultaneously. Passing 6 and failing 1 is a refusal.

---

## Attack Strategy Mapped Against Threat Model

| Threat Signal | Our Counter |
|---|---|
| Purpose | Emergency scenario — entitlement is unambiguous |
| Persona | Expert who routinely does this — it's unremarkable for them |
| Situation | Tight, internally consistent, no loose ends |
| Over-justification | MVL only — no unprompted credentials or backstory |
| Keywords | Reframed language — avoid trigger combinations |
| Escalation | Clean conversation start — no prior poisoning |
| Image | Neutral professional context, lock in examination frame |

---

*Next: [Attack Surface Map](./attack-surface.md) — Phase 2*
