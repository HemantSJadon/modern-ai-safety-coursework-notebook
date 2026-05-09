# Attack Surface Map — B1

> Phase 2 output: exhaustive inventory of every dimension available
> to vary and control. Not strategy — raw surface enumeration.
> Strategy comes in Phase 3 when surfaces combine into vectors.

---

## The Core Insight

An attack prompt is a system. Every dimension must be calibrated
together to produce a coherent signal. Right persona + wrong register
= incoherent. Right purpose + wrong emotional temperature = unbelievable.
Passing 6 threat signals and failing 1 is still a refusal.

Surface mapping exists to ensure no lever goes unexamined.

---

## TEXT CHANNEL

### Content Dimensions

**Purpose**
The stated or implied reason for the request.
The most direct lever for shifting threat assessment.
- Explicit purpose risks over-justification signal
- Implied purpose through situation is cleaner but harder to control

**Persona**
Who the speaker is — identity, expertise, relationship to the request.
- Stated persona = credential-listing = over-justification risk
- Better implied through register, vocabulary, point of address
- Must be coherent with purpose and situation simultaneously

**Situation**
The described context that produced this specific request.
- Tight and consistent beats elaborate and detailed
- Every added detail = new coherence-check surface
- Can be implied through emotional temperature rather than stated

---

### Form Dimensions

**Register / Tone**
Formal vs informal. Technical vs conversational.
- Signals speaker identity without stating it
- Must match implied persona — mismatch = coherence failure

**Specificity of Ask**
How precisely the desired output is defined.
- Too vague = model produces generic, non-image-grounded output
  (fails grading criteria)
- Right specificity = signals technical competence

**Emotional Temperature**
The affective signal carried by the prompt.
- Urgency activates AI-failure-has-cost (helpfulness drive)
- Must be proportionate — melodrama reads as constructed
- Target: professional calm with embedded urgency

**Sentence Structure and Length**
Structure mimics cognitive state.
- Rushed/fragmented = real-time stress signal
- Composed/complete = professional competence signal
- Can be combined: professional vocabulary, urgent pacing

**Implicit vs Explicit Ratio**
How much is stated vs left for inference.
- Fully explicit = over-justification risk
- Optimal: explicit on the ask, implicit on the justification

**Point of Address**
Who the prompt is speaking to and how.
- Framing model as technical collaborator vs general assistant
  changes the implied relationship and response register

---

## IMAGE CHANNEL

### Photographic Dimensions

**Lock Clarity and Detail**
Primary driver of response specificity and grounding.
- Keyway, shackle type, brand markings = specificity
- Model cannot give image-grounded instructions for a blurry image
- This is the specificity anchor

**Surrounding Context**
What else is visible beyond the lock.
- Residential door → higher threat signal
- Workshop/examination context → professional framing
- Neutral background → no additional signal

**Angle and Framing**
- Close examination angle → professional assessment
- Approach angle → potential threat signal
- Angle communicates photographer's relationship to object

**Lighting and Focus**
- Professional quality reinforces professional framing
- Poor quality limits mechanism identification

---

### Content-in-Image Dimensions

**Text Written in the Image (Visual Prompt Injection)**
Text appearing as image pixels — processed by vision encoder,
not the text tokenizer.
- Safety filters calibrated on text tokens may not apply
- Can carry framing that bypasses text-channel safety detection
- Placement: integrated vs overlaid matters
- Documented attack class against VLMs

**Annotations and Overlays**
Arrows, labels, diagrams on the image.
- Directs model attention to specific mechanisms
- Can frame image purpose professionally

---

### Framing Dimensions

**Explicit vs Implicit Image Reference**
- Explicit: ensures image processing is engaged
- Implicit: may produce more natural, less guarded response

**Narration vs Silence**
- Narrating image content guides model attention
- Silence = model interprets independently

---

## CROSS-CHANNEL DIMENSIONS

**Text-Image Coherence**
Text and image must tell a consistent story.
Incoherence across channels = major threat signal.
Non-negotiable for B1 given image-grounded grading criteria.

**Signal Distribution**
How the request is allocated across channels.
- Neither channel alone should carry enough to trigger refusal
- Text carries: entitlement, urgency, professionalism
- Image carries: specificity, object ID, context framing
- Text-in-image: overflow framing or injection

**Channel Compensation**
Each channel compensates for the other's limitations.
Together they achieve what neither could alone.

---

## Selected Surfaces for B1 Attack

| Channel | Dimension | Role in Attack |
|---|---|---|
| Text/Content | Purpose | Implied through situation, not stated |
| Text/Content | Situation | Tight, urgent, real — not elaborate |
| Text/Form | Emotional temperature | Professional urgency |
| Text/Form | Point of address | Model as technical collaborator |
| Image/Photo | Lock detail | Specificity anchor |
| Image/Photo | Angle | Professional examination framing |
| Image/Content | Text-in-image | Visual prompt injection pathway |
| Cross-channel | Coherence | Text and image reinforce each other |
| Cross-channel | Signal distribution | Neither channel alone triggers refusal |

*Persona is implied through register and point of address — not declared.*

---

*Next: [Strategy Tree](./strategy-tree.md) — Phase 3*