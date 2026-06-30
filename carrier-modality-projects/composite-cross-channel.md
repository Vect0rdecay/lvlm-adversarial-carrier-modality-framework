# Composite Cross-Channel Carriers

Ontology class: F (Cross-Channel Composite)

Maps from prompt-carriers #10 (Clean Trigger Artifacts) plus VLM fusion and cross-modal attack literature.

## Why this carrier class matters

The payload is split across channels, or hidden in whichever modality the safety stack treats as secondary. Benign text can bind the model to a harmful image, audio can steer text generation, and shared attention paths let one perturbation affect both image and text inputs. Fusion is the attack surface.

## Literature examples

| Source | Attack / technique | Carrier | Binding pattern |
|---|---|---|---|
| [Image Hijacks](https://arxiv.org/abs/2309.00236) | Image + text split | Benign text prompt; visual hijack shifts behaviour | Text asks a normal question; image controls answer |
| [Chain of Attack (CoA)](https://arxiv.org/abs/2411.15720) | Cross-modal semantic alignment | Iterative image-text semantic update for transfer | Black-box proxy; image perturbation aligned to target text |
| [Doubly-Universal UAP](https://arxiv.org/abs/2412.08108) | Shared vision-encoder perturbation | Single perturbation effective across image and text inputs | UAP added to images regardless of accompanying text |
| [CrossInject](https://arxiv.org/abs/2504.14348) | Cross-modal prompt injection | Visual latent alignment + textual guidance enhancement | Coordinated image and text adversarial cues for agents |
| [AudioJailbreak](https://arxiv.org/abs/2505.14103) / [AdvWave](https://arxiv.org/abs/2412.08608) | Audio-to-language fusion | Audio steers text generation in LALMs | Audio input; harmful text output via fusion |
| [Rethinking Audio-Visual Adversarial Vulnerability](https://arxiv.org/abs/2502.11858) | Modality misalignment | Incongruence between audio and visual streams | Synchronized perturbation across modalities (recognition models) |

## Open gaps

- Agentic pipelines that combine retrieved documents, tool outputs, and user text multiply composite carriers; Prompt Injection 2.0 describes the threat landscape but is not a single attack technique.
- Which modality "wins" in fusion layers varies by architecture and is still hard to predict from papers alone.
