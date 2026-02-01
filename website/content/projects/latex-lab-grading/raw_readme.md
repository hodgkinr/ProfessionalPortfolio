# LaTeX AI Grader Platform

This repository contains an AI-assisted grading system for LaTeX-based STEM assignments.  
It supports automated evaluation with multiple AI models, human review workflows, and Canvas integration.

---

## 🔍 Overview

The platform is designed around a two-database architecture:

### **1. `primary.db` (global)**
Stores:
- Canvas student IDs  
- Submission metadata  
- Sanitized LaTeX  
- PDF render paths  
- Human review decisions  
- Assignment linkage  

### **2. `evaluations.db` (global)**
Located at:

data/ai_outputs/evaluations.db


Contains:
- Multiple model evaluations per student (scoped by course_id + assignment_id)  
- Raw + structured AI outputs  
- Metadata (model version, parameters, timestamps)  
- Human reviews (linked to evaluations)

This separation makes the system scalable, modular, and research-friendly.

---

## 🗂️ Directory Structure

```

latex_grader/
├── config/
│   ├── settings.yaml
│   ├── db_config.yaml
│   └── prompts/
│       ├── expert/
│       ├── personality/
│       └── questions/
│
├── data/
│   ├── incoming/
│   ├── sanitized/
│   ├── compiled_pdfs/
│   ├── logs/
│   └── ai_outputs/  # per-assignment evaluation DBs
│
├── evaluator/
│   ├── prompt_builder.py
│   ├── evaluator_openai.py
│   ├── evaluator_ollama.py
│   ├── dual_evaluator.py
│   └── overseer_evaluator.py   # future expansion
│
├── preprocessor/
│   ├── strip_preamble.py
│   ├── sanitize_tex.py
│   ├── compile_pdf.py
│   └── utilities.py
│
├── reviewer_ui/
│   ├── app.py
│   ├── templates/
│   └── static/
│
├── scripts/
│   ├── run_all.py
│   ├── seed_prompts.py
│   ├── export_results.py
│   └── upload_canvas.py
│
├── tests/
├── requirements.txt
├── LICENSE
└── README.md

```

---

## ⚙️ Workflow

1. **Submission Ingestion**  
   Student .tex → PII removed → sanitized → PDF compiled  

2. **AI Evaluation**  
   - Multiple evaluator models run (OpenAI, Ollama, etc.)  
   - Results stored in the assignment-specific evaluation DB  

3. **Human Review UI**  
   - Left: rendered student PDF  
   - Right: editable AI-generated feedback  
   - Save corrected results into `evaluations.db`

4. **Export & LMS Integration**  
   Scripts can export summaries or upload results to Canvas.

---

## 🛠 Setup

python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

Make sure to have tectonic installed on the system (for macos)
```

brew install tectonic

```

Install dependencies:
```

pip install -r requirements.txt

```

For testing:
#make some sample student .tex files (data/incoming/raw_submissions_XXX_YYY) and expert files (config/prompts/expert)
use chatgpt to make these files..replace with actual student files when they come in..
#then run the sanitize_and_ingest.py:
python ./preprocessor/sanitize_and_ingest.py --course 123456 --assignment 987654 

preprocessor/santizie_and_ingest.py will create data/primary.db 

#if the question jsons aren't created but instead there's just a question .tex and an expert .tex then create the json
python scripts/build_question_json.py --course 123456 --assignment 987654

#test the prompt builder by adjusting the sample files and the assignment
python -m scripts.test_prompt_builder

#copy that into your favorite chatbot and see what it says 

#test the openai call using a sample submission 
python -m scripts.test_openai_eval 

#run a batch process of everything with run_all.py (see run_all.py for more information on the tags)
python ./scripts/run_all.py \
        --course 123456 \
        --assignment 987654 \
        --model gpt-4.1-mini --progress off --run_some 2

# Moving on to the reviewer stage
python -m reviewer_ui.app

#then go to: 
http://localhost:5001/review/123456/987654/456780

# Next steps

1. add in the ability to review with but ollama, and gemini would be good as well. also a summarizer would be nice to have too, not sure how to do that - this might be overseer_evaluator. So probably just need to build a script that compiles all of that, judges the models, and provides and update overseer evaluation.

2. also it would be nice to consider some filter mechanism for multiple graders on the reviewer_ui.app. This could be as easy as a list of graders and how many submissions they'll grade. 
Also a status bar of how many submissions are left to grade.

# Longer term to make this more intergrated with Canvas. 
Create a full pipeline, where submissions are pulled from canvas into raw_submissions_course_assignment. then run the sanitize and ingestion, build question and prompt, then run_all.py, then the reviewer, then the graders do their thing, then push the results back to canvas. 

## Notes:
 1. Assume FIELD_ORDER and FIELD_TYPES in review.js are the same as given in /config/prompts/personality/base.json in the "output_format_xml"
2. To compile the questions to a pdf from just a basebones .tex file:
chmod +x compile_fragments.sh
./compile_fragments.sh ./config/prompts/expert
./compile_fragments.sh ./config/prompts/questions


---

## 📌 License

See the included `LICENSE` file.

---

This project is designed to be modular, scalable, and suitable for large courses or research experiments involving multiple AI models.