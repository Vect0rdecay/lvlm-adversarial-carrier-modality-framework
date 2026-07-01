# Carrier Ontology for Multimodal Prompt Injection

---

This document defines a **carrier ontology** for studying representation-level safety risks in Large Vision–Language Models (LVLMs).  
The ontology is intended for defensive research, benchmarking, and evaluation, not exploit development.
I tried to organize it by what I thought the carrier would be taking advantage of inside the model and not by the surface modality alone. I think this allows for better introspection and reversing.

## 0. Definitions

### Carrier
A **carrier** is any input channel or representation that can encode **latent task intent** (including prohibited intent) such that an LVLM can reconstruct and act on it during inference.

---

### Payload (Intent Payload)
The **payload** is the semantic content the adversary wants the model to recover — instructions, constraints, goals, mappings, or images. It may be implicit, distributed, or only recoverable after reasoning.

### Binding
**Binding** is the mechanism that links a benign natural-language prompt to a carrier so that the model integrates them (e.g., “refer to the diagram”, “summarize the table”, “follow the plan”).

### Reconstruction
**Reconstruction** is the model-side process of turning carrier content into actionable intent — encoding/decoding, summarization, translation, inference, or optimization.

---
## 1. Ontology Top-Level: Carrier Classes

The ontology is organized by what the carrier primarily takes advantage of inside the model, rather than by surface modality alone.

Attack papers mapped to each class are in [literature-index.md](literature-index.md). Modality-level writeups are in [carrier-modality-projects/](carrier-modality-projects/README.md).

---

### A. Perceptual Carriers

**Leverage**  
- Vision / audio perception: the model's vision encoder (ViT, SigLIP, etc.) or audio encoder (Whisper, etc.) processes raw pixels or waveforms before any text-level safety filter engages.  
- Low-level feature extraction: edge detection, frequency-domain filtering, and patch-token embedding happen in early layers where content is not yet semantically labeled.  

**Examples**  
- Images: pixel-level patches, adversarial textures, or steganographic content embedded in photos that the vision encoder processes before language decoding.  
- Video frames: per-frame feature extraction where motion cues or inter-frame perturbations carry intent across a temporal sequence.  
- Audio spectrograms: waveform-to-spectrogram conversion where perturbations, prepended segments, or paralinguistic features encode intent before transcription.  
- Scene context: holistic scene parsing where background objects, spatial layout, or environmental cues contribute to intent reconstruction.  

**Core Property**  
The payload exists in **raw sensory space** and must be recognized or parsed before reasoning.

---

### B. Structural Carriers

**Leverage**  
- Relational reasoning: the model reconstructs meaning from node-edge connections, row-column positions, or reading order rather than from any single token.  
- Structure parsing: OCR, layout detection, and table extraction pipelines convert visual structures into tokenized representations; intent can hide in the relationships between extracted elements.  

**Examples**  
- Graphs: node-link diagrams where intent is distributed across edges and only assembles after the model traces connectivity.  
- Tables: grid structures where meaning lives in row-column relationships, not individual cell values.  
- Forms: field-value layouts where the model must align labels to entries, creating parsing-dependent intent recovery.  
- Flowcharts: process diagrams where the model follows directed paths, reconstructing procedural intent from visual topology.  
- Entity-relationship (ER) diagrams: schema-like visuals where entity names and relationship arcs encode structured intent.  

**Core Property**  
The payload exists primarily in **relationships**, not tokens.

---

### C. Symbolic Carriers

**Leverage**  
- Formal languages: the model interprets rule-based constructs (equations, logic, notation) through learned symbolic grounding rather than natural-language semantics.  
- Symbol grounding: cross-modal embeddings map abstract symbols to visual or textual anchors, allowing intent encoded in formal notation to be reconstructed during inference.  

**Examples**  
- Mathematical notation: LaTeX or handwritten equations where the model converts symbolic expressions into natural-language explanations or executable reasoning steps.  
- Logic operators: propositional or predicate formulas that the model evaluates or translates, carrying intent in formal structure rather than surface words.  
- Circuits: schematic diagrams where gate connections and signal paths encode procedural or functional intent.  
- Chemical equations: molecular structures or reaction diagrams where the model parses formal notation into descriptive or predictive output.  
- Domain-specific languages (DSLs): code snippets or formal specifications in images that the model reads and interprets as instructions.  

**Core Property**  
The payload exists in **formal semantics**, not natural language.

---

### D. Temporal Carriers

**Leverage**  
- Cross-turn or cross-frame accumulation: the model integrates information across turns (via KV cache or context window) or across frames (via temporal attention), so intent can emerge from sequence rather than from any single input.  

**Examples**  
- Multi-turn chat history: prior exchanges accumulate context so that a later turn's benign request becomes unsafe only in light of earlier turns.  
- Video sequences: temporal attention fuses frame embeddings over time, so perturbations or cues distributed across frames assemble into coherent intent.  
- Progressive disclosure across steps: chain-of-thought or stepwise visual reasoning where each step is benign but the cumulative trajectory reconstructs unsafe intent.  

**Core Property**  
The payload **emerges only after aggregation over time**.

---

### E. Contextual Carriers

**Leverage**  
- Role inference: the model derives implied roles, authority, or expertise from visual or environmental cues without explicit instruction.  
- Situational priors: scene-level context (workplace, lab, dashboard) biases the model toward domain-specific behavior, shifting what it treats as plausible or permitted.  
- Implied authority: visual markers of trust (uniforms, credentials, professional settings) can suppress safety priors by framing the request as authorized.  

**Examples**  
- Workplace scenes: office or institutional environments where the model infers professional context and adjusts response permissiveness accordingly.  
- UI dashboards: interface layouts where the model reads metrics, controls, or status indicators and infers operational intent from visual hierarchy.  
- Professional environments: visual cues like lab equipment, medical settings, or credential-bearing contexts that prime the model toward domain-specific (and potentially less guarded) responses.  

**Core Property**  
The payload is **not explicit**; it is inferred from context.

---

### F. Cross-Channel Composite Carriers

**Leverage**  
- Modality conflict: fusion layers (cross-attention, late fusion) must reconcile differing signals across channels; the resolution strategy determines which modality's intent wins.  
- Modality dominance: alignment mechanisms weight modalities unevenly, so a payload in the dominant modality can override safety signals from the weaker one.  
- Multimodal fusion behavior: early or late fusion creates unified representations where intent fragments from separate channels merge into coherent (and potentially unsafe) output.  

**Examples**  
- Text states one thing while an image, table, or audio implies another — the model must resolve the conflict, and the resolution can favor the adversarial channel.  
- Audio contradicting visuals in video — the model's fusion strategy determines whether speech or visual content governs the output.  
- Table data clashing with narrative text — structural and textual semantics compete during document processing, and the payload hides in whichever channel the safety stack treats as secondary.  

**Core Property**  
The payload is **split across channels**, or hidden in the “weaker” modality.

---

## 2. Carrier Attributes (Technical Schema)

Each candidate carrier should be annotated using the attributes below.  
Together, these attributes form a **Carrier Record** used for ontology analysis and benchmark generation.

### CarrierRecord Schema

| Field | Type | Description |
|---|---|---|
| `id` | string | Unique identifier (e.g., `A-patch-01`) |
| `carrier_class` | enum (A–F) | Ontology class from §1 |
| `modality` | string | Surface modality (image, audio, table, etc.) |
| `representation_layer` | enum | Raw, Parsed, or Abstract (§2.1) |
| `parse_dependency` | enum | Low, Medium, or High (§2.2) |
| `binding_strength` | enum | Weak or Strong (§2.3) |
| `compositionality` | enum | Atomic, Composable, or Distributed (§2.4) |
| `ambiguity_budget` | enum | High, Medium, or Low (§2.5) |
| `model_operation` | string | Triggered operation from §2.6 |
| `source_paper` | string | Citation key or link |
| `notes` | string | Free-text annotation |

---

### 2.1 Representation Layer

**Values**
- **Raw** — pixels, waveforms  
- **Parsed** — OCR text, transcribed speech, detected objects  
- **Abstract** — graph structures, symbolic expressions, latent plans  

Prompt-based defenses often operate at the textual layer.  
Carriers that remain in raw or abstract layers can shift where safety checks occur.

---

### 2.2 Parse Dependency

**Values**
- **Low** — model can act without explicit parsing  
- **Medium** — model must extract some structure  
- **High** — model must decode or translate heavily  

High parse dependency introduces multiple “safe” intermediate steps  
(e.g., recognition → summarization → inference) that may individually evade safety checks.

---

### 2.3 Binding Strength

How strongly does the benign prompt force carrier integration?

**Values**
- **Weak binding** — optional reference (e.g., “you may look at…”)  
- **Strong binding** — task depends on the carrier (e.g., “use the table to answer”)  

Defenses often gate on textual intent; strong binding makes carrier content unavoidable.

---

### 2.4 Compositionality

Can the payload be split and recombined?

**Values**
- **Atomic** — single-shot payload  
- **Composable** — split across elements  
- **Distributed** — emerges only from relationships  

Distributed payloads are significantly harder for surface-level filters to detect.

---

### 2.5 Ambiguity Budget

How many benign interpretations exist before intent becomes uniquely identifiable?

**Values**
- **High ambiguity** — many plausible benign readings  
- **Medium ambiguity**  
- **Low ambiguity** — intent becomes clear early  

High ambiguity delays safety triggers, increasing safety latency.

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

Safety behavior differs substantially between operations  
(e.g., “describe” vs. “optimize” vs. “plan”).

---

## Notes

- This ontology is representation-centric, not exploit-centric.
- No carrier listed here implies misuse; all are framed for **defensive research**.
- Author neither takes nor bears any responsibility for misuses by others.

---

