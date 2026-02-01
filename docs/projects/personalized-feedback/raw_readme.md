## BOBPE / BUFFALO grading workflow — main README

For docker addition see: https://chatgpt.com/share/e/68d5e489-93a4-8009-9f93-38ae9ab596a9 

docker compose up -d --build
docker compose down

to run both: 
docker compose -f docker-compose.review.yml up -d --build
docker compose -f docker-compose.step3.yml up -d --build 
docker compose -f docker-compose.rebecca_math.yml up -d --build

docker compose -f docker-compose.caddy.yml up -d --build


>> ipconfig getifaddr en0  
http://10.235.142.252:8080/code_review
http://10.235.142.252:8080/concept_review
/ralphie_math

step 3 multiple choice grader runs on 5006:5003 
and with caddy /concept_review

code reviewer runs on 5004:5001
and with caddy /code_review

rebecca math runs on 5005:5002
and with caddy /ralphie_math_guide

docker compose -f docker-compose.review.yml down  
docker compose -f docker-compose.step3.yml down
docker compose -f docker-compose.rebecca_math.yml down
docker compose -f docker-compose.caddy.yml down


## To run multiple /concept_review_1 and /concept_review_2

update the .jsons in docker-compose.step3-2 to point to the right default_jsons:
    environment:
      - DEFAULTS_FILE=/app/config/PC_1_2.json
      - PYTHONUNBUFFERED=1
    command: ["python","main.py","--step","3","--quiz_id","503155"]

docker compose -f docker-compose.step3-2.yml up -d --build 
docker compose -f docker-compose.caddy2.yml up -d --build

## October 1 2025: concept quiz: AM.1
>> python main.py --quiz_id 503158 --step 1 

update 2_Evaluation/default_jsons/1030_Fa2002: AM_1_1 and AM_2_2 (this could be automated with a little thinking)
also update docker-compose.caddy2.yml to point to those jsons

(this takes about 2 minutes for an initial run)
>> python main.py --quiz_id 503158 --step 2 --skip-existing-extract --skip-existing-eval --eval-limit 5 --defaults-file ./default_jsons/1030_Fa2025/AM_1_1.json 

>> python main.py --quiz_id 503158 --step 2 --skip-existing-extract --skip-existing-eval --eval-limit 5 --defaults-file ./default_jsons/1030_Fa2025/AM_2_2.json

manually check the created _evaluated_db files OR
>> docker compose -f docker-compose.step3-2.yml up -d --build 

check localhost:5003 and localhost:5004 to make sure the AI did ok..then (might not need to bring down docker?)
>> docker compose -f docker-compose.step3-2.yml down

run again but without the --eval-limit 5 tag

python main.py --quiz_id 503158 --step 2 --skip-existing-extract --skip-existing-eval --defaults-file ./default_jsons/1030_Fa2025/AM_1_1.json 

>> python main.py --quiz_id 503158 --step 2 --skip-existing-extract --skip-existing-eval --defaults-file ./default_jsons/1030_Fa2025/AM_2_2.json

then bring up the docker again 
then bring up the caddy docker:
docker compose -f docker-compose.caddy2.yml up -d --build 

confirm IP
>> ipconfig getifaddr en0
172.25.18.117/concept_review_1
and
172.25.18.117/concept_review_2


This is useful: history | tail -20


# To return submission back. PC.1 
(grade)
run the pdf creator, and the combiner if multiple:

>>python main.py --quiz_id 503155 --step 4 --defaults-file ./default_jsons/1030_Fa2025/PC_1_2.json
>>python main.py --quiz_id 503155 --step 4 --defaults-file ./default_jsons/1030_Fa2025/PC_2_1.json
if multiple
>> python ./3_Dissemination/combine_same_named_pdfs.py --inputs 3_Dissemination/127598_output_pdfs_503155_Concept.PC.1.2 3_Dissemination/127598_output_pdfs_503155_Concept.PC.2.1 --output 3_Dissemination/127598_output_pdfs_503155_combined 
the when ready.. change the POST_GRADE in upload_comment_improved.py 
python main.py --quiz_id 503155 --step 5 --output-dir 3_Dissemination/127598_output_pdfs_503155_combined


10/3/2025 for AM.1
>>python main.py --step 4 --defaults-file ./default_jsons/1030_Fa2025/AM_1_1.json
>>python main.py --step 4 --defaults-file ./default_jsons/1030_Fa2025/AM_2_2.json
>>python ./3_Dissemination/combine_same_named_pdfs.py --inputs 3_Dissemination/127598_output_pdfs_503158_Concept.AM.1.1 3_Dissemination/127598_output_pdfs_503158_Concept.AM.2.2 --output 3_Dissemination/127598_output_pdfs_503158_combined 
>>python main.py --quiz_id 503158 --step 5 --output-dir 3_Dissemination/127598_output_pdfs_503158_combined




This document describes how to use `main.py` as a one-flag runner for the entire BOBPE/Buffalo grading workflow, and it references the key scripts used in each of the subdirectories: `0_Generation`, `1_Extraction`, `2_Evaluation`, and `3_Dissemination`.

Keep this file in the repository root as `readme_main.md`.

## Purpose and high-level workflow

This repo implements a multi-stage workflow for converting concept/quiz definitions into Canvas quizzes, extracting student responses, evaluating responses with an AI-powered evaluator, letting human reviewers refine evaluations, and producing/uploading feedback (PDFs/comments) back to Canvas.

Top-level steps (exposed through `main.py --step <n>`):
- Step 0 — Build quiz from a concept JSON (generate HTML, optionally upload to Canvas)
- Step 1 — Extraction: pull student responses from Canvas into a local DB
- Step 2 — Evaluation: run the evaluator to produce organized and evaluated databases
- Step 3 — Review: run the review UI (human review) and a queue manager in parallel
- Step 4 — Dissemination: generate PDFs for student feedback (and optionally include expert explanations / full review)
- Step 5 — Upload: push feedback (comments) back to Canvas

The single-entry point is `main.py` which aggregates arguments and invokes the appropriate scripts in subfolders.

## Prerequisites

- Python 3.11 is used by the project (code compiled with 3.11 pyc files present). Adjust for your environment.
- Install dependencies listed in `requirements.txt`:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

- Create a `.env` file in the repo root (or set environment variables) containing at least `COURSE_ID`. `CANVAS_BASE` can be set as well; otherwise a default is used.

.env example:

```text
COURSE_ID=127598
CANVAS_BASE=https://canvas.colorado.edu
```

## Where defaults live

`main.py` exposes a `DEFAULTS` dict near the top of the file for convenience. Common defaults you may find there:

- `CANVAS_BASE` — default Canvas base URL
- `JSON_PATH` — path to the concept JSON used by steps 0/2/3
- `DB_FILE`, `ORGANIZED_DB`, `EVALUATED_DB`, `REVIEWED_DB` — default DB paths used by evaluation and review
- `OUTPUT_DIR` — default output directory for generated PDFs
- `INCLUDE_EXPERT`, `INCLUDE_FULL_REVIEW` — defaults controlling PDF content inclusion

When running `main.py` you can override these defaults by passing explicit CLI flags.

## Important helper functions in `main.py`

- `quiz_url(quiz_id, dotenv_path)` — builds the Canvas quiz URL from `CANVAS_BASE` and `COURSE_ID`.
- `db_path(quiz_id, dotenv_path)` — default path for extracted quiz responses DB in `./1_Extraction/quiz_responses/`.
- `org_db_path(quiz_id, dotenv_path)` — default organized DB path in `./2_Evaluation/evaluator_dbs/`.
- `eval_db_path(quiz_id, dotenv_path)` — default evaluated DB path in `./2_Evaluation/evaluator_dbs/`.
- `html_from_json(json_path)` — helper to map a JSON file to the generated HTML path.

These are used by the step helpers in `main.py` to calculate file paths and URLs when not overridden on the command line.

## Running `main.py` — basic usage

Run the script from the repo root. `main.py` exposes a `--step` integer and a handful of common flags. The general idea is to select a quiz id once then increment the `--step` as you progress.

Example: build HTML from a concept JSON (step 0):

```bash
python main.py --step 0 --json ./0_Generation/Course_Folders/ASEN1030_Fa2025/CT.2/Concept.CT.2.2.json
```

Example: extract a quiz from Canvas (step 1):

```bash
python main.py --quiz_id 494588 --step 1
```

Example: evaluate (step 2) — evaluated DB paths and options can be overridden

```bash
python main.py --quiz_id 494588 --step 2 --skip-existing-extract --skip-existing-eval --eval-limit 5
```

Example: run review UI and queue manager in parallel (step 3)

```bash
python main.py --quiz_id 494588 --step 3
```

Example: generate PDFs (step 4)

```bash
python main.py --step 4 --include-expert --include-full-review
```

Example: upload comments to Canvas (step 5)

```bash
python main.py --quiz_id 494588 --step 5
```

Pass-through flags: any flags after a `--` are forwarded verbatim to the underlying script being invoked by `main.py`. Example:

```bash
python main.py --quiz_id 494588 --step 2 -- --some-underlying-flag value
```

## Step-by-step script references

Below are the primary scripts that `main.py` calls for each step and what they do. Use these directly for development/troubleshooting if needed.

- 0_Generation (build Canvas-ready quiz html or batch convert JSONs)
  - `json_to_canvas_html.py` — converts a concept JSON to an HTML template used to create the Canvas quiz.
  - `create_quiz_from_html.py` — (optionally) takes the generated HTML and posts/creates a quiz in Canvas.
  - `batch_json_to_html.py` — batch converts many JSONs to HTMLs.

- 1_Extraction
  - `one_stop_canvas_quiz_pull_improved.py` — preferred script to pull quiz submissions from Canvas for a specific quiz URL and store them locally in `./1_Extraction/quiz_responses/`.
  - `one_stop_canvas_quiz_pull.py` — earlier/alternate version.

- 2_Evaluation
  - `evaluator.py` — main evaluation pipeline. Inputs:
    - `--concept-path` path to the concept JSON
    - `--db_file` path to extracted quiz DB
    - `--org_db` path to organized DB (output)
    - `--eval_db` path to evaluated DB (output)
    - `--skip-existing-extract`, `--skip-existing-eval`, `--eval-limit` control behavior
  - `extract_mcq.py` — helper for multiple choice extraction.
  - `student_to_expert.py` — helper for converting student responses into the input for an expert answer generator.
  - `RPHF.py` — implements the reviewer / prompt heuristics for improving evaluator instructions (supervisor logic).
  - `queue_manager.py` — lightweight manager that can auto-run `evaluator.py` when review/evaluation gaps meet thresholds. Used by step 3.
  - `appV1.py` / `app.py` — web UI for human reviewers to inspect and modify model-generated expert answers and evaluations.

  All evaluation-related DBs and state are in `./2_Evaluation/evaluator_dbs/`.

- 3_Dissemination
  - `generate_pdfs_mcq.py` — generate feedback PDFs per student/question. Has options to include expert explanations and full review information.
  - `upload_comment.py` / `upload_comment_improved.py` — upload feedback/comments back to Canvas; `upload_comment_improved.py` is a newer version.
  - Fonts and styling for PDFs are in `3_Dissemination/Open_Sans/` and `3_Dissemination/static/`.

## File/DB naming conventions

- Extracted DBs and CSVs: `./1_Extraction/quiz_responses/{COURSE_ID}_quizzes_{QUIZ_ID}.db` and `.csv`.
- Organized/evaluated/reviewed DBs: `./2_Evaluation/evaluator_dbs/{COURSE_ID}_organized_{QUIZ_ID}.db`, etc.
- Generated PDFs output directory default: `./3_Dissemination/output_pdfs/` or per-run `OUTPUT_DIR` from `DEFAULTS`.

## Common flags and options (overview)

- `--quiz_id` — the Canvas quiz id used to build URLs and db paths
- `--json` or `--concept-path` — the path to the concept JSON that defines questions and expert expectations
- `--org_db`, `--eval_db`, `--reviewed_db`, `--db_file` — explicit DB paths to override defaults
- `--skip-existing-extract` — avoid re-extracting when DB already exists
- `--skip-existing-eval` — skip re-evaluating items already evaluated
- `--eval-limit` — number of evaluations to run in one batch
- `--include-expert` / `--include-expert-explanations` — include AI expert text in generated PDFs
- `--include-full-review` — include full human review history in generated PDFs

Refer to the top of `main.py` and the `evaluator.py`/`generate_pdfs_mcq.py` scripts for exact flag names and semantics.

## Troubleshooting & tips

- If a step can't find a DB, verify `COURSE_ID` in `.env` or pass `--db_file` explicitly.
- If Canvas requests fail, confirm `CANVAS_BASE` and `COURSE_ID` and that any required Canvas access tokens or authentication are configured in the environment or credential files used by the extraction scripts.
- For flaky evaluation runs, increase logging in `evaluator.py` and inspect `./2_Evaluation/evaluator_dbs/` for partial outputs.
- When running step 3, `main.py` launches `queue_manager.py` and `appV1.py` in parallel. Close the UI to stop the queue manager; check process logs if the QM keeps running.

## Development notes and possible improvements

- Automate environment creation (venv/conda) and pin dependencies in `requirements.txt`.
- Add small unit tests for helper functions in `main.py` (path builders, URL builders) to prevent regressions.
- Add more robust error reporting and a graceful shutdown for the queue manager / UI pairing.
- Consider a `--dry-run` mode in `main.py` to print the commands that would be run without executing them. (Parts of `main.py` already accept `dry_run` flags.)

## Where to look for more details

- `main.py` — primary orchestrator and authoritative source of the CLI interface and default values.
- `2_Evaluation/evaluator.py` — core evaluation logic; read header comments and argument parsing to see full behavior.
- `3_Dissemination/generate_pdfs_mcq.py` — PDF generation options and templates.

## Quick checklist — mapping user request to artifact

- Requested: "very detailed readme file for the overall operation starting with main.py and referencing relevant scripts" — Done: `readme_main.md` created in repo root.

## Final notes

Open `readme_main.md` for examples and actionable commands. If you'd like, I can:

- Add a short quickstart script that sets up a venv and runs step 1 for a test quiz.
- Expand `readme_main.md` with screenshots or a diagram of data flow.
- Auto-generate a smaller `README` targeted at instructors that only shows the minimal set of commands to grade a single assignment.

---
Generated by repository tooling guidance; update the examples above if your environment or file layout changes.
