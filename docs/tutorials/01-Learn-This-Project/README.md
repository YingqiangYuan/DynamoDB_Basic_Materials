# DynamoDB + pynamodb — Hands-On Tutorial

## What You'll Learn

**Amazon DynamoDB** is AWS's fully managed NoSQL database — think of it as the database behind your Amazon shopping cart, scaled to handle Black Friday traffic without breaking a sweat. **pynamodb** is a Python ORM that lets you define DynamoDB tables as Python classes and cuts your code to about a third of what raw boto3 would require.

If you're building anything on AWS — APIs, data pipelines, serverless apps — DynamoDB is the go-to for workloads that need single-digit millisecond latency at any scale. This tutorial takes you from zero to competent through a series of small, runnable scripts you can execute against a real AWS account.

### Skills You'll Walk Away With

- **The pynamodb skeleton** — Define tables with Model + Attribute + Meta; read and write items in a few lines
- **Single-item CRUD** — save / get / update / delete / refresh, and why PutItem and UpdateItem are different tools for different jobs
- **Batch & query** — batch_write / batch_get / query / scan, and why "never scan in production" is the first rule of DynamoDB
- **Conditional writes & optimistic locking** — Server-side preconditions that prevent race conditions without database locks
- **Transactions** — Atomic multi-item writes (TransactWrite / TransactGet), when to pay the 2x cost, and when not to
- **Secondary indexes (GSI / LSI)** — Build indexes for non-key access patterns; understand projections and consistency trade-offs
- **Single-table design** — Model one-to-many and many-to-many relationships in a single table, driven by access patterns rather than entity shapes


## Prerequisites

We assume you've already completed:

1. **AWS Fundamentals** — You know how to create an AWS account and configure CLI credentials (`aws configure`)
2. **Claude Code tutorial series** — You know how to use slash commands (`/`) to collaborate with an AI assistant
3. **mise-en-place** — You know this is the fastest way to bootstrap a dev environment on your machine

If any of these are unfamiliar, go complete the corresponding prerequisite first.


## Setup

### Step 1: Install dependencies

Two commands:

```bash
mise install
mise run inst
```

If something doesn't work, open Claude Code and type `/learn-this-project` — the AI will help you get your environment running on macOS or GitHub Codespaces.

### Step 2: Configure AWS

These scripts talk to real DynamoDB tables in your AWS account (no local emulator). You need an AWS profile with DynamoDB read/write access in `us-east-1`:

1. Copy the example env file:
   ```bash
   cp .env.example .env
   ```
2. Edit `.env` and fill in your profile name:
   ```
   AWS_PROFILE="your-profile-name"
   ```

That's it. You're ready to go.


## How to Learn

### Course Structure

The `examples/` directory contains 12 modules, progressing from a minimal hello-world all the way to single-table design patterns. **Each folder has its own `README.md`** with background context and per-script explanations:

```
examples/
├── 00-minimal-poc/                    # Minimal end-to-end script (Model + Attribute + Meta)
├── 01-attributes/                     # Attribute types: scalar, collection, JSON
├── 02-table-management/               # create_table / describe_table / billing modes
├── 03-crud-basic/                     # save / get / update / delete / refresh
├── 04-batch-operations/               # batch_write / batch_get / UnprocessedItems
├── 05-query-and-scan/                 # query vs scan, pagination, sort + limit
├── 06-condition-expression/           # Conditional writes, optimistic locking
├── 07-transactions/                   # TransactWrite / TransactGet — cross-item ACID
├── 08-gsi-and-lsi/                    # Secondary indexes, projections, GSI vs scan cost
├── 09-pipeline-metadata-demo/         # Composite demo: real-world pipeline metadata table
├── 10-single-table-one-to-many/       # Single-table design: 1:N (Customer → Card → Transaction)
└── 11-single-table-many-to-many/      # Single-table design: M:N three ways
```

Folders 00–08 are independent — jump to whichever topic interests you. Folders 09–11 are sequential demos — run their scripts in numeric order (s01 → s02 → ...).

Run any script like this:

```bash
python examples/00-minimal-poc/s01_minimal_poc.py
```

### AI-Assisted Learning

**The key command:** Open Claude Code and type:

```
/learn-this-project
```

This loads the full tutorial context into the AI. It offers two modes:

- **Guided Tour** — The AI walks you through the project step by step, explaining each script
- **Quiz** — 50 questions across 10 topic areas, graded with code references so you can verify

### Recommended Workflow

Open **two terminal sessions** side by side:

1. **Session 1** — Learning: run `/learn-this-project`, follow the guided tour
2. **Session 2** — Testing: run `/learn-this-project`, pick Quiz mode, test yourself as you go

While learning:

- **Run the code yourself** — Don't just read. Execute each script and look at the output
- **Check the AWS Console** — Log into DynamoDB in the AWS Console and browse the tables, items, and indexes your scripts created
- **Screenshot anything confusing and ask the AI** — That's how learning works

### How to Know You've Got It

The learning cycle is: **learn → test → re-learn**. You've got it when:

1. **Every script runs** — You've executed each one and understand what it printed
2. **80%+ on the Quiz** — If someone asked you these questions in a technical interview, you could explain the concepts in plain English (no need to recite code, but you should be able to articulate the "what" and the "why")

If you get stuck on a Quiz question, go back to the relevant script, re-read it, re-run it, then try again.

### Pacing

There's a lot here — 12 modules, 50 Quiz questions. Take your time. No rush.

When you hit something you don't understand:

- **Factual questions** ("What's the difference between query and scan?" "How many items can BatchWriteItem handle?") → Ask the AI. It knows.
- **Judgment questions** ("When should I use single-table design?" "DynamoDB vs Postgres for my use case?" "Is this access pattern reasonable?") → Write them down and bring them to your mentor.

Learning to tell these two categories apart is itself a valuable career skill.

### Review

After you've gone through everything, come back anytime and run `/learn-this-project` in Quiz mode to stay sharp.


## Cleanup

All example scripts create tables prefixed with `dynamodb_basic_opeartions_` in your AWS account. When you're done:

```bash
python examples/cleanup_all_tables.py
```

Type `yes` to confirm. This deletes only the tutorial tables — nothing else in your account is touched.


## Show Your Work

🚨 **Important — this is your actionable deliverable!** 🚨

When you're done, we want you to put your learning on GitHub so others can see you're someone who learns systematically and ships incrementally.

### Create Your Own Repository

This tutorial repo is private. You'll need to:

1. Clone it locally
2. Create a **new public repository** under your own GitHub account

Naming suggestion: don't copy `learn_dynamodb_basic_opeartions`. Put your name in it and add `POC`. For example:

```
firstname-lastname-dynamodb-basic-operations-poc
```

Or pick any name you like — just make it yours.

### Commit Incrementally (This Matters)

🚨 **Do NOT dump everything into one giant commit!** 🚨

Your repo should tell the story of progressive learning. Suggested commit sequence:

1. Root config files (`mise.toml`, `pyproject.toml`, etc.)
2. The `.claude/` directory
3. Source code (`dynamodb_basic_opeartions/`)
4. Each `examples/` subfolder as its own commit

Aim for **15–20 commits** that show a clear learning progression.

### 🚨 Files You Must Delete 🚨

In your public repo, **remove these files**:

- `README.md` — delete
- `README-cn.md` — delete
- `TICKET.md` (if present) — delete

These are tutorial scaffolding. They shouldn't appear in your showcase repo.

**Keep only `README.rst`** — that's the real project README. It reads like something you wrote after exploring and learning on your own, not like you followed a tutorial.

### End Result

Your public repo should look like:
- 15–20 commits showing progressive, incremental learning
- Only `README.rst` as the project description
- No tutorial artifacts (no README.md, README-cn.md, or TICKET.md)
- A portfolio piece that shows you're methodical and self-driven
