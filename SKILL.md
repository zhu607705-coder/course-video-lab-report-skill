---
name: course-video-lab-report
description: Use when a user wants to turn a course video, recorded lecture, learning-platform lesson, or experiment demonstration into a lab report draft, experiment-report writing anchors, extracted text notes, formulas, procedures, instrument sketches, and cleaned temporary artifacts.
metadata:
  short-description: Convert course videos into lab report drafts
---

# Course Video Lab Report

## Purpose

Convert a course video into materials that directly support a lab report: extracted text, experiment objectives, principles, instruments, hand-copyable diagrams, procedures, parameters, formulas, cautions, demo data, and data-analysis placeholders.

## Output Contract

Produce only durable deliverables in the project workspace:

- `reports/<experiment>-实验报告.md`: report-ready draft.
- `data/<experiment>-视频提取信息.md`: compact source notes and video-derived facts.
- `artifacts/<instrument-or-process>示意图.svg` and optionally `.png`: simple hand-copyable diagram, not a decorative poster.

Never save account credentials, login tokens, cookies, or API keys in deliverables.

## Workflow

1. Define the success target first.
   - Treat the source video as primary evidence.
   - Use web sources only to verify or clarify, and label non-video additions.
   - If no student experimental data exists, leave data analysis and conclusion as placeholders.

2. Gather source material.
   - Prefer official subtitles, transcript, slide text, API captions, or page text when available.
   - If no captions exist, extract video frames at a reasonable interval and OCR the frames.
   - Use audio transcription only when it is available and worth the cost.
   - MiniMax CLI is a good fit for vision OCR and synthesis when the user authorizes external model calls.

3. Keep temporary artifacts isolated.
   - Use a clear temp directory such as `/tmp/<course-video-task>/`.
   - Store downloaded video, extracted frames, contact sheets, full OCR, and intermediate synthesis only there.
   - Do not place raw screenshots or full OCR dumps in the project unless explicitly requested.

4. Extract report anchors.
   - Experiment title.
   - Objectives.
   - Analysis object and samples.
   - Theory and formulas.
   - Instrument model, structure, and parameters.
   - Reagents and glassware.
   - Operating procedure.
   - Data-processing method.
   - Cautions and error sources.
   - Demo data shown in the video.
   - What still requires the student's own data.

5. Synthesize the report.
   - Match existing project report style when examples exist.
   - Mark video demo data clearly as `视频演示数据`.
   - Add fillable tables for missing real measurements.
   - Keep conclusions conditional when real data is absent.
   - Use a plain hand-copy diagram: boxes, arrows, minimal labels, key formula.

6. Verify before completion.
   - Inspect the report and source-notes sections.
   - Search for leaked secrets with patterns such as `sk-`, `token=`, `eyJ`, `Cookie`, `Authorization`, account IDs, and passwords.
   - Search for placeholders and confirm they are intentional.
   - Verify created images exist and are readable.
   - Delete all temporary videos, frames, OCR files, browser caches, and model scratch files.

## MiniMax CLI Pattern

Use MiniMax only after the user accepts outbound model use for course media.

Suggested flow:

```bash
mmx vision describe <contact-sheet-or-frame>
mmx text chat --messages-file <messages.json> --max-tokens 10000 --temperature 0.2
```

For large OCR, split or write a valid messages JSON file instead of relying on fragile shell stdin. Ask MiniMax to distinguish video-derived facts from inferred report prose.

## Diagram Rule

For hand-written reports, favor this level of detail:

```text
光源 -> 单色器 -> 吸收池 -> 检测器 -> 读数
              I0 入射   It 透射

T = It / I0
A = lg(I0 / It) = -lgT
A = abc
```

Only create a polished diagram if the user explicitly wants a presentation-grade artifact.

## Stop Conditions

The task is not complete until:

- The report draft exists.
- The compact source-notes file exists.
- Any requested diagram exists and has been inspected.
- Secret scan has no hits.
- Temporary artifacts have been deleted and verified absent.

