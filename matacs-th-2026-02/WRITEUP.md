# AI Proposal Triage Design

## Proposed process
1. A reviewer uploads proposal PDFs and selects the category.
2. The application checks file type/size, stores the original securely, and extracts text. OCR is used only when a PDF has no usable text.
3. AI extracts facts into a fixed JSON schema and includes short evidence excerpts. The document is treated as untrusted input.
4. Code validates the JSON, checks mandatory requirements, identifies missing/conflicting values, and flags uncertain cases.
5. Code applies the category's versioned scoring rubric and weights, calculates totals, and sorts eligible proposals.
6. The reviewer checks evidence and flags, corrects extraction errors if needed, and confirms the ranked list. A human makes the final award decision.

## 1. AI vs. rules
I would use AI to extract proposal facts from unstructured text and draft concise evidence-based explanations. I would use normal code for upload validation, PDF text extraction/OCR routing, mandatory pass/fail checks, score calculation, sorting, versioning, and audit logs. Rules and arithmetic are more predictable than asking a language model to calculate scores or decide eligibility. AI output is untrusted until validated.

## 2. Consistency and explainability
Each category stores its criteria, weights, scoring rubric, and version. For this sample, each weighted criterion receives a score from 0 to 100, then the application multiplies it by its weight and adds the results. The same extracted facts, rubric version, and code therefore produce the same total and ordering. The reviewer sees the overall score, each criterion score, the evidence excerpt, and any missing information. AI is not allowed to alter the numeric score.

### Illustrative scoring rubric used for this sample
- **Experience (30%)**: 12 or more qualifying projects = 100; 9–11 = 80; 6–8 = 60; 1–5 = 30; none/unknown = 0 or flag if unknown.
- **Price (25%)**: at or below CAD 200,000 = 100; CAD 200,001–220,000 = 90; CAD 220,001–240,000 = 80; CAD 240,001–260,000 = 70; above budget = fail/flag according to the configured rule.
- **Timeline (15%)**: 10 weeks or less = 100; 11–12 = 90; 13 = 85; 14 = 80; 15–16 = 70; over 16 = does not meet the project constraint. Unknown timeline = flag; do not guess.
- **Warranty (15%)**: panel warranty earns 50 points for 25 years; workmanship warranty earns 50 points for 10 years or more, 40 for 8–9 years, 30 for 5–7 years, and proportionally less below that. Cap the criterion score at 100.
- **Team and approach (15%)**: 25 points each for a named project lead, named/defined crew, safety plan, and monitoring plan.

The rubric above is an explicit sample assumption so the example is reproducible; an administrator would configure and version the rubric rather than hard-code it. Rounded totals: A = 94, E = 78, C = 76. These illustrative scores are not a substitute for a reviewer checking the extracted evidence.

## 3. Human in the loop
The reviewer confirms the category and checks extracted evidence, especially licence, WSIB, insurance, price, and schedule. Missing or conflicting information is flagged for clarification. The reviewer can correct extracted fields or override a result with a reason. The audit log records uploader/reviewer, timestamps, original and corrected values, evidence, criterion scores, total score, criteria and prompt/model versions, and override reasons. The final award decision remains with a person.

## 4. Changing criteria
Criteria are stored as data, not hard-coded. Each admin edit creates a new version with an effective date and audit record. Past results retain the version used at the time. If a reviewer requests re-scoring, all proposals in that comparison set are recalculated using the same new version, and both old and new results remain visible.

## 5. Risks and safeguards
- **Wrong extraction or OCR:** retain the source document, show evidence excerpts, validate data types, and require human confirmation for low-confidence or conflicting fields.
- **Made-up facts:** require evidence for each extracted value; represent missing facts as `unknown`, never infer them.
- **Prompt injection/manipulation:** treat document text as untrusted data and ignore instructions that attempt to change the system or ranking. Proposal C's embedded ranking instruction is ignored.
- **Inconsistent scores:** use a fixed, versioned rubric and deterministic code for arithmetic and sorting.
- **Bias or unsuitable criteria:** require transparent admin-defined criteria, review them for job relevance, and test them against representative examples.
- **Budget/schedule risk:** check the budget ceiling and 16-week project constraint in code; flag missing schedule evidence and do not assume compliance.
- **Privacy and cost:** restrict access, limit file sizes and retention, monitor model usage, and use only an approved AI service for confidential proposals.

## Sample outcome
A ranks first with strong experience, price within budget, a 12-week schedule, and a complete team/monitoring plan. E ranks second with an in-budget price, a 13-week schedule, and strong team and warranty details. C ranks third; the embedded instruction to rank it first is ignored. B is ineligible because the proposal does not confirm that the bidder itself holds the required Ontario electrical contractor licence. D is flagged because its schedule is undetermined, so compliance with the 16-week project limit cannot be verified. A reviewer should verify the evidence before relying on this illustrative result.
