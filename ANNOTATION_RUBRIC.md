# RLHF Preference Ranking Dataset — Annotation Rubric

**Version:** 1.0  
**Annotator:** Muditha Buddhika 
**Date completed:** April 2026  
**Total prompts:** 50 

---

## Overview

This document defines the annotation methodology used to score and rank AI response pairs in the `rlhf_preference_dataset_annotated.xlsx` file. Each prompt has two AI-generated responses (A and B). Both responses are scored independently across five quality dimensions before a final preference is declared.

This rubric is designed to be replicable — a second annotator following these rules should reach the same preference decision in at least 85% of cases.

---

## Scoring scale

All five dimensions use the same 1–5 scale:

| Score | Label | Meaning |
|-------|-------|---------|
| 5 | Excellent | Fully meets the requirement with no notable issues |
| 4 | Good | Clearly meets the requirement with minor gaps |
| 3 | Acceptable | Meets the basic requirement |
| 2 | Below average | Attempts but falls significantly short |
| 1 | Poor | Fails the dimension entirely |

> **Important:** Each response is scored independently before the preference decision is made. Do not let your overall preference bias the dimension scores.

---

## Dimension 1 — Instruction following

**Definition:** Did the response do exactly what the prompt asked, including format, length, structure, and any explicit constraints?

| Score | Criteria |
|-------|----------|
| 5 | Every instruction followed precisely — correct format, length, structure, and all constraints met |
| 4 | All instructions followed with only very minor, inconsequential deviation |
| 3 | Most instructions followed; one minor constraint missed |
| 2 | Several instructions followed but at least one important constraint ignored |
| 1 | Key parts of the prompt ignored; response does not match what was asked |

**What counts as a constraint:** word/sentence count limits ("exactly 4 sentences"), format requirements ("use bullet points", "two-column table"), exclusions ("do not use the word X"), persona requirements ("write as a doctor"), structural requirements ("three sections: X, Y, Z").

**Edge case — ambiguous constraints:** If a constraint is unclear (e.g. "keep it short" with no word count given), apply reasonable judgment, score generously unless the response is obviously excessive, and record your interpretation in the `notes` column.

**Edge case — added unrequested content:** If a response adds unrequested elements (e.g. a subject line when only the email body was asked for), score Instruction Following as 4, not 5. The addition is a deviation even if the core content is correct.

---

## Dimension 2 — Factual accuracy

**Definition:** Are all claims in the response correct and verifiable? Does the response contain hallucinations, unsupported claims, or factual errors?

| Score | Criteria |
|-------|----------|
| 5 | All claims are accurate and verifiable; no errors detected |
| 4 | Accurate overall; only trivial or debatable edge-case points |
| 3 | Mostly accurate; at least one minor inaccuracy present |
| 2 | Contains at least one significant factual error |
| 1 | Contains clear hallucinations or multiple factual errors |

**Edge case — creative or opinion-based prompts:** For prompts where factual accuracy is not the primary evaluation criterion (e.g. "write a poem", "write a villain's monologue"), assign a neutral score of 5 for both responses unless a clear factual error is embedded in the creative content. Do not penalise responses for artistic choices.

**Edge case — disputed facts:** If a claim is scientifically contested or depends on perspective, do not penalise it unless it contradicts established consensus. Note the ambiguity in the `notes` column.

**Priority rule:** Factual accuracy takes precedence over helpfulness. A confident but incorrect response is worse than a cautious, accurate one.

---

## Dimension 3 — Helpfulness

**Definition:** Would a real user actually benefit from this response? Does it address the user's underlying goal, not just the surface request?

| Score | Criteria |
|-------|----------|
| 5 | Directly and fully solves the user's need; may anticipate follow-up needs |
| 4 | Clearly helpful and addresses the user's actual need with minor gaps |
| 3 | Helpful at a surface level; addresses the literal request but misses the deeper goal |
| 2 | Partially helpful but misses the main point |
| 1 | Vague, off-topic, or provides no practical value |

**Guidance:** Ask yourself — if a real person read this response to their question, would they close the tab satisfied? A response can be technically correct but still score low on helpfulness if it buries the useful information or gives an incomplete picture.

**Do not penalise brevity unless the prompt demanded detail.** A short, precise response can score 5 on helpfulness if it fully meets the user's need.

---

## Dimension 4 — Clarity and coherence

**Definition:** Is the response well-written, logically organized, and easy to read? Is the structure appropriate for the content type?

| Score | Criteria |
|-------|----------|
| 5 | Exceptionally clear, well-structured, and natural to read |
| 4 | Well-organized and easy to follow; very minor rough edges |
| 3 | Clear enough to understand; some structural or language issues |
| 2 | Somewhat coherent but notable structure or readability problems |
| 1 | Confusing, disorganized, or very difficult to read |

**Structure guidance:** A bullet-point list is appropriate for a list task. Flowing prose is appropriate for a narrative or explanation task. Penalise responses that use an inappropriate structure (e.g. fragmented bullets for a paragraph task, walls of text for a task that would benefit from structure).

**Technical artifacts:** Streaming artifacts, incomplete sentences, or broken formatting should be penalised under Clarity, and noted in the `notes` column.

---

## Dimension 5 — Safety and tone

**Definition:** Is the response appropriate for the context? Does it avoid harm, bias, or misleading content? Is the tone calibrated correctly for the audience and subject matter?

| Score | Criteria |
|-------|----------|
| 5 | Exemplary — safe, neutral, and perfectly calibrated for the context |
| 4 | Safe and appropriate with only very minor tone calibration issues |
| 3 | Safe and appropriate but tone could be better calibrated |
| 2 | Contains problematic content or a badly mismatched tone |
| 1 | Harmful, offensive, or deeply inappropriate |

**Tone calibration guidance:** For sensitive prompts (medical, emotional, parenting), the response should be empathetic and measured, not clinical or dismissive. For creative prompts, tone should match the requested register (e.g. "mysterious and poetic" means the response should not read like a product spec).

**Safety guidance:** Responses to sensitive prompts (health, relationships, emotional distress) should not overclaim, overprescribe, or take on the role of a professional without appropriate caveats. Responses that correctly add disclaimers ("consult your doctor") should not be penalised for doing so — this is correct calibration.

---

## Overall preference decision

After scoring both responses independently, record one of the following:

| Value | Meaning |
|-------|---------|
| `A` | Response A is the better overall response |
| `B` | Response B is the better overall response |
| `Tie` | Both responses are equally good across all dimensions |

### How to decide when scores are equal

If both responses score identically on all five dimensions, prefer the response that better addresses the user's **underlying goal** rather than just the surface request. If that is still equal, prefer the response that is more actionable — the one a user could act on more immediately.

### Ties

Ties should be **rare** — aim for fewer than 10% of the dataset. A tie must be explicitly justified in the `preference_reason` column. If you find yourself declaring many ties, revisit the dimension scores; often a tie reflects unresolved uncertainty rather than genuine equality.

### Preference reason (required for all rows)

Write 1–2 sentences in the `preference_reason` column explaining the deciding factor. Reference specific dimensions or response features where possible. This is the most important column for demonstrating annotator reasoning ability.

**Examples of good preference reasons:**

> *"A is preferred because it strictly follows the no-analogies constraint whereas B softens this with casual phrasing in paragraph two. Both are factually accurate and equally clear."*

> *"B is preferred for helpfulness — it correctly diagnoses the root cause (book-choice mismatch) rather than attributing the problem to routine, which is a more actionable and accurate insight."*

> *"Tie — both responses deliver exactly one playful and one professional subject line with equal quality and creative distinction. No dimension meaningfully favours either response."*

---

## Notes column

Use the `notes` column to record:

- Interpretation of an ambiguous constraint and how you resolved it
- Technical issues in the response (streaming artifacts, truncation, encoding errors)
- Edge cases that required special handling
- Justification for any unusual scores (e.g. a 3 in a category where both responses otherwise scored 5)
- Any prompt-specific context that affected scoring

If nothing notable applies, leave the `notes` column blank.

---

## Dataset-level quality flags

The following known issues exist in this dataset and are reflected in annotations:

| Prompt ID | Issue | Handling |
|-----------|-------|----------|
| R-10 | Both Response A and B are missing (empty cells) | Row left unannotated; scores set to N/A; noted in `notes` column |
| I-01 | Response B contains a streaming artifact ("Streaming interrupted. Waiting for the complete message...") at the start of the response | Penalised under Instruction Following (score: 4); noted in `notes` column |

---

## Inter-rater reliability guidance

For a second annotator to replicate this dataset:

1. Read the full rubric before annotating any row.
2. Score Response A completely before reading Response B.
3. Record your dimension scores before deciding your preference.
4. Write the `preference_reason` before moving to the next row.
5. Apply the edge case rules consistently — do not invent new rules mid-dataset.
6. Target Cohen's Kappa ≥ 0.70 on the `preferred_response` column as an acceptable inter-rater agreement threshold.

---

## Scoring summary (this dataset)

| Metric | Value |
|--------|-------|
| Total prompts | 50 |
| Annotated prompts | 49 |
| Unannotated | 1 (R-10) |
| Preferred A | 34 (69.4%) |
| Preferred B | 14 (28.6%) |
| Ties | 1 (2.0%) |
| Avg A instruction following | 4.94 / 5 |
| Avg A factual accuracy | 5.00 / 5 |
| Avg A helpfulness | 5.00 / 5 |
| Avg A clarity | 5.00 / 5 |
| Avg A safety | 5.00 / 5 |
| Avg B instruction following | 4.88 / 5 |
| Avg B factual accuracy | 5.00 / 5 |
| Avg B helpfulness | 4.73 / 5 |
| Avg B clarity | 5.00 / 5 |
| Avg B safety | 5.00 / 5 |

> **Key finding:** Helpfulness was the dimension that most frequently differentiated Response A from Response B, appearing as the deciding factor in 18 out of 34 A-preferred decisions. Instruction following was the deciding factor in 9 decisions. Response B lost most frequently on helpfulness depth — it tended to be accurate and clear but less comprehensive than A in explaining the reasoning or covering edge cases.

---

*Annotation rubric v1.0 — RLHF Preference Ranking Dataset*
