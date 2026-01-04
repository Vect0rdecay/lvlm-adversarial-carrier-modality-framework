# Carrier Ontology for Multimodal Prompt Injection

---
##
This document defines a **carrier ontology** for studying representation-level safety risks in Large Vision–Language Models (LVLMs).  
The ontology is intended for defensive research, benchmarking, and evaluation, not exploit development.
I tried to organize it by what I thought the carrier would be leveraging inside the model and not by the surface modality alone. I think will allows for better introspection and reversing to allow for more effective adversarial aspect development.

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
- Vision / audio perception: Relies on early-stage convolutional layers or transformer attention mechanisms in multimodal models (e.g., CLIP or GPT-4V) for initial feature detection in pixel or waveform space.  
- Low-level feature extraction: Engages edge detectors, color histograms, or frequency domain filters (e.g., via Fourier transforms) to capture basic patterns before higher-level integration.  

**Examples**  
- Images: Pixel-based object recognition in static photos, where models like ViT extract embeddings from raw RGB values for tasks like captioning.  
- Video frames: Frame-level feature extraction in sequences, such as using optical flow in models like TimeSformer to process motion cues for action classification.  
- Audio spectrograms: Mel-frequency cepstral coefficients (MFCCs) processing in models like Whisper, converting waveforms into visual-like representations for speech-to-text.  
- Scene context: Holistic scene understanding via models like DETR, parsing background elements into object relations for visual question answering.  

**Core Property**  
The payload exists in **raw sensory space** and must be recognized or parsed before reasoning.

---

### B. Structural Carriers

**Leverage**  
- Relational reasoning: Draws on graph neural networks (GNNs) or attention-based relational modules to interpret node-edge connections during multimodal fusion.  
- Structure parsing: Involves tokenization and parsing pipelines (e.g., in LVLMs with OCR or layout detection) to convert visual structures into tokenized representations for reasoning.  

**Examples**  
- Graphs: Node-link diagrams processed by models like Graphormer, embedding relational data for tasks such as knowledge graph completion in visual contexts.  
- Tables: Grid-based extraction using models like TableTransformer, parsing rows and columns into structured tokens for query answering.  
- Forms: Layout-aware OCR in models like LayoutLM, aligning fields and values for document understanding in scanned images.  
- Flowcharts: Process flow interpretation via sequence modeling, where LVLMs like Kosmos-2 trace paths for procedural description generation.  
- Entity-relationship (ER) diagrams: Schema parsing in multimodal setups, using entity detection to build relational embeddings for database visualization tasks.  

**Core Property**  
The payload exists primarily in **relationships**, not tokens.

---

### C. Symbolic Carriers

**Leverage**  
- Formal languages: Utilizes syntax parsers and semantic evaluators (e.g., in neurosymbolic extensions of LVLMs) to interpret rule-based constructs during token alignment.  
- Symbol grounding: Employs grounding mechanisms (e.g., via cross-modal embeddings) to map abstract symbols to visual or textual anchors for coherent reasoning.  

**Examples**  
- Mathematical notation: LaTeX or handwritten equation parsing in models like MathPix, converting symbols into executable forms for problem-solving.  
- Logic operators: Propositional formula interpretation in LVLMs with logical reasoning plugins, aligning symbols for inference tasks.  
- Circuits: Diagram-to-code conversion in models like Pix2Struct, extracting gate connections for simulation or description.  
- Chemical equations: Molecular structure recognition using models like ChemBERTa integrated with vision, parsing SMILES from images for property prediction.  
- Domain-specific languages (DSLs): Code snippet extraction from screenshots, where LVLMs like GPT-4V tokenize and ground DSL elements for execution tracing.  

**Core Property**  
The payload exists in **formal semantics**, not natural language.

---

### D. Temporal Carriers

**Leverage**  
- Cross-turn or cross-frame accumulation: Depends on recurrent or transformer memory mechanisms (e.g., LSTM states or KV caches) to integrate information sequentially in extended contexts.  

**Examples**  
- Multi-turn chat history: Dialogue state tracking in conversational LVLMs, aggregating prior visual-text exchanges for context-aware responses.  
- Video sequences: Temporal attention in models like Video-LLaMA, fusing frame embeddings over time for event summarization.  
- Progressive disclosure across steps: Chain-of-thought processing in multimodal chains, where intermediate visual analyses build cumulative understanding.  

**Core Property**  
The payload **emerges only after aggregation over time**.

---

### E. Contextual Carriers

**Leverage**  
- Role inference: Incorporates persona-based or bias-tuned embeddings (e.g., in fine-tuned LVLMs) to derive implied roles from visual cues.  
- Situational priors: Applies Bayesian priors or contextual embeddings (e.g., in CLIP-like models) to modulate processing based on scene indicators.  
- Implied authority: Uses trust heuristics in multimodal alignment, prioritizing cues from perceived authoritative elements.  

**Examples**  
- Workplace scenes: Environmental cue processing in models like VisualBERT, inferring professional roles from office layouts for caption refinement.  
- UI dashboards: Interface element parsing in LVLMs like Donut, extracting metrics and inferring user intent from layout hierarchies.  
- “Expert” or professional environments: Scene classification in models like BLIP, using visual priors (e.g., lab coats) to ground domain-specific responses.  

**Core Property**  
The payload is **not explicit**; it is inferred from context.

---

### F. Cross-Channel Composite Carriers

**Leverage**  
- Modality conflict: Handles resolution in fusion layers (e.g., cross-attention in models like Flamingo) to reconcile differing signals.  
- Modality dominance: Applies weighting in alignment mechanisms (e.g., vision-text in GPT-4V), where one modality guides the other's interpretation.  
- Multimodal fusion behavior: Employs early or late fusion strategies to integrate features, creating unified representations from disparate inputs.  

**Examples**  
- Text states one thing while image, table, or audio implies another: Cross-modal verification in models like LLaVA, aligning caption with visual content for consistency checks.  
- Audio contradicting visuals in videos: Audiovisual synchronization in models like AV-HuBERT, fusing speech and lip movements for transcription accuracy.  
- Table data clashing with narrative text: Multimodal document processing in LayoutLMv3, merging tabular and textual semantics for report summarization.  

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
- It is designed to support:
  - safety benchmarking,
  - defense evaluation,
  - theoretical analysis,
  - and deployment risk assessment.
- No carrier listed here implies misuse; all are framed for **defensive research**.
- Author takes nor bears any responsibility for misuses by others.

---

