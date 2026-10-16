# spoiler-check results

Each model got only `script.docx` and the task instruction, with web access and Python (`python-docx`), and wrote `script_act1.docx`. Each folder holds that output, `score.json` (score, diagnostics and the exact error spans) and `reward.txt`.

These were run as Claude Code subagents in a shared sandbox, not through `harbor run` (no Docker network or API keys there). The golden doc was moved out of the repo during the runs and the agents were told not to read outside their folder.

Score = `1 - errors / 165` (165 = words struck in the golden doc). Recall = share of golden-struck words the model struck; precision = share of the model's struck words that the golden doc also strikes.

| Model | Score | Recall | Precision | F1 | Missed words | Over-struck words | Tool calls |
|---|---|---|---|---|---|---|---|
| Fable  | **0.745** | 0.812 | 0.924 | 0.865 | 31 | 11  | 68 |
| Opus   | 0.327 | 0.848 | 0.619 | 0.716 | 25 | 86  | 45 |
| Sonnet | 0.085 | 1.000 | 0.522 | 0.686 | 0  | 151 | 21 |
| Haiku  | 0.000 | 1.000 | 0.406 | 0.578 | 0  | 241 | 12 |

No model added, deleted or reworded any text.

## Where each model went wrong

- **Fable** left the closing "lone survivors ... defy Balor's army" sentence visible (judged it a restatement of the prologue premise), and struck "Concurrently," plus the start of "The characters actively confront the sins of their ancestors, with".
- **Opus** kept "Cai ... son of the goddess Aurora ... grandson of Solel and Yu Phas" visible (it found sources placing those reveals in Part I), and struck the two "By the late-game climax / Instead, they harness the bonds" sentences and the "late-game convergence" sentence, which the golden doc keeps.
- **Sonnet** caught every spoiler but also struck the Leda/Theodora "hidden power brokers" clause, the "late-game climax" sentences, and the cross-faction alliances / Damascar recreation clause.
- **Haiku** caught every spoiler by striking each paragraph from its first "mid-game" sentence to the end, which over-struck the general "As mid-game ..." framing sentences.
