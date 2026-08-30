# Week 1: Tokenization Analysis

Measures the "multilingual tax" between English and European Portuguese
using OpenAI's tiktoken library - no API key or GPU required.

## Contents

- `week1_tokenization_starter.ipynb` - the analysis notebook (run top to
  bottom in Jupyter or Colab)
- `week1_tokenization_starter.html` - exported copy submitted to Canvas

## Passage

~115-word excerpt from Fernando Pessoa's *Livro do Desassossego* ("Gósto
de dizer..."), written in pre-1911 Portuguese orthography. Public domain
(Pessoa died in 1935). English translation drafted with Claude, cross-checked
against Google Translate, and verified by me as a Portuguese speaker.

## Headline findings

- Multilingual tax: 1.21x under GPT-4 (cl100k_base), 1.05x under GPT-4o
  (o200k_base) - most of the gap closes with the newer tokenizer's larger
  vocabulary.
- A 128,000-token context window fits 735 English copies of the passage
  vs. 609 Portuguese copies.
- Three bias pairs (não/no, palavrar/word, engenharia/engineering) show
  the tokenizer's English-corpus bias directly.
- Failure case: the archaic spelling "pagina" costs fewer tokens than the
  modern, correctly accented "página" - token cost doesn't track
  linguistic correctness.

See `practice/tokenizer_exploration.py` for supporting investigation into
why GPT-4o compresses Portuguese better than GPT-4.

## Running it

Requires Python 3 and `tiktoken` (`pip install tiktoken`). Open the
notebook in Jupyter or Colab and run all cells in order.
