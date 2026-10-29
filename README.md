# AI Proposal Triage Design

- Full name: **Muhammad Bilal**
- Reference: MATACS-TH-2026-02
- Time actually spent: **2.5 hours**

This is my design for a tool that helps reviewers compare contractor proposals. It extracts the relevant details, checks the requirements, and produces a ranked list with reasons. A person makes the final award decision.

## Files

- [Design and answers](WRITEUP.md)
- [Extraction prompt](PROMPTS.md)
- [Expected ranking](ranking.json)
- [Process diagram](diagram/process.png)

## AI use

I used ChatGPT to help draft and revise the design, prompt and diagram, and to check the sample calculations. During review, I supplied analysis supporting a change to its initial suggestion of giving no experience points where project dates were unclear. The revised design counts that experience provisionally, records the assumption, and requires date verification before award.

## Sample result

A ranks first, E second and C third under the scoring rules in the write-up. Their experience scores depend on the stated assumption about project dates. C's result assumes a reviewer has checked and dismissed its instruction to manipulate the ranking. D needs a confirmed schedule, while B does not meet the requirement to state that the bidder holds the licence.
