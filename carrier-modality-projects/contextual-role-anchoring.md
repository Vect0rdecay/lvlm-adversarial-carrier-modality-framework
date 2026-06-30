# Contextual Role and Scene Anchoring

Ontology class: E (Contextual)

Maps from prompt-carriers #8 (Physical World Artifacts), #9 (Role-Anchoring Visuals).

## Why this carrier class matters

The payload is not spelled out; the model infers it from scene, role, authority cues, game framing, or speech style. A lab photo, a dashboard, a competitive game scenario, or an emotional speaking style can shift what the model treats as plausible or permitted without any explicit harmful string in the carrier itself.

## Literature examples

| Source | Attack / technique | Carrier | Binding pattern |
|---|---|---|---|
| [StyleBreak](https://arxiv.org/abs/2511.10692) | Style-aware audio jailbreak | Paralinguistic/extralinguistic speech attributes | Adversarial TTS style around malicious query |
| [VisCo](https://aclanthology.org/2025.emnlp-main.487/) | Image-driven context injection | Fabricated multi-turn scenario grounded in images | Fake dialogue history makes harmful ask plausible |
| [GAMBIT](https://aclanthology.org/2026.acl-long.367/) | Gamified jailbreak | Competition framing, rules, role-as-participant | Model pursues "winning" over safety monitoring |
| [Anchoring the Mind of Multimodal Reasoners](https://openaccess.thecvf.com/content/CVPR2026/html/Cong_Anchoring_the_Mind_of_Multimodal_Reasoners_Cognitive_Bias_as_a_CVPR_2026_paper.html) | Reasoning-chain anchoring (RA-Attack) | Visual mind map as "safe" cognitive anchor | Early anchor biases later harmful rationalization |

Role or authority visuals (uniforms, credentials, professional environments) remain intuitive E-class examples even where dedicated attack papers are sparse.

## Open gaps

- Generic text jailbreak framing (fiction, research, DAN-style roles) is abundant but mostly text-only; this file focuses on multimodal context carriers.
- Physical-world artifact carriers (#8) overlap A and E depending on whether the payload is sensory appearance or inferred environmental role.
