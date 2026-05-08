# DynamoDB + pynamodb Tutorial

## What this course teaches

**Amazon DynamoDB** is the most important NoSQL database on AWS — built for high concurrency, low latency, and infinite horizontal scaling. **pynamodb** is the most elegant Python ORM for DynamoDB, letting you define tables as Python classes with one-third the boilerplate of raw boto3.

This tutorial walks you through 12 progressive, runnable POC scripts — from a minimal model definition all the way to single-table design with one-to-many and many-to-many relationship modeling. All examples use a fin-tech credit-card domain (AxiomCard) and run against a real AWS account.

### What you'll be able to do after

- Define pynamodb Models and perform DynamoDB read/write operations
- Understand the performance difference between query and scan, and know when to create GSI/LSI
- Handle concurrency with conditional writes and optimistic locking, use Transactions for cross-item atomicity
- **Model 1:N and M:N relationships with Single-Table Design** — the fundamental shift from SQL to DynamoDB thinking


## Prerequisites

We assume you've completed:

1. **AWS Basics** — you can create an AWS account and configure AWS CLI credentials
2. **Claude Code tutorials** — you know how to use the `/learn-this-project-*` family of commands
3. **mise-en-place** — you know this is the fastest way to set up a dev environment


## Setup

```bash
mise install
mise run inst
```

Then configure your AWS profile:

```bash
cp .env.example .env
# Edit .env with your AWS profile name (needs DynamoDB read/write in us-east-1)
```


## Course content

The `examples/` directory contains 12 progressive modules:

```
examples/
├── 00-minimal-poc/                    # Minimal runnable example (golden skeleton)
├── 01-attributes/                     # Attribute types: scalar, collection, JSON
├── 02-table-management/               # Create / describe / billing modes
├── 03-crud-basic/                     # save / get / update / delete / refresh
├── 04-batch-operations/               # batch_write / batch_get / UnprocessedItems
├── 05-query-and-scan/                 # query vs scan, pagination, sort
├── 06-condition-expression/           # Conditional writes, optimistic locking
├── 07-transactions/                   # TransactWrite / TransactGet
├── 08-gsi-and-lsi/                    # Secondary indexes, projections, GSI vs scan cost
├── 09-pipeline-metadata-demo/         # Composite demo: Pipeline Metadata table
├── 10-single-table-one-to-many/       # Single-table design: 1:N
└── 11-single-table-many-to-many/      # Single-table design: M:N three approaches
```

Folders 00–08 are independent — learn in any order. Folders 09–11 are sequential — run s01 → s02 → ... in order.


## How to learn

Open Claude Code and use these commands in order:

| Order | Command | What it does |
|---|---|---|
| 1 | `/learn-this-project-absorb` | Walk through the entire project, understand the what and why of each module |
| 2 | `/learn-this-project-quiz` | 60 granular questions to verify you actually internalized it |
| 3 | `/learn-this-project-elevate` | See how a senior engineer would improve this project, explore advanced directions |
| 4 | `/learn-this-project-interview` | Mock interview — pressure-test your understanding |
| 5 | `/learn-this-project-demo` | Learn how to present this project to others |

While learning, always run each script yourself and check the actual data in the AWS Console.


## Cleanup

When you're done, run:

```bash
python examples/cleanup_all_tables.py
```

This deletes all `dynamodb_basic_opeartions_*` tables from your AWS account.


## Show your work

After completing the course, showcase your learning on GitHub:

1. **Create a new public repo**, suggested name: `firstname-lastname-dynamodb-basic-operations-poc`
2. **Commit incrementally** (at least 15-20 commits): config files first, then example folders one by one
3. **Delete teaching artifacts**: remove `README.md`, `README-cn.md`, `TICKET.md`, `docs/learn-this-project/`, `.claude/skills/learn-this-project-*/`
4. **Keep only `README.rst`** as the project description
5. **Write your own README** — describe what you learned in your own words
