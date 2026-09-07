# Week 2: Inference and Sampling

## What this covers

Two stages of generation: the forward pass that turns a prompt into a next-token probability distribution, and the sampling step that turns that distribution into a choice.

- **Part 1** — Trace a real forward pass on `distilgpt2` (CPU, no API key): token IDs, embedding shape, attention weights for one layer/head, logits shape, top-10 next tokens.
- **Part 2** — Implement and visualize temperature, top-k, and top-p filtering over the model's real next-token distribution.
- **Part 3** — Evaluate three sampling settings with real numbers: entropy, max probability, tokens kept by nucleus sampling. Predictions stated before running.
- **Part 4** — One failure case with an explanation and mitigation.

## Environment

- `transformers`, `torch` (CPU), `numpy`, `matplotlib`
- No GPU, no API key
- ~350MB model download on first run

## Files

- `week2_inference_sampling.ipynb` — main notebook

## AI tool use

See `CLAUDE.md` (or equivalent agent context file) in the repo root for notes on how AI tools were used for this assignment.
