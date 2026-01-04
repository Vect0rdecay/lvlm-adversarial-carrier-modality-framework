# Temporal Sequences (Time as a Carrier)

## Modality

* Video

* Multi-frame image sets

* Stepwise diagrams

* Multi-turn interaction history

## Why it works
LVLMs integrate meaning across time, not just within a single input. Safety systems often analyze each turn or frame independently.

## Research Questions

* At what temporal depth does safety attribution fail?

* Does intent reconstruction occur only after aggregation?

* Can benign early context “anchor” later reasoning?

## Test Case Classes

* Benign → ambiguous → disallowed progression across frames

* Safe narration + risky implication emerging only when summarized

* “Explain what changes over time” prompts

## Evaluation Signals

* Refusal latency (how late safety triggers)

* Cross-frame semantic accumulation

* Internal contradiction between early and late safety judgments
