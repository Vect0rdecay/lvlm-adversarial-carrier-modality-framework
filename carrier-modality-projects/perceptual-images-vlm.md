# Perceptual Images and VLM Attacks

Ontology class: A (Perceptual)

## Threat relevance

Most VLM attack literature lives here. The payload is encoded in pixels, patches, or vision-encoder features before the model ever reasons in language. Text safety filters never see the carrier because it never enters as a string. Physical-world robustness, downscaling survival, and encoder-only perturbations are all variations on the same idea: intent carried in raw sensory space.

## Literature examples

| Source | Attack / technique | Carrier | Binding pattern |
|---|---|---|---|
| [Adversarial Patch](https://arxiv.org/abs/1712.09665) | Localized patch attack | Printed or overlaid texture that dominates classification | Patch placed in scene; model attends to patch region |
| [LaVAN](https://arxiv.org/abs/1801.02608) | Localized adversarial region | Same as patch family; optimized for object-context placement | Object-aware patch placement in image |
| [RP2](https://arxiv.org/abs/1707.08945) / [EoT](https://arxiv.org/abs/1707.07397) | Physical-world robust perturbation | Printed/projected structure surviving capture and resampling | Sticker, sign, or projection in environment |
| [Chameleon](https://arxiv.org/abs/2512.04895) | Scaling-robust visual prompt injection | Perturbation that activates after image downscaling in preprocessing | Image submitted to VLM pipeline with standard resize |
| [Image-Based Prompt Injection (IPI)](https://arxiv.org/abs/2603.03637) | Visually embedded instruction text | OCR-readable text blended into natural regions | Benign caption or VQA prompt; model reads embedded text |
| [VEAttack](https://arxiv.org/abs/2505.17440) | Vision-encoder-only perturbation | Shifted patch-token features without task-specific gradients | Image paired with ordinary text prompt |
| [Image Hijacks](https://arxiv.org/abs/2309.00236) | Behaviour-matching adversarial image | Small perturbation that forces targeted VLM behaviour at runtime | Benign text prompt; image hijacks output |
| [AnyAttack](https://arxiv.org/abs/2410.05346) | Self-supervised targeted perturbation | Pre-trained noise generator produces transferable adversarial images | Any image + target output specification |

## Open gaps

- Physical-world VLM attacks (beyond patches and scaling) are less mature than classifier-targeted physical attacks.
- Whether encoder-only attacks generalize across LVLM architectures with different fusion designs is still uneven in the literature.
