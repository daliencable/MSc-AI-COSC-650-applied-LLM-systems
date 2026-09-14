# Week 3: Prompts as Engineering Artifacts

Support ticket triage for a connected diabetes management app (CGM sensor + a smart pen cap that detects removal/replacement from an insulin pen to log a treatment). Classifies a ticket into one of six categories - device, treatment, lifestyle, supply, account, caregiver - and assigns a severity level (P1-P4).

## Contents

- `prompts/diabetes_prompt.md` (v1) - system framing, six category definitions, seven few-shot examples, a plain P1-P4 severity table. Severity is scored by reading the table directly against the ticket's stated problem.
- `prompts/diabetes_prompt_expanded.md` (v2) - identical categories and severity tiers, plus one added instruction: score severity by the worst plausible downstream consequence to the patient, not just the literal complaint. P1's definition is also expanded to cover cases where a malfunction causes the patient to change their behavior in a way that undermines their own safety net. One added few-shot example demonstrates this (a silently dropped dose log, correctly flagged P1).
- `week3_prompt_engineering.ipynb` - the 10-case test suite, run twice live against Gemini, plus the full tradeoff and failure write-up in the markdown cells at the bottom of the notebook.

## Summary

The write-up lives directly in the notebook, not in a separate file - the analysis references specific run outputs printed earlier in the same notebook, so it's easier to follow alongside the code that produced it.

v1 and v2 use the same six categories and the same severity tiers. v2 adds one instruction on top of v1: score severity by the worst plausible downstream consequence, not just the literal complaint.

Severity went from 50% to 80% both times. v1 keeps missing the same pattern both runs, a ticket that sounds routine on the surface but implies something worse underneath. v2 catches more of those, consistently, not just once.

The CGM transmitter died completely. Run 1, v1 called it device and v2 called it supply. Run 2, both called it device. The category flip didn't hold up so that was noise, not a real weakness in v2. The severity call did hold up though, and it was wrong both times. v1 scored this P2 instead of P1, in both runs, and v2 made the same mistake both times. v1's own table lists complete loss of critical alerts under P1, and total transmitter death fits that description. My downstream rule didn't catch it either.

I assumed v1 would miss the danger in the sleep-disruption ticket (#2) and the account lockout ticket (#9), scoring them too low, and that v2's downstream rule would catch them. Live, v1 already scored both correctly, P1 and P2. The model reasoned about the consequence on its own even without my instructions telling it to. That demonstrates that a capable model already does some of this thinking by default, and my literal only v1 prompt didn't stop it from happening the way I had predicted.

The cracked pen cap ticket went from P4 under v1 to P3 under v2, and I didn't build this case to test that. Finding a real, unplanned severity bump is better evidence that the downstream rule generalizes vs either of my hand-picked cases.

My fixture-based predictions were built around a hypothesis that turned out to be partly wrong. If I'd submitted on fixtures, I would have written a research note about two escalation cases that live aren't escalations at all, confirmed across two separate runs. I also would have missed that both versions get #3's severity wrong the same way, since my fixtures assumed even v1 would obviously score total transmitter failure as P1. Real model output surfaced a shared blind spot I hadn't thought to predict, and disproved part of what I assumed going in.

## Running it

The notebook runs end-to-end without any API key: `LIVE` evaluates to `False` and every model call falls back to labeled fixtures.

To run it live instead, set `GEMINI_API_KEY` as an environment variable before launching Jupyter, and make sure `google-genai` is installed (not `openai` - Gemini's newer Auth-key format requires the native SDK):

```bash
pip install google-genai
set GEMINI_API_KEY=your-key-here
jupyter notebook
```

No key value is committed anywhere in this repo.
