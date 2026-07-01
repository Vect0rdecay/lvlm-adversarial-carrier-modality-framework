# Structural Failure Classes

Eight recurring failure patterns that arise from representation-level reasoning in LVLMs. These are structural properties of multimodal inference, not specific exploits. Each class maps to one or more carrier classes from the [ontology](ontology.md).

---

### 1. Representation Gap Failures

Safety filters trained on text miss equivalent intent encoded in non-textual carriers. The model understands the carrier content, but the safety layer never inspects it.

**Primary carrier classes:** A (Perceptual), C (Symbolic)  
**Example:** a pixel-level perturbation encodes an instruction that the vision encoder processes but the text-level safety classifier never sees.

---

### 2. Temporal Intent Emergence

Individually benign turns or frames combine into unsafe intent only visible across a sequence. Per-step safety checks pass because no single step is harmful.

**Primary carrier classes:** D (Temporal)  
**Example:** a multi-turn conversation where each message is innocuous but the accumulated context reconstructs a prohibited request.

---

### 3. Distributed Compositional Payloads

Unsafe intent is split across modalities so no single channel triggers defenses. The payload only assembles after multimodal fusion.

**Primary carrier classes:** F (Composite), B (Structural)  
**Example:** benign text asks the model to "follow the diagram," while the diagram encodes the harmful instruction in node-edge structure.

---

### 4. Cognitive Operation Mismatch

The model performs reasoning operations (analogy, abstraction, optimization) that safety classifiers don't monitor. Safety checks inspect input and output but not intermediate reasoning steps.

**Primary carrier classes:** C (Symbolic), B (Structural)  
**Example:** a mathematical optimization problem whose solution, when decoded, produces prohibited content — the safety layer sees math in and math out, missing the semantic meaning.

---

### 5. Modality Dominance Bias

One modality's signal overrides safety cues from another during fusion. The model's fusion architecture implicitly trusts one channel more than another.

**Primary carrier classes:** F (Composite), A (Perceptual)  
**Example:** a safety-aligned text prompt is overridden by a strong visual signal in the image encoder because cross-attention weights favor vision features.

---

### 6. Abstraction Amplification

Abstract or symbolic inputs get decoded into concrete unsafe outputs at inference time. The input is high-level enough to pass safety checks, but the model's reasoning concretizes it.

**Primary carrier classes:** C (Symbolic), B (Structural)  
**Example:** an abstract flowchart describing a "process" that the model renders as step-by-step instructions for a prohibited activity.

---

### 7. Refusal Instability

Refusal behavior is inconsistent across semantically equivalent rephrasings or contexts. The safety boundary is not a clean decision surface.

**Primary carrier classes:** E (Contextual), D (Temporal)  
**Example:** the model refuses a request phrased one way but complies when the same intent is rephrased, presented in a different language, or preceded by specific context.

---

### 8. Contextual Prior Override

Role, authority, or environmental framing suppresses learned safety priors. The model infers that the context makes the request permissible.

**Primary carrier classes:** E (Contextual)  
**Example:** a professional-environment image (lab, medical setting) primes the model to treat a dangerous request as a legitimate domain query.

---

## Relationship to Carrier Classes

| Failure class | Primary carriers | Interaction |
|---|---|---|
| Representation gap | A, C | Carrier bypasses text-layer safety entirely |
| Temporal emergence | D | Sequential aggregation creates intent not present in any single step |
| Distributed payload | F, B | Fusion assembles fragments from separate channels |
| Cognitive mismatch | C, B | Reasoning operations escape safety monitoring scope |
| Modality dominance | F, A | Fusion weighting favors adversarial channel |
| Abstraction amplification | C, B | Model concretizes abstract input past safety threshold |
| Refusal instability | E, D | Safety boundary is context-sensitive and inconsistent |
| Contextual override | E | Environmental framing suppresses safety priors |
