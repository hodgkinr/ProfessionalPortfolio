---
layout: page
title: Privacy Policy — Augmented Grader
nav_exclude: true
---

# Privacy Policy — Augmented Grader

**Last updated:** July 2026

Augmented Grader is a Chrome extension built for CU Boulder course staff to assist with grading in Canvas SpeedGrader. This policy explains what data the extension accesses, where it goes, and why.

## What data is accessed

When a grader uses the extension inside a Canvas SpeedGrader session, it reads:
- The student's submitted answer text for the current question, so an AI-generated draft response can be produced
- The assignment/quiz identifier, used to look up the correct grading rubric for that assignment

The extension does **not** collect, transmit, or store any student name, student ID, or other information that could identify which student a submission belongs to.

## Where the data goes

The submission text is sent, over an encrypted connection, to a backend service (AWS Lambda) that the course instructor operates. That service:
1. Sends the submission text and the relevant grading rubric to OpenAI's API to generate a draft feedback response
2. Returns the draft to the grader for review, editing, and approval
3. Records the draft, the grader's final edited version, and timing information (how long grading took, how much the grader changed) in a database, for the purpose of improving the grading tool and studying grading workflows

No submission data is shared with anyone beyond this pipeline (the instructor's backend and OpenAI, solely to generate the draft). It is not sold, shared with advertisers, or used for any purpose beyond generating and studying grading feedback.

## Grader information

Canvas's own SpeedGrader interface, not this extension, is what attributes final submitted comments to the grader who entered them. The extension itself does not separately collect or transmit grader identity.

## Data retention

Interaction records (draft, final edit, timing) are retained by the instructor for the duration of the course and subsequent research analysis. There is no separate account system — access to the extension is limited to course staff the instructor has directly distributed it to.

## Permissions this extension requests, and why

- **activeTab / scripting:** to read the submission text visible in the current SpeedGrader tab and insert the AI-drafted response into the feedback box
- **storage:** to temporarily hold a draft in the browser while the grader reviews and edits it
- **Host access to canvas.colorado.edu:** required to operate within Canvas SpeedGrader
- **Host access to the backend API:** required to request AI-generated drafts and log grading events

## Contact

Questions about this extension or its data handling can be directed to the instructor operating it, Bobby Hodgkinson (CU Boulder).
