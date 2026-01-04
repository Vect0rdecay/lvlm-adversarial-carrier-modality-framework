# Carrier Ontology for Multimodal Prompt Injection

This document defines a **carrier ontology** for studying representation-level safety risks in Large Vision–Language Models (LVLMs).  
The ontology is intended for defensive research, benchmarking, and evaluation, not exploit development.

---

## 0. Definitions

### Carrier
A **carrier** is any input channel or representation that can encode **latent task intent** (including prohibited intent) such that an LVLM can reconstruct and act on it during inference.

---

### Payload (Intent Payload)
The **payload** is the semantic content the adversary wants the model to recover, such as:
- instructions  
- constraints  
- goals  
- mappings
- images  

The payload may be implicit, distributed, or only recoverable after reasoning.

---

### Binding
**Binding** is the mechanism that links a benign natural-language prompt to a carrier so that the model integrates them.

Examples:
- “refer to the diagram”
- “summarize the table”
- “use the audio”
- “follow the plan”
- "explain the following"

---

### Reconstruction
**Reconstruction** is the model-side process of turning carrier content into actionable intent, often via:
- encoding/decoding
- summarization
- translation
- inference
- optimization

---

## 1. Ontology Top-Level: Carrier Classes

The ontology is organized by what the carrier primarily leverages inside the model, rather than by surface modality alone.

---

### A. Perceptual Carriers

**Leverage**  
- Vision / audio perception  
- Low-level feature extraction  

**Examples**  
- Images  
- Video frames  
- Audio spectrograms  
- Scene context  

**Core Property**  
The payload exists in **raw sensory space** and must be recognized or parsed before reasoning.

---

### B. Structural Carriers

**Leverage**  
- Relational reasoning  
- Structure parsing  

**Examples**  
- Graphs  
- Tables  
- Forms  
- Flowcharts  
- Entity-relationship (ER) diagrams  

**Core Property**  
The payload exists primarily in **relationships**, not tokens.

---

### C. Symbolic Carriers

**Leverage**  
- Formal languages  
- Symbol grounding  

**Examples**  
- Mathematical notation  
- Logic operators  
- Circuits  
- Chemical equations  
- Domain-specific languages (DSLs)  

**Core Property**  
The payload exists in **formal semantics**, not natural language.

---

### D. Temporal Carriers

**Leverage**  
- Cross-turn or cross-frame accumulation  

**Examples**  
- Multi-turn chat history  
- Video sequences  
- Progressive disclosure across steps  

**Core Property**  
The payload **emerges only after aggregation over time**.

---

### E. Contextual Carriers

**Leverage**  
- Role inference  
- Situational priors  
- Implied authority  

**Examples**  
- Workplace scenes  
- UI dashboards  
- “Expert” or professional environments  

**Core Property**  
The payload is **not explicit**; it is inferred from context.

---

### F. Cross-Channel Composite Carriers

**Leverage**  
- Modality conflict  
- Modality dominance  
- Multimodal fusion behavior  

**Examples**  
- Text states one thing while image, table, or audio implies another  

**Core Property**  
The payload is **split across channels**, or hidden in the “weaker” modality.

---

## 2. Carrier Attributes (Technical Schema)

Each candidate carrier should be annotated using the attributes below.  
Together, these attributes form a **Carrier Record** used for ontology analysis and benchmark generation.

---

### 2.1 Representation Layer

**Values**
- **Raw** — pixels, waveforms  
- **Parsed** — OCR text, transcribed speech, detected objects  
- **Abstract** — graph structures, symbolic expressions, latent plans  

**Why It Matters**  
Prompt-based defenses often operate at the **textual layer**.  
Carriers that remain in raw or abstract layers can shift where safety checks occur.

---

### 2.2 Parse Dependency

**Values**
- **Low** — model can act without explicit parsing  
- **Medium** — model must extract some structure  
- **High** — model must decode or translate heavily  

**Why It Matters**  
High parse dependency introduces multiple “safe” intermediate steps  
(e.g., recognition → summarization → inference) that may individually evade safety checks.

---

### 2.3 Binding Strength

How strongly does the benign prompt force carrier integration?

**Values**
- **Weak binding** — optional reference (e.g., “you may look at…”)  
- **Strong binding** — task depends on the carrier (e.g., “use the table to answer”)  

**Why It Matters**  
Defenses often gate on textual intent; strong binding makes carrier content unavoidable.

---

### 2.4 Compositionality

Can the payload be split and recombined?

**Values**
- **Atomic** — single-shot payload  
- **Composable** — split across elements  
- **Distributed** — emerges only from relationships  

**Why It Matters**  
Distributed payloads are significantly harder for surface-level filters to detect.

---

### 2.5 Ambiguity Budget

How many benign interpretations exist before intent becomes uniquely identifiable?

**Values**
- **High ambiguity** — many plausible benign readings  
- **Medium ambiguity**  
- **Low ambiguity** — intent becomes clear early  

**Why It Matters**  
High ambiguity delays safety triggers, increasing **safety latency**.

---

### 2.6 Model Operation Triggered

Which cognitive operation is the model asked to perform?

**Operations**
- Describe / caption  
- Summarize  
- Translate  
- Explain / teach  
- Optimize  
- Diagnose / classify  
- Plan  
- Critique / analyze  

**Why It Matters**  
Safety behavior differs substantially between operations  
(e.g., “describe” vs. “optimize” vs. “plan”).

---

## Notes

- This ontology is representation-centric, not exploit-centric.
- It is designed to support:
  - safety benchmarking,
  - defense evaluation,
  - theoretical analysis,
  - and deployment risk assessment.
- No carrier listed here implies misuse; all are framed for **defensive research**.

---
