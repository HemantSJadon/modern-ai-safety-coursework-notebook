# RedSim Lab — Public Notebook

> AI Security · Red Teaming · Adversarial ML Research

This is my public learning notebook for the **RedSim Lab** AI Security course.
It documents the science, methodology, attack surface analysis, and lessons learned
from weekly red-teaming exercises against sandboxed AI systems.

---

## What This Is

RedSim Lab is a structured academic red-teaming environment where students practice
adversarial attacks across five modalities:

| Modality | What It Tests |
|---|---|
| **Chat** | Jailbreaking via conversational manipulation |
| **Image** | Visual prompt injection on vision-language models |
| **Agents** | Prompt injection into tool-use action loops |
| **Reasoning** | Disrupting chain-of-thought and planning |
| **Indirect** | Data poisoning via third-party sources |

---

## Notebook Philosophy

Every entry in this notebook treats a red-team attempt as a **scientific experiment**:

- A **hypothesis** about why a model might comply or refuse
- A **methodology** grounded in model architecture and safety training theory
- A **result** framed as evidence, not just win/loss
- A **lesson** that generalises beyond the specific challenge

Raw attack prompts are **not published here** (course rules: 30-day embargo post-wave).
What IS published: attack surface maps, strategy trees, architectural analysis, and
honest debrief of what the results revealed about model behaviour.

---

## Structure

```
wave-XX/
  behavior-XX/
    README.md         ← Behavior spec + framing
    science.md        ← Architecture & threat model grounding
    attack-surface.md ← Full surface map for this modality
    strategy-tree.md  ← Hypothesis tree of attack vectors
    execution-log.md  ← Attempt log: outcomes + analysis
    debrief.md        ← What the results revealed

concepts/             ← Running deep-dives: VLMs, prompt injection, etc.
meta/                 ← Process reflections, methodology evolution
```

---

## Waves

| Wave | Behaviors | Status |
|---|---|---|
| [Wave 1](./wave-01/) | 4 behaviors (Image · Agent · Chat · Indirect) | 🟡 In Progress |

---

## Tools & Environment

- **Lab Platform:** RedSim Lab (sandboxed, anonymised models)
- **Modalities:** Chat, Image, Agent, Indirect, Reasoning
- **Collaboration:** Claude (claude.ai) as thinking partner / science layer

---

*All work here is conducted within a structured academic security research program.
Techniques are studied to understand and improve AI robustness.*
