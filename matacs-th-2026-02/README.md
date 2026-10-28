# MATACS Associate Software Developer Assessment

**Candidate:** Muhammad Hammad  
**Reference:** MATACS-TH-2026-02  
**Time spent:** 50 minutes

## Overview
This repository presents a design for an AI-assisted proposal triage tool for commercial solar installation proposals. AI extracts facts and evidence from uploaded documents. Ordinary application code checks mandatory requirements, applies a fixed scoring rubric, calculates weighted totals, and sorts eligible proposals. A human reviewer verifies the evidence and makes the final award decision.

## Repository contents
- [Process flow diagram](diagram/process-flow.pdf)
- [Design write-up](WRITEUP.md)
- [AI prompts](PROMPTS.md)
- [Expected ranking](ranking.json)

## AI use
I used ChatGPT to assist with designing the AI-assisted proposal triage workflow, developing the evaluation and scoring methodology, preparing the documentation, and structuring the final assessment deliverables.

AI helped me organize the mandatory eligibility checks, define weighted evaluation criteria, compare the sample proposals, and develop a consistent approach to ranking eligible submissions. I also used AI to identify potential risks, including prompt injection, missing information, inconsistent scoring, and the need for human oversight.

One important design decision was to ignore the instruction embedded in Proposal C that attempted to influence the automated ranking. Instead, the proposal is evaluated using the predefined criteria and supporting evidence only.

The final design combines rule-based eligibility checks with weighted scoring, explainable evaluation results, human review for flagged proposals, and audit logging. AI was used to support the design process, while the proposed system keeps evaluation decisions transparent, consistent, and subject to human oversight.

## Assumptions
- A bidder must state that it holds the Ontario electrical contractor licence; an unconfirmed subcontractor does not satisfy that requirement.
- The 16-week completion limit is treated as a project constraint. A missing schedule is flagged for human clarification rather than guessed.
- Scores are illustrative and use the rubric described in `WRITEUP.md`; a production system would have the administrator configure and version the rubric for each category.
