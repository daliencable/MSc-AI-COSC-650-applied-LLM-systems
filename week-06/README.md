# Week 5: RAG Pipeline with Retrieval Evaluation

A retrieval-augmented generation (RAG) pipeline over 8 diabetes care and prevention PDFs. It cleans the extracted text, compares three chunking configurations with chunk-level precision and recall, and generates grounded answers with Gemini.

Notebook: `week5_RAG_Dalien_Cable.ipynb`

## What it does
1. Loads 8 PDFs and removes repeated headers, footers, and page numbers before chunking.
2. Chunks the text three ways: small fixed-size (120/20), large fixed-size (320/40), and whole sentences packed up to 500 characters.
3. Embeds the chunks locally with `all-MiniLM-L6-v2` and stores them in a FAISS index.
4. Scores retrieval on 10 test questions, with relevance decided in advance: a chunk is relevant if it contains the question's fact.
5. Sends the top 3 chunks to Gemini (`gemini-3.5-flash`) with an instruction to answer only from that context.

## Results (k = 3)

| Config | Precision | Recall | Answer found in top 3 |
|---|---|---|---|
| small (120/20) | 0.17 | 0.17 | 4/10 |
| large (320/40) | 0.13 | 0.04 | 3/10 |
| by-sentence | 0.13 | 0.06 | 2/10 |

Small chunks retrieved best, because a short chunk holding one fact matches a specific question closely. When retrieval missed, Gemini sometimes gave a confident wrong answer and sometimes said the answer wasn't in the context. The main failure was a question written in everyday language that missed an answer written in clinical terms. Rewriting the question in the document's language would likely fix it.

## How to run
The PDFs are not included in this repo, out of respect for the publishers' copyright. To run the notebook:

1. Download the 8 documents listed below and save them in a `corpus` folder next to the notebook, using the file names shown.
2. Create a `.env` file next to the notebook containing `GEMINI_API_KEY=your-key`.
3. Install the libraries:
   `pip install sentence-transformers faiss-cpu openai pypdf python-dotenv`
4. Run all cells.

| File name | Document | Publisher |
|---|---|---|
| cdc_174262_DS1.pdf | Newer Pharmacologic Treatments in Adults With Type 2 Diabetes | American College of Physicians, via CDC Stacks |
| dprp-standards.pdf | Diabetes Prevention Recognition Program Standards | CDC |
| dsa509.pdf | Management of Diabetes Mellitus: Standards of Care and Clinical Practice Guidelines | WHO Regional Office for the Eastern Mediterranean |
| guidance-on-global-monitoring-for-diabetes.pdf | Guidance on global monitoring for diabetes prevention and control | WHO |
| HPDP_Diabetes_guide_for_diabetes_care.pdf | Diabetes Care Guidelines | Vermont Department of Health |
| On-your-way-to-preventing-type-2-diabetes.pdf | On Your Way to Preventing Type 2 Diabetes | CDC |
| Optimal_Diabetes_Care.pdf | Optimal Diabetes Care specifications | MN Community Measurement |
| who-ucn-ncd-20.1-eng.pdf | HEARTS-D: Diagnosis and management of type 2 diabetes | WHO |