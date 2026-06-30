# Relational Structure (Graphs and Layout as Carriers)

Ontology class: B (Structural), with crossover into E (Contextual) when structure is used to anchor reasoning.

## Modality

- Knowledge graphs
- Entity-relationship diagrams
- Network visualizations
- Dependency trees
- Tables, forms, and document layout
- Typographic and OCR-rendered text inside images

## Why this carrier class matters

The intent lives in relationships, not tokens. A text filter scans for words, but the reasoning the model performs happens over nodes, edges, rows, and reading order. When the payload is distributed across structure, no single element looks unsafe, and the meaning only assembles after the model parses the layout. The same property shows up whether the structure is a flowchart, a table, or text fragments arranged across an image.

## Literature examples

| Paper / attack family | What the carrier is | How binding typically works | Carrier class |
|---|---|---|---|
| [FigStep / FigStep-Pro](https://arxiv.org/abs/2311.05608) | Harmful instructions rendered as typographic images, with FigStep-Pro splitting text across visual tiles so no single tile is complete | "Follow the steps shown in the image" while the safety-aligned text channel stays clean | B |
| [Image-Based Prompt Injection (IPI)](https://arxiv.org/abs/2603.03637) | OCR-readable instruction text embedded into natural image regions through font, scale, and background-aware placement | The model reads the embedded text during normal captioning or VQA and treats it as instruction | B (perceptual overlap with A) |
| [CrossInject](https://arxiv.org/abs/2504.14348) | Adversarial cues aligned in the visual latent space and recombined through layout and structure rather than one visible string | Visual latent alignment plus a textual command that grounds the structured cues | B (composite overlap with F) |
| [Prompt Injection 2.0](https://arxiv.org/abs/2507.13169) | Retrieved PDF pages, screenshots, charts, and document images entering an agentic pipeline as external content | Page or document image is bound as trusted context inside a RAG or tool-using workflow | B (agentic delivery) |
| [Cross-Agent Multimodal Provenance-Aware Defense](https://arxiv.org/abs/2512.23557) | Defense side: treats OCR text, metadata, image patches, and document regions as separate structural units needing trust labels | Sanitizes and tags each structural unit before it propagates between agents | B (defensive framing) |
| [Anchoring the Mind of Multimodal Reasoners](https://openaccess.thecvf.com/content/CVPR2026/html/Cong_Anchoring_the_Mind_of_Multimodal_Reasoners_Cognitive_Bias_as_a_CVPR_2026_paper.html) | A structured visual mind map whose node/edge layout supplies a pre-built reasoning chain | The mind-map structure acts as a "safe anchor" that the model extends into the harmful request | B carrying an E (contextual) effect |

Tables, spreadsheets, and financial records still fit this class on principle, but the published attack work is thin. They belong here because meaning comes from row, column, and field relationships rather than any single cell.

## Open gaps

- Direct table and layout attacks against current LVLMs are underexplored compared to typographic image attacks.
- Knowledge-graph style relational reasoning (renamed nodes, unlabeled processes) is intuitive as a carrier but lacks a clean primary citation; it currently leans on the structural reasoning behind FigStep and the mind-map structure in RA-Attack.
- Whether structural carriers evade safety because of weak visual-embedding alignment (FigStep's explanation) or because of distributed compositionality is still an open question.

# Relational Carrier Flow
<img width="895" height="1588" alt="relational_carrier_flow" src="https://github.com/user-attachments/assets/a3624de0-b5ae-465c-8675-09ed18d4e03f" />
