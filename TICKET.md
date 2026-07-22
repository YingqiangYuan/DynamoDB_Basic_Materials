# TICKET: DynamoDB + pynamodb Basic Operations — Learning Checklist

[Tutorial](https://github.com/easyscale-academy/learn_dynamodb_basic_opeartions-project/tree/01-Learn-This-Project/)

## Objective

Complete a deep, hands-on pass through all 12 DynamoDB example modules (`examples/00-minimal-poc/` through `examples/11-single-table-many-to-many/`). Validate that you've internalized both the **what** and the **why** of every component, then ship a portfolio version of this repo under your own GitHub.

## Checklist

### Setup
- [X] Clone the repo and switch to the `01-Learn-This-Project` branch
- [X] `mise install` — installs Python 3.12, uv, claude, pandoc as pinned in `mise.toml`
- [X] `cp .env.example .env` and edit it to set `AWS_PROFILE=<your-profile>` (profile needs DynamoDB read/write on `dynamodb_basic_opeartions_*` in `us-east-1`)
- [X] `mise run inst` — creates `.venv` and runs `uv sync --all-extras`
- [X] Smoke-test: `python examples/00-minimal-poc/s01_minimal_poc.py` runs end-to-end and prints `holder_name = Alice` (the first run hangs 5–30s on `create_table(wait=True)` — that's expected)

### Absorb (learn the content)
- [X] Run `/learn-this-project-absorb` in **Orient mode** for the high-level map and the explicit `files to READ` vs `files to RUN/DO` lists
- [X] Run every file on the run-list yourself (every `s*.py` under `examples/00-minimal-poc/` through `examples/11-single-table-many-to-many/`); watch the output, peek at the table in the AWS Console
- [X] Come back to `/learn-this-project-absorb` in **Context-dive mode** whenever a specific spot needs unpacking (paste a `file:line` and let it follow your context, don't get dragged back to "let me walk you through the architecture")
- [X] Be able to explain **why** every example uses `with use_boto_session(Model, bsm):` and what would silently break if you stripped it out (it's not a pynamodb built-in — it's from the `pynamodb-session-manager` package and routes pynamodb's connection at `one.bsm` for the duration of the block)
- [X] Be able to explain **why** `save()` is PutItem (full-row overwrite) and `update()` is UpdateItem (partial), and walk through the bug you'd produce by using `save()` to "change one field" (folder `03-crud-basic/`)
- [X] Be able to explain the composite SK pattern `TX#<card_id>#<ts>` in `examples/10-single-table-one-to-many/s01_setup_and_seed.py:20-21` — why the component order is the access pattern, and what query becomes impossible if you swap the order

### Quiz (verify understanding)
- [X] Run `/learn-this-project-quiz` in **Bank mode** — clear the floor on a 10-question round (no ⚠️ partial / ❌ wrong)
- [X] Use **Open-ended mode** to drill 2–3 topics where you came up shallow — name a topic like "Single-Table-Design adjacency vs GSI inversion" and let it generate fresh discussion-style questions
- [X] If anything keeps coming up partial, go back to the relevant analysis doc (`docs/learn-this-project/01-knowhow-inventory.md` is the spine) or the source file, then re-quiz

### Elevate (see what's beyond)
- [X] Run `/learn-this-project-elevate`, explore 1–2 upgrade directions (top candidates per the roadmap: a real `tests/` suite + DynamoDB Local fixture, a `DYNAMODB_ENDPOINT` env-var toggle in the currently-empty `OneConfigMixin`, type-safe single-table keys for folders 10–11)
- [X] **Converge each chosen direction into a concrete starter deliverable** with explicit file paths and a success criterion (e.g. "add `tests/test_examples.py` that subprocess-runs every `s*.py` and asserts exit 0", not vague "add tests")
- [X] (Optional, high-value) Hand the deliverable to `/learn-this-project-absorb` in **Build mode** and actually build the first iteration
- [X] Note down "next small projects" that interest you (Streams + Lambda? PartiQL? Global Tables? TTL?)

### Interview (pressure-test yourself)
- [X] Run `/learn-this-project-interview`, complete a full mock session (calibrate first: role, format, time)
- [X] Survive at least one pushback per Round 3 ("why pynamodb over boto3?", "why PAY_PER_REQUEST?") and Round 5 ("traffic 100x'd, what breaks first?") question
- [X] Review the debrief; for the 3 weak-spot questions, return to `/learn-this-project-quiz` or `/learn-this-project-absorb` and close the gap

### Demo (learn to present)
- [X] Run `/learn-this-project-demo`, rehearse at least the 5-minute version (the wow beat is folder 11 — three M:N approaches side by side)
- [X] Walk through the cardinal-rule "do NOT show" list — confirm you know that `README.md`, `README-cn.md`, `TICKET.md`, `CLAUDE.md`, `docs/learn-this-project/`, and the five sibling skills (`absorb`, `quiz`, `elevate`, `interview`, `demo`, `publish` — **`meta` is the explicit exception, keep it**) must never appear on screen during a demo

### Mastery Gate
- [X] You can answer ~70% of quiz questions to the **3-part standard** (where + what + why), not just factually
- [X] You can survive at least one pushback per interview question without buckling
- [X] You have a clear list of "what I'd study next" from the elevate session, each item with a concrete file path
- [X] You can deliver the 5-minute demo without notes, without exposing any cardinal artifact

### Publish (turn it into a portfolio artifact)
- [ ] Decide on a new public repo name (pattern: `<firstname>-<lastname>-dynamodb-poc` or `<firstname>-<lastname>-dynamodb-basic-operations-poc`)
- [ ] Run `/learn-this-project-publish` in **Transform mode** — the skill walks you through:
  - [ ] Intake: new repo name + your name (used in commit-message tone and optional README byline)
  - [ ] Delete cardinal teaching artifacts (skill does this with your dry-run consent — `README.md`, `README-cn.md`, `TICKET.md`, `CLAUDE.md`, `docs/learn-this-project/`, the five sibling skills; **`.claude/skills/learn-this-project-meta/` is kept as a portfolio bonus**)
  - [ ] Borderline review (your call on each file — `README.rst`, `dynamodb_basic_opeartions/tests/`, `docs/source/`, `.mise/tasks/`, `uv.lock`)
  - [ ] Generate `tmp/publish-commit-plan.md` — your dependency-ordered copy-paste cheat-sheet (~20 commits, root config → package skeleton → `one/` runtime → `examples/README.md` → each example folder one at a time → cleanup utility → meta-skill → hand-written `README.md`)
  - [ ] Co-write your `README.md` in your own voice (D-mode — it asks, you answer, it drafts, you edit; English only, 250–500 words)
- [ ] Verify **Audit mode** returns **0 🔴 HIGH RISK findings** before publishing
- [ ] Create the public GitHub repo yourself (skill won't do this — that's your deliberate publication act)
- [ ] Open `tmp/publish-commit-plan.md` and run the commits one at a time
- [ ] `git remote add origin <github-url>` and `git push -u origin main`

### Cleanup
- [ ] Run `python examples/cleanup_all_tables.py` to remove all `dynamodb_basic_opeartions_*` tables from your AWS account
