# spoiler-check results (task v0.5: divine-class abilities and Act 3-only items mixed in)

Each model got only `script.docx` and the task instruction, with web access and Python (`python-docx`), and wrote `script_act1.docx`. Each folder holds that output, `score.json` (score, diagnostics and the exact error spans) and `reward.txt`.

These were run as Claude Code subagents in a shared sandbox, not through `harbor run`. The golden doc, `sentences.json` and `build_case.py` were moved out of the repo during the runs and the agents were told not to read outside their folder.

Score = `1 - errors / 1128` (1,128 = struck words in the golden doc). Accuracy = share of all 1,766 words with the right strike status.

| Model | Score | Accuracy | Spoilers caught | Struck words that were right | Tool calls |
|---|---|---|---|---|---|
| Opus   | **1.000** | 100.0% | 100% | 100% | 17 |
| Fable  | 0.983 | 98.9% | 98% | 100% | 16 |
| Haiku  | 0.476 | 66.5% | 60% | 83% | 13 |
| Sonnet | 0.361 | 59.2% | 57% | 73% | 3 |

No model added, deleted or reworded any text.

## Errors

- **Opus** made no errors. It looked up the divine classes and god-trial rewards, struck every later-act passage, and this time also struck the veiled "far less human" Dagda line.
- **Fable** made one error: it left the veiled Act 2 line "One ruler at the heart of this story turns out to be far less human than his title suggests." visible, as in the last two rounds.
- **Haiku** kept many class and item sentences as act-neutral gameplay (squads, Kalla's Needle and others), and struck Act 1 prologue lines because it placed Eshmel, Hong Hua and Troy in Act 3.
- **Sonnet** did almost no research (3 tool calls). It kept most class and item sentences, including the War Monk and the Shining Spear, and struck whole mixed sentences plus Act 1 opening lines such as Fortuna sending Eshmel back in time.

Earlier cases: `../spoiler-check-v0.4/`, `../spoiler-check-v0.3/`, `../spoiler-check-v0.2/`, `../spoiler-check-v0.1/`.
