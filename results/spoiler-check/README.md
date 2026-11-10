# spoiler-check results (task v0.4: story plus act-specific items, weapons, classes and builds)

Each model got only `script.docx` and the task instruction, with web access and Python (`python-docx`), and wrote `script_act1.docx`. Each folder holds that output, `score.json` (score, diagnostics and the exact error spans) and `reward.txt`.

These were run as Claude Code subagents in a shared sandbox, not through `harbor run`. The golden doc, `sentences.json` and `build_case.py` were moved out of the repo during the runs and the agents were told not to read outside their folder.

Score = `1 - errors / 761` (761 = struck words in the golden doc). Accuracy = share of all 1,292 words with the right strike status.

| Model | Score | Accuracy | Spoilers caught | Struck words that were right | Tool calls |
|---|---|---|---|---|---|
| Fable  | **0.975** | 98.5% | 97.5% | 100% | 24 |
| Opus   | **0.975** | 98.5% | 97.5% | 100% | 16 |
| Haiku  | 0.778 | 86.9% | 83% | 94% | 8 |
| Sonnet | 0.587 | 75.7% | 66% | 90% | 7 |

No model added, deleted or reworded any text.

## Errors

- **Fable** and **Opus** made the same single mistake: they left the veiled Act 2 spoiler "One ruler at the heart of this story turns out to be far less human than his title suggests." visible. Both looked up the item sentences in walkthroughs and struck every later-act weapon, class and system.
- **Haiku** left about five later-act item sentences visible (the Deadeye Longbow, master classes at level 45, squads, equipped gear following units into the war, Smyrnos's Thoron Gauntlet), and struck the Act 1 halves of two mixed sentences plus a stray "though".
- **Sonnet** treated most gameplay sentences as harmless tips and left about ten later-act ones visible: war-camp +2/+3 weapons, the Concoction/Antitoxin rule, equipped gear, squads, the squad-building tip, master classes, the Elephant Rider, the five keys and divine classes, the sealed temple, and weapons returning in the convoy. It also struck three Act 1 setup lines about Sothis, Fortuna's time travel and the survivors having no army.

Earlier cases: `../spoiler-check-v0.3/`, `../spoiler-check-v0.2/`, `../spoiler-check-v0.1/`.
