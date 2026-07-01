# Audio Streams

Ontology class: A (Perceptual) and D (Temporal) depending on whether the carrier is waveform content or prepend timing.

Maps from prompt-carriers #1 (Audio Streams).

## Threat relevance

Speech-LLMs and audio-capable LVLMs process waveforms and spectrograms before text safety layers engage. Attacks can hide in paralinguistic style, imperceptible perturbations, or a short prefix that changes how the model handles everything after it. Audio is both a sensory carrier and, when prepended, a temporal one.

## Literature examples

| Source | Attack / technique | Carrier | Binding pattern |
|---|---|---|---|
| [Muting Whisper](https://aclanthology.org/2024.emnlp-main.430/) | Universal acoustic prepend | Short learned audio segment before speech | Any speech input; prefix mutes or alters transcription |
| [Controlling Whisper](https://arxiv.org/abs/2407.04482) | Task-control prepend | Universal segment forcing task switch | Model set to transcribe; audio forces translate |
| [Universal Acoustic Attacks for Speech-LLMs](https://aclanthology.org/2025.findings-emnlp.990/) | Flexible prepend / selective prepend | Fixed prefix; selective activation by speaker attributes | Prefix prepended to user audio in speech-LLM pipeline |
| [AudioJailbreak](https://arxiv.org/abs/2505.14103) | Suffixal jailbreak audio | Crafted audio with asynchrony, universality, over-the-air robustness | Weak or strong adversary control over prompt |
| [AdvWave](https://arxiv.org/abs/2412.08608) | Stealthy adversarial waveform | Perturbation resembling environmental noise | Malicious audio query + optimized adversarial wave |
| [StyleBreak](https://arxiv.org/abs/2511.10692) | Style-aware jailbreak | Paralinguistic and extralinguistic speech attributes (emotion, age, gender) | TTS-synthesized adversarial speech style |

## Open gaps

- Over-the-air audio jailbreaks against commercial speech APIs are newer and less replicated than text jailbreaks.
- The boundary between perceptual audio perturbation (A) and temporal prepend (D) is worth tagging explicitly when annotating carriers.
