# CLAUDE.md

## Project
Course repository for COSC 650: Applied LLM Systems (Maryville University).
8-week graduate course covering tokenization, transformer architecture,
prompt engineering, function calling, retrieval-augmented generation,
fine-tuning, and evaluation.

## Structure
- week-01/ through week-08/ : weekly assignments and notebooks
- notes/ : research notes and reading annotations
- practice/: code testing and practice for pre-assignment work
- project/ : final project code and documentation
- CLAUDE.md : this file
- README.md : human-facing project description

## Conventions
- Notebooks are saved from Google Colab via File > Save a copy in GitHub and also run locally in Jupyter
- All code is Python 3.11+
- Both OpenAI and Anthropic SDKs may be used depending on the assignment
- tiktoken is used for tokenization experiments
- Commits use descriptive messages, not "update" or "fix"

## Do Not
- Delete files or directories without confirming first
- Push to main without checking what is staged
- Commit API keys or any file in .env

## Week 2 — Transformer Inference and Sampling Controls

Used Claude for: notebook scaffolding, debugging a kernel crash on setup (OMP library conflict between numpy and torch, fixed with KMP_DUPLICATE_LIB_OK), implementation guidance for temperature/top-k/top-p sampling functions, GitHub workflow (branch, PR, linked issue).

My own work: choosing the domain-relevant test prompt, predicting each sampling setting's behavior before running it, identifying two genuine findings in Part 4 (top-p=1.0 doesn't guarantee full vocabulary retention once temperature collapses the distribution onto one token, and a floating-point off-by-one bug reporting 50258 tokens kept when only 50257 exist), and the failure case write-up and mitigation.

## Week 3 — Prompt Engineering (Diabetes Support Triage)

Used Claude for: JSON schema structure, boilerplate prompt scaffolding (system message format, few-shot framing), notebook code (metrics functions, test harness, fixture wiring), debugging live API integration (model name, SDK compatibility with Gemini's newer key format, JSON parsing around reasoning text).

My own work: the six-category taxonomy (device, treatment, lifestyle, supply, account, caregiver) and where to draw the lines between them, all category definitions, all example tickets and their category/severity assignments (drawn from real product concerns from my time at Bigfoot Biomedical), the severity framework (adapted from a clinical ticket severity matrix I use professionally), the downstream-consequence rule for v2, and all analysis and conclusions in the notebook's write-up, including catching that a result didn't reproduce across two live runs and correcting my own initial conclusion.

## Week 4: Multi-Tool Assistant

AI tool used: Claude (Anthropic), chat interface.

What Claude helped with:
- Scaffolding the live tool-calling loop (message/tool_call/tool_result cycle) against the Gemini OpenAI-compatible endpoint.
- Debugging: diagnosed a Windows multiprocessing hang in the guarded run_python timeout and suggested switching to a ThreadPoolExecutor-based timeout instead.
- Suggested query phrasings to stress-test schema edges (invalid enum values, ambiguous units) while evaluating the tools.
- Reviewed draft write-up text (PR description, this file, README) for clarity.

What stayed mine:
- Domain choice (diabetes glucose data) and all three tool schemas.
- The synthetic patient data and glucose classification thresholds (sourced and verified: Battelino et al. 2019, Diabetes Care).
- Running the actual evaluation queries and identifying which one produced a real function-calling failure to document (issue #7).
- Analysis and conclusions in the PR description and README.
