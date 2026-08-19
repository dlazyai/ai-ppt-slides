# Standard Workflow

## Overview

The dLazy PPT workflow emphasizes staged confirmation. Instead of immediately generating the entire presentation, it first confirms the outline, style, and sample slide to reduce rework.

## Stage 1: Review the Source Material

The agent first determines:

- The topic and central argument
- The target audience
- The presentation objective
- The required slide count
- Content that must be included or excluded
- Whether any image assets are required

## Stage 2: Confirm the Outline

The agent generates `outline.md`, which usually includes:

- Slide number
- Slide title
- 3-5 key points per slide
- The role of each slide, such as cover, agenda, concept explanation, process, comparison, data evidence, or summary
- Optional visual ideas
- Required image assets and how they will be used

No final slide images, `speech.md`, or `.pptx` file should be generated before the outline is approved.

## Stage 3: Confirm the Visual Style

The agent proposes 2-3 style directions and recommends one. Candidates come from the 12 built-in styles, including clean professional, scientific defense, hand-drawn technical explanation, McKinsey-style, party and government red, and teaching courseware, as well as your personal style library. The agent can also reproduce a style from screenshots, a PDF, or a presentation you provide. See [Styles and Personal Style Library](/en/styles.md) for complete style previews.

After a style is selected, the entire presentation should maintain a consistent visual language, while individual slide layouts may vary according to the content.

## Stage 4: Check That dLazy Is Configured

Every slide image is generated through dLazy by `scripts/image_gen.py`. Before the first image, the agent runs `dlazy_ppt_runtime.py doctor --check-api` to confirm the key is configured and the model is available.

This is a readiness check, not a choice: there is no second image backend, and the agent must not fall back to its own built-in image tool.

## Stage 5: Generate and Confirm a Sample Slide

Generate one sample slide first and check:

- Whether the text is clear
- Whether the style matches expectations
- Whether the information density is appropriate
- Whether the colors and layout are stable
- Whether the design can scale to the full presentation

Generate the full deck only after the sample slide is approved.

## Stage 6: Batch Generation

After the sample slide is approved, the agent generates `origin_image/slide_XX.png` one slide at a time. In environments that support sub-agents, one sub-agent generates each slide in parallel to speed up multi-slide production. Every slide follows the same style and the same generation parameters approved for the sample.

## Stage 7: Quality Review and Fixes

Before assembly, the agent reviews every slide for text clarity, consistency with the outline, clipped content, visual consistency, unnecessary page numbers, and overlapping elements. Slides with serious issues are regenerated using stricter prompts, while minor local issues are preferably corrected with targeted image-editing tools.

## Stage 8: Speaker Notes and Assembly

The agent generates the speaker notes in `speech.md`, then uses `assemble_ppt.py` to assemble the presentation into a `.pptx` file. The speaker notes are automatically added to the notes section of each slide.

## Stage 9 (Optional): Save the Style

If the presentation uses a custom or adjusted style, the agent notes in the final report that you can save it to your personal style library and reuse it by name in the future. See [Styles and Personal Style Library](/en/styles.md).
