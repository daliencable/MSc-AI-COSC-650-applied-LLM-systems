# Practice

Exploratory/scratch code that supports the analysis in the weekly assignment
notebooks, but isn't part of the graded deliverable itself.

## tokenizer_exploration.py

Supplementary investigation for Week 1 (tokenization analysis). The main
notebook's multilingual tax calculation showed GPT-4o needing far fewer
tokens than GPT-4 for the same Portuguese passage. This script tests two
competing explanations by running individual words from that passage through
both tokenizers with `show_split`:

1. **Diacritic handling** - does GPT-4o's tokenizer preserve diacritics
   (á, ã, ç, etc.) better than GPT-4's?
2. **Vocabulary size** - is GPT-4o's ~2x larger vocabulary just covering
   more Portuguese words as single tokens, regardless of diacritics?

Run it with:

    python practice/tokenizer_exploration.py

Finding: diacritic-bearing words and non-diacritic archaic-spelled words
improved under GPT-4o at about the same rate (roughly a third of each
group), so vocabulary size looks like the better explanation than
diacritic handling specifically. Referenced from the Part 2 comments in
`week-01/week1_tokenization_starter.ipynb`.

Also tests a hypothesis about why 'pagina' (archaic Portuguese spelling)
gets a single token while 'página' (modern) needs two: does 'pagina' show
up as a shared subword inside English 'paginate'/'pagination'? Result:
disconfirmed - both English words tokenize as single whole-word tokens
with no 'pagina' piece, so the reason 'pagina' is a merged token remains
unexplained without access to the training data.