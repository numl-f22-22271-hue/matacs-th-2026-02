# Design and assumptions

An admin sets up the criteria, weights, limits and scoring rules through admin screens. The system checks that weights total 100 and that every criterion says how to handle missing evidence. Publishing creates a fixed version for an evaluation. The application reads this configuration rather than hard-coding solar-specific rules.

For the sample, I treat prices as comparable CAD totals and the summaries as the available proposal evidence. Claimed credentials are enough for the initial document check, but need independent verification before award. I treat the budget and 16-week deadline as eligibility limits. If information needed to check eligibility is missing, the proposal waits for clarification.

**Experience assumption:** The brief gives no exact evaluation date. A year such as “since 2021” does not establish how many projects fall within a rolling five-year window. For this exercise, I assume A's 14, C's nine and E's eight installations qualify. Their scores are provisional. The reviewer must approve this assumption and obtain dated project lists before award. In a real evaluation, the system would record the evaluation date and keep assumed counts separate from verified ones.

## 1. Where I would use AI and rules

The flow is: select a category version and upload proposals; validate files; extract text with page references, using OCR if needed; ask AI to extract the requested facts; validate its response; have a reviewer resolve issues; run eligibility and scoring rules; present the ranking for a human decision.

AI is useful because bidders describe the same things in different ways. It can turn those descriptions into fields and point out missing or conflicting statements. I would leave eligibility, calculations and tie-breaks to code because those steps should follow explicit rules.

Each extracted fact needs a source quote and page reference. Code can check whether the quote exists, but that alone does not prove the interpretation is right. Reviewers see the source beside the extracted value. Unreadable files return for replacement rather than receiving a score.

## 2. Repeatable scores and clear reasons

The brief supplies weights but no scoring scale. I would use this illustrative admin-configured rubric:

| Criterion | Points |
|---|---|
| Experience | 30 × min(approved qualifying installs / 10, 1) |
| Price | 25 × (1 − price / 260000), for a known nonnegative price within budget |
| Timeline | 15 × (16 − weeks) / 16, for a known positive duration of at most 16 weeks |
| Warranty | 7.5 × min(panel years / 25, 1) + 7.5 × min(workmanship years / 12, 1) |
| Team and approach | 3.75 each for a named lead, specified crew, safety plan and monitoring plan |

Ten qualifying installations earn full experience points. The price and timeline formulas reward savings against fixed limits, so small savings earn relatively few points. These are design choices for the admin to approve before scoring, not rules supplied by the brief. Warranty durations must be nonnegative.

Missing optional evidence earns zero with a visible warning. Ambiguous evidence needs clarification or an approved assumption. For this exercise, a summary saying the lead is named is sufficient; the tool must not invent a name. Compare unrounded totals, then display two decimals. Ties use lower price, shorter duration, then proposal ID, in that order.

| Proposal | Experience | Price | Timeline | Warranty | Team | Total and result |
|---|---:|---:|---:|---:|---:|---|
| A | 30 | 2.12 | 3.75 | 13.75 | 15 | 64.62; first, provisional |
| E | 24 | 1.68 | 2.81 | 15 | 15 | 58.50; second, provisional |
| C | 27 | 1.54 | 1.88 | 13.75 | 7.50 | 51.66; third, provisional |
| D | — | — | — | — | — | Flagged: no schedule to check against the deadline |
| B | — | — | — | — | — | Ineligible: bidder-held licence not stated; a future subcontractor does not satisfy the requirement |

C's ranking assumes a reviewer has dismissed its instruction to manipulate the result. Until that review, C is flagged and unranked. Experience warnings remain visible for A, C and E without blocking their provisional ranks. D stays unranked until its schedule is clarified.

The same approved evidence and criteria version produce the same scores. A fresh AI extraction may differ, even at temperature zero. Store approved extractions and their document, schema, prompt and model versions. Reviewers can inspect each score's formula, inputs and source evidence.

## 3. Human review and audit

A reviewer checks mandatory facts, numbers and warnings before releasing the ranking. Corrections create new revisions, preserving the original evidence and result. Any override needs a reason, reviewer identity and timestamp. Log the document hash, criteria version, prompt/model details, extracted evidence, calculations, approvals and final decision. A human makes the award after outstanding assumptions and credentials have been checked.

## 4. Changes to criteria

An edit creates a new version without altering earlier results. A reviewer starts a new evaluation of the whole batch under that version. Reuse verified evidence where it still fits and request any newly required fields. Show what changed, and do not mix scores from different versions in one ranking.

## 5. Risks and safeguards

- **Wrong extraction or invented facts:** Require source quotes, validate field types and have a reviewer check important values.
- **Manipulation:** Treat proposal text as evidence, not instructions. Give the model no tools or network access, and send suspicious passages for review.
- **Bias:** Exclude irrelevant identity details from scoring and apply the same rubric to every bidder. Retain identity where needed to check who holds a licence.
- **Missing information:** Distinguish unknown from false. Use the configured clarification or scoring policy instead of guessing.
- **Cost:** Limit pages, tokens and retries; reuse saved extraction results and queue large uploads.
- **Privacy and security:** Restrict access by role, scan uploads, encrypt data in transit and at rest, and set retention limits.
