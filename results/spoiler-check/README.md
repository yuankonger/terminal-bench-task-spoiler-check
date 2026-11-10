# spoiler-check results (task v0.2: scrambled one-act sentences)

Each model got only `script.docx` and the task instruction, with web access and Python (`python-docx`), and wrote `script_act1.docx`. Each folder holds that output, `score.json` (score, diagnostics and the exact error spans) and `reward.txt`.

These were run as Claude Code subagents in a shared sandbox, not through `harbor run`. The golden doc, `sentences.json` and `build_case.py` were moved out of the repo during the runs and the agents were told not to read outside their folder.

Score = `1 - errors / 345` (345 = words in the 20 struck Act 2/3 sentences). Accuracy = share of all 562 words with the right strike status.

| Model | Score | Accuracy | Spoilers caught | Struck words that were right | Sentences wrong |
|---|---|---|---|---|---|
| Fable  | **1.000** | 100.0% | 100% | 100% | 0 |
| Sonnet | **1.000** | 100.0% | 100% | 100% | 0 |
| Opus   | 0.933 | 95.9% | 100% | 94% | 1 |
| Haiku  | 0.896 | 93.6% | 96% | 94% | 2 |

No model added, deleted or reworded any text.

## Errors

- **Opus** struck the Act 1 sentence "The progenitor god Sothis summons a single soul, the Mediator Eshmel ...". It found a source saying who Eshmel really is gets revealed in Part III, and struck it to be safe.
- **Haiku** left the Act 3 sentence "The Temple of the Diadem opens and grants the heroes access to divine classes." visible, and struck the Act 1 sentence "Fortuna sends Eshmel five years into the past ..." as a twist.

Results for the original hand-written script are in `../spoiler-check-v0.1/`.
