# spoiler-check v0.3 results (mixed-act, foreshadowing and veiled sentences)

Each model got only `script.docx` and the task instruction, with web access and Python (`python-docx`), and wrote `script_act1.docx`. Each folder holds that output, `score.json` (score, diagnostics and the exact error spans) and `reward.txt`.

These were run as Claude Code subagents in a shared sandbox, not through `harbor run`. The golden doc, `sentences.json` and `build_case.py` were moved out of the repo during the runs and the agents were told not to read outside their folder.

Score = `1 - errors / 380` (380 = struck words in the golden doc). Accuracy = share of all 777 words with the right strike status.

| Model | Score | Accuracy | Spoilers caught | Struck words that were right |
|---|---|---|---|---|
| Sonnet | **0.974** | 98.7% | 100% | 97% |
| Fable  | 0.950 | 97.6% | 95% | 100% |
| Opus   | 0.950 | 97.6% | 95% | 100% |
| Haiku  | 0.903 | 95.2% | 100% | 91% |

No model added, deleted or reworded any text. Every model struck the spoiler half of every mixed sentence and kept all six foreshadowing lines.

## Errors

- **Sonnet** struck the Act 1 tail "the same city she first walked into thirsting for revenge." of the Dagsion escape sentence.
- **Fable** and **Opus** both left the veiled Act 2 spoiler "One ruler at the heart of this story turns out to be far less human than his title suggests." visible, reading it as a harmless teaser.
- **Haiku** struck three Act 1 clauses as spoilers (Fortuna's time travel, "The survivors of that ruined future have no army to speak of,", "Half a decade before that catastrophe,") and the joining words "though" and "but" at two mixed-sentence boundaries.

Task version 0.4.0 replaced this case. Earlier cases: `../spoiler-check-v0.2/` (scrambled one-act sentences) and `../spoiler-check-v0.1/` (original hand-written script).
