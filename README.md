# RLHF Preference Ranking Dataset

A human-annotated dataset of 50 AI response pairs scored and ranked using a 5-dimension quality rubric, designed to mirror the preference annotation workflow used in Reinforcement Learning from Human Feedback (RLHF) pipelines.

---

## What this is

RLHF is the technique used by AI labs to improve language models using human feedback. Human annotators compare pairs of AI responses and judge which one is better — and why. This dataset replicates that workflow from end to end: prompts were written, two AI responses were generated per prompt, and each pair was annotated according to a structured rubric.

This project was built as a practical demonstration of data annotation skills relevant to roles at AI labs, annotation platforms, and ML data teams.

---

## Dataset at a glance

| Property | Value |
|----------|-------|
| Total prompts | 50 |
| Annotated pairs | 49 |
| Unannotated | 1 (R-10 — responses missing) |
| Prompt categories | 5 |
| Annotation dimensions | 5 |
| Preferred A | 34 (69.4%) |
| Preferred B | 14 (28.6%) |
| Ties | 1 (2.0%) |

---

## Prompt categories

The 50 prompts are evenly split across five categories (10 prompts each), chosen to cover the range of tasks a general-purpose language model is typically evaluated on:

| Category | IDs | Description |
|----------|-----|-------------|
| Factual questions | F-01 to F-10 | Accurate, well-explained factual information |
| Creative output | C-01 to C-10 | Creative writing tasks with explicit format constraints |
| Instruction compliance | I-01 to I-10 | Prompts with highly specific, measurable constraints |
| Reasoning and recommendations | R-01 to R-10 | Open-ended problems requiring judgment and trade-off reasoning |
| Sensitive / calibrated advice | S-01 to S-10 | Emotionally or contextually sensitive prompts requiring careful tone |

---

## Annotation rubric

Each response pair was scored on **five dimensions**, each rated 1–5, before a final preference was declared.

| Dimension | What it measures |
|-----------|-----------------|
| Instruction following | Did the response do exactly what was asked — format, length, constraints? |
| Factual accuracy | Are all claims correct? Any hallucinations or errors? |
| Helpfulness | Would a real user benefit? Does it address the underlying goal? |
| Clarity and coherence | Is it well-written, well-structured, and easy to follow? |
| Safety and tone | Is it appropriate, non-harmful, and correctly calibrated for context? |

Full rubric documentation — including scoring criteria for each level, edge case rules, and inter-rater reliability guidance — is in [`ANNOTATION_RUBRIC.md`](./ANNOTATION_RUBRIC.md).

---

## Score summary

### Response A

| Dimension | Average score |
|-----------|--------------|
| Instruction following | 4.94 / 5 |
| Factual accuracy | 5.00 / 5 |
| Helpfulness | 5.00 / 5 |
| Clarity | 5.00 / 5 |
| Safety | 5.00 / 5 |

### Response B

| Dimension | Average score |
|-----------|--------------|
| Instruction following | 4.88 / 5 |
| Factual accuracy | 5.00 / 5 |
| Helpfulness | 4.73 / 5 |
| Clarity | 5.00 / 5 |
| Safety | 5.00 / 5 |

### Key finding

**Helpfulness was the primary differentiator** between A and B. Response A was preferred in 69.4% of annotated cases, most frequently because it provided deeper reasoning, more specific detail, or better addressed the user's underlying goal — not just the surface request. Instruction following was the second most common deciding factor, appearing in cases where one response violated a specific constraint (word count, format, exclusion rule, or no-analogy directive).

**Both response sets scored perfectly on factual accuracy, clarity, and safety**, indicating that the response quality was generally high and that preference decisions were driven by comparative depth and compliance rather than outright failures.

---

## File structure

```
rlhf-preference-dataset/
│
├── README.md                                  ← this file
├── ANNOTATION_RUBRIC.md                       ← full rubric and methodology
└── rlhf_preference_dataset_annotated.xlsx     ← main dataset (2 sheets)
```

### Spreadsheet structure

The `.xlsx` file contains two sheets:

**Sheet 1 — Annotations**

| Column | Description |
|--------|-------------|
| `prompt_id` | Unique ID (e.g. F-01, C-05, I-10) |
| `category` | One of the five prompt categories |
| `prompt_text` | The full prompt given to the model |
| `response_A` | Raw AI-generated Response A |
| `response_B` | Raw AI-generated Response B |
| `A_instruction` | Instruction following score for A (1–5) |
| `A_accuracy` | Factual accuracy score for A (1–5) |
| `A_helpfulness` | Helpfulness score for A (1–5) |
| `A_clarity` | Clarity score for A (1–5) |
| `A_safety` | Safety and tone score for A (1–5) |
| `B_instruction` | Instruction following score for B (1–5) |
| `B_accuracy` | Factual accuracy score for B (1–5) |
| `B_helpfulness` | Helpfulness score for B (1–5) |
| `B_clarity` | Clarity score for B (1–5) |
| `B_safety` | Safety and tone score for B (1–5) |
| `preferred_response` | Final preference: A, B, or Tie |
| `preference_reason` | 1–2 sentence annotation justification |
| `notes` | Edge cases, artifacts, or ambiguities flagged |

**Sheet 2 — Summary**

Aggregate statistics including per-dimension average scores for A and B, preference distribution, and total annotation counts.

---

## Known issues

| Prompt ID | Issue |
|-----------|-------|
| R-10 | Both responses are missing. The prompt ("I am considering going vegetarian for health reasons...") was not answered. Row is marked N/A across all annotation columns. |
| I-01 | Response B contains a streaming artifact at the start of the response. This was flagged in the `notes` column and penalised under Instruction Following (score: 4 instead of 5). |

---

## Sample annotation

| Field | Value |
|-------|-------|
| `prompt_id` | F-04 |
| `prompt_text` | Explain how a black hole forms. Do not use any analogies — use only direct scientific explanation. |
| `A_instruction` | 5 |
| `A_accuracy` | 5 |
| `A_helpfulness` | 5 |
| `A_clarity` | 5 |
| `A_safety` | 5 |
| `B_instruction` | 5 |
| `B_accuracy` | 5 |
| `B_helpfulness` | 4 |
| `B_clarity` | 5 |
| `B_safety` | 5 |
| `preferred_response` | A |
| `preference_reason` | A strictly follows the no-analogies constraint, using precise scientific terminology throughout (nucleon degeneracy pressure, Schwarzschild radius). B is accurate and clear but introduces slight simplification. Both are strong; A wins on instruction compliance. |
| `notes` | Prompt explicitly said no analogies. A follows this strictly; B stays mostly technical but with minor softening. |

---

## Methodology notes

- Responses were generated using publicly available large language models.
- Each response was scored independently before the preference decision was made, to prevent confirmation bias.
- The `preference_reason` column was written before moving to the next row, to ensure reasoning was not reverse-engineered from the preference.
- Ties were kept rare (1 out of 49 annotated pairs, ~2%) in line with industry best practice. The single tie (C-10) was justified because both responses met all constraints identically with equal creative quality.
- The rubric was designed before annotation began and applied consistently throughout. No new rules were introduced mid-dataset.

---

## How to use this dataset

This dataset can be used as a reference or starting point for:

- Learning the structure and content of RLHF preference annotation
- Understanding how quality rubrics are applied to AI response evaluation
- Training or fine-tuning reward models (note: small dataset, use for demonstration only)
- Building inter-rater reliability exercises for annotator onboarding

---

## About

Built as a portfolio project to demonstrate practical data annotation skills for AI/ML roles. The full workflow — prompt design, response generation, rubric development, and annotation — was completed end to end by a single annotator.

**Skills demonstrated:** preference annotation, rubric design, edge case documentation, inter-rater reliability awareness, structured data production, and dataset documentation.
