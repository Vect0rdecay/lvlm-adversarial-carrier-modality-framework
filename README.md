# lvlm-adversarial-carrier-modality-framework
**A Framework for Representation-Level Safety Evaluation in Large Vision–Language Models**

---
# LVLM-Carrier-Ontology  
## Overview

This repository contains a framework for studying inference-phase safety failures in Large Vision–Language Models (LVLMs).

The core idea of this project is that unsafe model behavior does not originate from prompt text alone, but from latent intent reconstruction across representations—including images, structure, symbols, time, and context. This repository provides:

- A carrier ontology for categorizing non-textual and cross-modal prompt representations  
- A theory-grounded threat model under realistic black-box assumptions  
- A structural failure classification identifying unavoidable classes of safety degradation  
- A defensive evaluation protocol with metrics beyond Attack Success Rate (ASR)

This is a **measurement and analysis framework**, not an exploit toolkit.

---

## Motivation

Most LVLM safety research focuses on textual jailbreaks or pixel-level visual perturbations. But modern LVLMs reason over structure, relationships, abstraction, time, and inferred context.

This project formalizes **representation-level risk**, enabling principled evaluation of prompt-based defenses, inference-time safety mechanisms, and multimodal alignment strategies.

---

## Key Contributions

### 1. Carrier Ontology
A carrier is any representation capable of encoding **latent task intent** that a model can reconstruct during inference.

Carrier classes: **Perceptual** (images, audio, video) · **Structural** (graphs, tables, flowcharts) · **Symbolic** (math, logic, formal notation) · **Temporal** (multi-turn or sequential inputs) · **Contextual** (roles, environments, authority cues) · **Composite** (cross-modal or conflicting representations). Each carrier is specified using a `CarrierRecord` schema.

---

### 2. Threat Model (Black-Box, Inference-Phase)

Assumptions: query-only access, no gradients/weights/training data, no system-level compromise, only intended user-facing interfaces. The adversary objective is **intent reconstruction**, not explicit instruction injection.

---

### 3. Evaluation Protocol

The framework evaluates *how* safety fails, not just *whether* it fails.

Metrics: **SLI** (safety latency — how late safety triggers) · **IRD** (intent reconstruction depth — abstraction depth before risk emerges) · **SLS** (semantic leakage — actionable detail leakage) · **MDC** (modality dominance — which modality overrides safety) · **RS** (refusal stability — consistency under paraphrase/context)

---

### 4. Structural Failure Classes

The repository formalizes **eight classes of unavoidable failure modes**, including:

- **Representation gap failures** – safety filters trained on text miss equivalent intent encoded in non-textual carriers
- **Temporal intent emergence** – benign individual turns combine into unsafe intent only visible across a sequence
- **Distributed compositional payloads** – unsafe intent is split across modalities so no single channel triggers defenses
- **Cognitive operation mismatch** – the model performs reasoning operations (analogy, abstraction) that safety classifiers don't monitor
- **Modality dominance bias** – one modality's signal overrides safety cues from another during fusion
- **Abstraction amplification** – abstract or symbolic inputs get decoded into concrete unsafe outputs at inference time
- **Refusal instability** – refusal behavior is inconsistent across semantically equivalent rephrasings or contexts
- **Contextual prior override** – role, authority, or environmental framing suppresses learned safety priors

---
## Intended Use Cases

This framework is designed for defensive benchmarking, inference-phase safety evaluation, red-team / blue-team alignment research, academic surveys, and pre-deployment risk assessment for multimodal systems. It is **not** intended for misuse, exploit development, or policy circumvention.

---

## Research Positioning

> This project does not provide step-by-step attacks.  
> It characterizes **structural failure modes** that arise from representation-level reasoning in LVLMs. Look at this more like vulnerability classes.

The goal is to shift the safety conversation from:
> “Which prompt broke the model?” (exploit)

to:
> “Which representations inevitably reconstruct unsafe intent?” (vulnerability)

---

## Citation

If you use this framework in academic work, please cite as:
@misc{lvlm_carrier_ontology,
title = {A Carrier Ontology and Failure Taxonomy for Inference-Phase Safety in LVLMs},
author = {Vect0rdecay},
year = {2026},
note = {Github / working paper}
}
