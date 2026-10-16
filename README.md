# Terminal Bench Tasks

Tasks for [Terminal-Bench](https://github.com/harbor-framework/terminal-bench), written in the [Harbor task format](https://harborframework.com/docs/task-format). Each task lives in its own folder under `tasks/` and follows the same layout as upstream tasks such as [`cad-model`](https://github.com/harbor-framework/terminal-bench/tree/main/tasks/cad-model).

## Layout

```
tasks/<task-name>/
├── README.md           # Reviewer-facing metadata: difficulty, solution, verification explanations
├── instruction.md      # The prompt the agent sees
├── task.toml           # Config (timeouts, resources, verifier mode) and metadata
├── environment/
│   ├── Dockerfile      # The agent's container; copy input files into /app here
│   └── ...             # Input data the task needs
├── solution/
│   ├── solve.sh        # Oracle entry point, run inside the agent container
│   └── solve.py        # Optional helper scripts used by solve.sh
└── tests/
    ├── Dockerfile      # Verifier image (only with environment_mode = "separate")
    ├── test.sh         # Runs the tests and writes /logs/verifier/reward.txt (1 or 0)
    └── test_outputs.py # Pytest checks against the agent's output
```

`tasks/log-summary` is a small working example: the agent parses a web access log and writes `/app/report.json`, and the verifier compares it to values measured from the oracle.

## Setup

```bash
uv tool install harbor        # or: pip install harbor
harbor --version
docker ps                     # Docker must be running
```

## Adding a task

1. Create a branch: `git checkout -b add-<task-name>`.
2. Scaffold the folder. Either copy the example (`cp -r tasks/log-summary tasks/<task-name>`) or let Harbor generate it:
   ```bash
   harbor tasks init <task-name> --include-canary-strings -p tasks/
   ```
3. Write `instruction.md`. State exactly what to produce and where (absolute paths under `/app`), so that a correct solution passes if and only if it follows the instruction.
4. Build the environment in `environment/Dockerfile` and put input files beside it. Pin base images and package versions so the task is reproducible.
5. Write the oracle in `solution/solve.sh`. It runs inside the agent's container and must produce the expected output from scratch.
6. Write the verifier in `tests/`. `test.sh` must write `1` or `0` to `/logs/verifier/reward.txt`. With `environment_mode = "separate"` (as in the example and `cad-model`), the verifier runs in its own image built from `tests/Dockerfile`, and only the paths listed in `artifacts` in `task.toml` are copied over from the agent's container, so bake every test dependency into that image.
7. Fill in `task.toml` (name, version, metadata, timeouts, resources, `artifacts`) and the task `README.md`.
8. If you copied a task, give it a fresh canary GUID (`python3 -c "import uuid; print(uuid.uuid4())"`) and replace the old one in every file.

## Testing a task

```bash
# The oracle must score 1.0
harbor run -p tasks/<task-name> -a oracle

# A no-op agent must score 0.0 (checks the verifier can't be passed by doing nothing)
harbor run -p tasks/<task-name> -a nop

# Try a real agent
harbor run -p tasks/<task-name> -a claude-code -m anthropic/claude-opus-5-5
```

Results land in `jobs/` (ignored by git); browse them with `harbor view jobs`.

## What makes a good task

Upstream's [CONTRIBUTING.md](https://github.com/harbor-framework/terminal-bench/blob/main/CONTRIBUTING.md) is the bar: real work someone would be paid to do in a terminal, hard for frontier agents for a meaningful reason, a solution not easily found online, and a verifier that passes if and only if the instruction was followed.
