# Temporal Sequences (Time as a Carrier)

Ontology class: D (Temporal), with crossover into E (Contextual) when early turns anchor later reasoning and F (Composite) when audio and visual streams are involved.

## Modality

- Audio streams and prepended audio segments
- Video and multi-frame image sets
- Stepwise or chained images
- Multi-turn interaction history

## Threat relevance

LVLMs and speech-LLMs integrate meaning across time, not just within a single input. Safety systems often inspect each turn or frame on its own, so a payload that only assembles after aggregation can slip past per-step checks. The carrier here is timing, order, and accumulation: where an element sits in the sequence matters as much as what it contains.

## Literature examples

| Source | What the carrier is | Binding pattern | Carrier class |
|---|---|---|---|
| [Muting Whisper](https://aclanthology.org/2024.emnlp-main.430/) | A short universal audio segment prepended before speech | The prefix changes how the model handles everything that follows, muting transcription | D |
| [Controlling Whisper](https://arxiv.org/abs/2407.04482) | A prepended audio segment that shifts task selection | Position in the audio stream overrides the intended task, e.g. forcing translation over transcription | D |
| [Universal Acoustic Attacks for Speech-LLMs](https://aclanthology.org/2025.findings-emnlp.990/) | Timing and placement of a universal audio prefix, with selective variants keyed to speaker attributes | The prefix conditions later generation across prompts and can activate only for targeted inputs | D |
| [VoTA (Visualization-of-Thought Attack)](https://proceedings.neurips.cc/paper_files/paper/2025/hash/64365cafdbd0cb49c442edc02efda40d-Abstract-Conference.html) | A chain of images carrying risky "visual thoughts" where intent emerges across the sequence | The model is led to reason step by step over the image chain until unsafe content is reconstructed | D |
| [VisCRA](https://arxiv.org/abs/2505.19684) | A visual reasoning chain steered through attention-guided masking and staged induction | Multi-stage reasoning induction builds the unsafe conclusion across steps, not in one frame | D |
| [VisCo (Visual Contextual Attack)](https://aclanthology.org/2025.emnlp-main.487/) | A fabricated multi-turn dialogue history grounded in images | Earlier turns establish a plausible scenario so a later request reads as reasonable | D carrying an E (contextual) effect |
| [Rethinking Audio-Visual Adversarial Vulnerability](https://arxiv.org/abs/2502.11858) | Temporal redundancy across consecutive segments of audio-visual input | A temporal-invariance perturbation exploits cross-segment consistency; targets are recognition models, but the temporal-carrier concept transfers | D (composite overlap with F) |

## Open gaps

- Multi-turn text-only progressive-disclosure jailbreaks are well known anecdotally, but this notebook does not yet cite a verified primary source for them; VisCo currently stands in for the multi-turn dimension because its context is image-grounded.
- Pure video (continuous frames rather than curated image chains) is thinner in the LVLM attack literature than audio prepend work.
- The boundary between temporal accumulation and contextual anchoring is fuzzy: several of these attacks succeed because early sequence elements bias later judgment, which is also an E-class property.
