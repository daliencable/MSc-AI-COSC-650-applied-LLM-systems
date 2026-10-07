# Week 6: Advanced Retrieval, Transform and Measure

This week extends my Week 5 RAG pipeline on the diabetes documents. I added two query transformations (pseudo-relevance feedback and HyDE) and a MedCPT re-ranker, then measured what each one changed on the same 10 questions.

## What's in this folder

- `week6_RAG_Expanded_Dalien_Cable.ipynb`: the full notebook, with the write-ups in markdown cells

## Setup

- 8 diabetes PDFs (CDC, WHO, ACP), cleaned and split by sentence into 1,209 chunks
- Embeddings: `all-MiniLM-L6-v2`, searched with FAISS
- 10 questions, with the answer fact decided in advance. A chunk counts as relevant only if it contains that fact exactly
- HyDE passages are written by Gemini (`gemini-3.5-flash`) at temperature 0
- Re-ranker: `ncbi/MedCPT-Cross-Encoder` over the top 30 results

## Results

Mean precision@3 across the 10 questions:

| Pipeline | Mean P@3 | Improved / Regressed / Same |
|---|---|---|
| Baseline | 0.13 | n/a |
| PRF | 0.17 | 3 / 4 / 3 |
| HyDE | 0.27 | 7 / 1 / 2 |
| Re-rank | 0.20 | 3 / 2 / 5 |
| HyDE + re-rank | 0.20 | 6 / 2 / 2 |

HyDE helped most, especially on questions written in everyday language. The re-ranker only reorders the top 30, so it can't rescue an answer ranked below that. PRF struggled because the baseline top 3 was wrong for 8 of the 10 questions, so it copied vocabulary from the wrong chunks.

## Failure case

The sessions question got worse under HyDE. The first chunk containing the answer dropped from rank 14 to rank 67. Gemini's passage wandered into weight loss targets and maintenance, and the real answer is a short requirements row that reads nothing like flowing prose. The full explanation is in Part 3 of the notebook.

## Running it

The PDFs are not committed, so the notebook won't run end to end from a fresh clone. To rerun it you need:

- the 8 PDFs in a local `corpus/` folder
- a Gemini API key in a `.env` file

HyDE passages are regenerated on each full run, so the numbers can shift slightly from the ones above.
