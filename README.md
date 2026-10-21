# Codebook and prompts for zero-shot behavior detection of group interaction

Supplementary materials of the paper

> Liu, S., McTeague, C., Klöckner, H. W., Dreyer, S., & Thoring, K. (2026). *Observing Group Interaction in the Physical Workspace: Zero-Shot Behavior Detection with Vision-Language Models.* Manuscript under review.

The paper presents a method that applies vision-language models (VLMs) to detect group interactions, and evaluates the models against trained human annotators. Adapting the method to another study amounts to revising two artifacts: the **behavior codebook** and the **prompt**. Both are published here so that they can be reused and adapted.

## Contents

```
codebook/
  codebook_group_interaction_detection.pdf    # codebook as given to the human annotators
  codebook_group_interaction_detection.docx   # editable source
  codebook_group_interaction_detection.md     # Markdown export of the same content
prompts/
  system_prompt_vision.txt        # system prompt: video with audio as input
  system_prompt_speech.txt        # system prompt: video plus timestamped transcript as input
  user_prompt_vision.txt          # user prompt: video with audio as input
  user_prompt_visionSpeech.txt    # user prompt: video plus timestamped transcript as input
CITATION.cff                      # citation metadata
.zenodo.json                      # Zenodo archiving metadata
LICENSE                           # CC BY 4.0
```

## The codebook

The codebook defines the target behaviors of the study (the coding scheme), the unitizing rules that segment the material into codable units, and the annotation principles that distinguish easily confused labels. The human annotators and the models work from the same codebook: the annotators receive it as a PDF, and the models receive its content through the structured prompt, where the coding scheme enters the *label* category and the annotation principles enter the *task* category.

The study setting is the NASA Moon Survival Task, a group decision-making exercise in which participants rank survival items on a shared touchscreen. Please adapt the codebook to your study settings befor using.

## The prompts

Each prompt pair consists of a **system prompt**, which describes the input modalities and the cross-modal analysis method, and a **user prompt**, which carries the study-specific content. The user prompt follows the structure described in the paper:

| Category | Content in the user prompt |
|---|---|
| Context | `## Background` and `## Analyzed Person` |
| Task | `## Task`, Steps 1 to 4 (unitizing, indicator description, label assignment, behavior target) |
| Label | `**Label List**` in Step 3 |
| Evidence | the nonverbal and verbal indicator categories in Step 2 |
| Output example | `## Output` with the JSON array |

Two input configurations are provided:

- **vision**: the model receives the pre-processed video together with its audio track (`system_prompt_vision.txt` and `user_prompt_vision.txt`).
- **visionSpeech**: the model receives the pre-processed video together with a timestamped transcript (`system_prompt_speech.txt` and `user_prompt_visionSpeech.txt`).

The placeholders `{person}` and `{transcript}` are filled per analysis run. The doubled braces `{{` and `}}` in the JSON example are escaped for Python string formatting and become single braces in the final prompt. The video is pre-processed with YOLO-based person tracking, so that each participant carries a person ID label and a bounding box.

### Model configuration used in the paper

The prompts were used with three open-source models of the Qwen family, Qwen3.5-397B, Qwen3.8-27B, and Qwen3.5-9B, in 8-bit quantized checkpoints. The video was sampled at two frames per second at a resolution of 1280 by 720 pixels. Decoding was greedy (temperature 0) and without thinking mode. See the paper for details.

## Adapting the materials to another study

Researchers can rewrite the *context* for their own research setting and participants and replace the *label* definitions with the codebook of their own study. The annotation procedure in *task*, the indicator categories in *evidence*, and the output format in *output example* can largely be reused.

## Citation

Please cite the paper when using these materials. Citation metadata for this repository is in `CITATION.cff`; the archived version on Zenodo carries a DOI.

## License

The materials are released under the [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0).
