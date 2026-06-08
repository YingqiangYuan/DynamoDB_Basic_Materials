# DynamoDB + pynamodb Tutorial

**Amazon DynamoDB** is the most important NoSQL database on AWS — built for high concurrency, low latency, and infinite horizontal scaling. **pynamodb** is the most ergonomic Python ORM for DynamoDB — it lets you write tables as Python classes with about a third the boilerplate of raw boto3. This repo walks you through 12 progressive POC scripts, from a minimal Model definition all the way to Single-Table Design with 1:N and M:N modeling. All examples use a fin-tech credit-card domain (AxiomCard) and run against a real AWS account.

---

## What is this — the methodology in 30 seconds

This is a **learn-this-project** repo: a small, deliberately-scoped codebase that teaches **one vertical skill end-to-end**. The point isn't to ship the code — it's to **absorb** the skill by running it, reading it, being able to defend every design choice, and finishing with a portfolio version on your own GitHub.

Six interactive skills make up the process:

- **`/learn-this-project-absorb`** — on-call mentor for the repo. Multi-mode: **Orient** (gives you the map plus an explicit `files to READ` vs `files to RUN/DO` split), **Context-dive** (you bring a `file:line`, it unpacks that spot), **Next-step**, **Build** (helps you extend the repo with per-edit consent). It's a mentor, not a curriculum — invoke when you're lost, stuck, or about to attempt something new, not as something to sit through linearly.
- **`/learn-this-project-quiz`** — discussion-style Q&A. Each answer is scored against the **3-part standard**: **where to look + what + why**. A factually correct one-liner doesn't pass. Two modes: the pre-written bank (lower-bound check) and open-ended (you name a topic, it generates fresh questions in the same shape).
- **`/learn-this-project-elevate`** — what's beyond this repo's current state. For each upgrade direction it walks you through current state → senior-engineer target → alternatives → prerequisite knowledge, and **converges into a concrete starter deliverable** ("add `tests/test_examples.py` that subprocess-runs each script and asserts exit 0") that you hand back to Absorb Build mode to actually build.
- **`/learn-this-project-interview`** — full-project mock interview with pushback. Tests whether you can defend the work to a stranger.
- **`/learn-this-project-demo`** — script your live walk-through. The highest-value part isn't "what to show" — it's the cardinal-rule "do NOT show" list (teaching artifacts that would tell the audience this came from a tutorial).
- **`/learn-this-project-publish`** — convert this teaching repo into a portfolio version on your own GitHub. Deletes teaching artifacts, generates a dependency-ordered commit cheat-sheet for you to copy-paste (the skill never runs `git`), co-writes your README in your own voice, finishes with a hostile-scan audit.

**Recommended order**: absorb → quiz → elevate → interview → demo → publish. But the skills are mentors on call — invoke when you need orientation, context, or help. Not a curriculum to follow linearly.

---

## What's in this repo

```
.
├── examples/                          # 12 progressive modules (the actual content)
│   ├── 00-minimal-poc/                #   golden-reference skeleton (~80 lines)
│   ├── 01-attributes/                 #   attribute types: scalar / collection / JSON
│   ├── 02-table-management/           #   create / describe / billing modes
│   ├── 03-crud-basic/                 #   save / get / update / delete / refresh
│   ├── 04-batch-operations/           #   batch_write / batch_get / UnprocessedItems
│   ├── 05-query-and-scan/             #   query vs scan, pagination, sort
│   ├── 06-condition-expression/       #   conditional writes, optimistic locking
│   ├── 07-transactions/               #   TransactWrite / TransactGet
│   ├── 08-gsi-and-lsi/                #   secondary indexes, projections, GSI vs scan cost
│   ├── 09-pipeline-metadata-demo/     #   composite demo: PipelineRun + StatusIndex GSI
│   ├── 10-single-table-one-to-many/   #   single-table design: 1:N (Customer → Card → Tx)
│   ├── 11-single-table-many-to-many/  #   single-table design: M:N three approaches compared
│   ├── README.md                      #   index of all 12 folders + conventions + learning order
│   └── cleanup_all_tables.py          #   nuke every dynamodb_basic_opeartions_* table
├── dynamodb_basic_opeartions/         # thin runtime package: exposes one.bsm singleton
│   └── one/                           #   BotoSesManager cached_property wrapper
├── docs/learn-this-project/           # 7 analysis docs (knowledge base for the 6 skills)
├── .claude/skills/learn-this-project-*/   # 6 interactive learning skills
├── mise.toml / pyproject.toml         # toolchain + deps (mise → Python+uv+claude; uv → packages)
└── .env.example                       # the only env var needed: AWS_PROFILE
```

Folders 00–08 are **independent** — learn in any order. Folders 09–11 are **sequential** within each folder — run `s01 → s02 → ...` in order.

---

## Tech stack + setup

| Component | Version | Role |
|---|---|---|
| Python | 3.12 (pinned in `mise.toml`) | runtime |
| uv | latest | package manager (replaces pip + virtualenv) |
| pynamodb | ≥6.1.0 | DynamoDB ORM |
| boto3 + boto-session-manager | ≥1.42.95 | AWS SDK + profile/region/cached-client wrapper |
| pynamodb-session-manager | ≥0.1.2 | the `use_boto_session(Model, bsm)` context manager |

Four commands to get running:

```bash
mise install                # installs Python 3.12 / uv / claude / pandoc
cp .env.example .env        # then edit and set AWS_PROFILE=<your-profile>
mise run inst               # creates .venv and runs uv sync --all-extras
python examples/00-minimal-poc/s01_minimal_poc.py   # smoke test
```

Step 4's first run will hang for 5–30 seconds — that's `create_table(wait=True)` waiting for DynamoDB to transition the new table to ACTIVE. Normal. Subsequent runs finish in ~2 seconds. Your AWS IAM principal needs read/write on `dynamodb_basic_opeartions_*` tables in `us-east-1`.

---

## When you're done

Run the cleanup utility:

```bash
python examples/cleanup_all_tables.py
```

It lists every table whose name starts with `dynamodb_basic_opeartions_`, asks for confirmation, and deletes them all. Then move on to `/learn-this-project-publish` to turn this teaching repo into a portfolio version.

---

## Publish to your own GitHub

A teaching repo always reads as a teaching repo — push the raw version to GitHub and an interviewer will spot it immediately. `/learn-this-project-publish` automates the conversion: deletes the teaching artifacts, generates a dependency-ordered commit cheat-sheet, co-writes a new README in your own voice, and finishes with a hostile-scan audit.

- The skill operates on local files. **It never runs `git`** — you do `git add` / `git commit` / `git push` yourself by copy-pasting from `tmp/publish-commit-plan.md`.
- The README co-write is D-mode: it asks 2–3 questions per section, listens to your answers, drafts a paragraph in your voice, you review and edit. It does not invent insight you didn't supply.
- The final Audit must come back with **zero 🔴 HIGH RISK findings** before the publish is considered complete.

**Cardinal rule**: the published repo cannot read as teaching material. That's the single hard test — the whole publish flow is built around it.

---

## What mastery looks like

By the end, you should be able to:

- Define pynamodb Models and do DynamoDB reads/writes — and explain to a teammate **why** `save()` is a full-row PutItem while `update()` is a partial UpdateItem, and what silently breaks when they mix them up.
- Pick between query and scan by reflex, and use real ConsumedCapacity numbers (see `examples/09-pipeline-metadata-demo/s04_compare_capacity.py`) to justify adding a GSI rather than scanning.
- Handle concurrency with conditional writes and optimistic locks; know when to escalate to `TransactWrite` and compute whether the 2× cost is worth it.
- Sketch a Single-Table Design on a whiteboard — **access patterns first, PK/SK second** — and explain why the component order in a composite SK like `TX#<card_id>#<ts>` is load-bearing. That's the actual jump from SQL thinking to DynamoDB thinking.
