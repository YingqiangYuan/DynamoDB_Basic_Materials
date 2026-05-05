# TICKET: DynamoDB + pynamodb Deep Dive

Read this [Tutorial](https://github.com/easyscale-academy/learn_dynamodb_basic_opeartions-project/tree/01-Learn-This-Project/)

## Objective

Complete a deep, hands-on pass through all 12 example modules in this repository. By the end, you should be able to explain DynamoDB concepts and pynamodb usage at an interview-ready level — articulating the "what," "why," and "when" for each topic without needing to reference code.

## Acceptance Criteria

- [ ] All scripts in `examples/00-minimal-poc/` through `examples/11-single-table-many-to-many/` run successfully against your AWS account
- [ ] You have browsed the resulting tables in the AWS Console (DynamoDB → Tables) and can describe what each script created
- [ ] You can score **80% or higher** on the built-in Quiz (50 questions, 10 categories) using `/learn-this-project` → Quiz mode
- [ ] For any question you answer, you can explain it in plain English as if in a technical interview — no code recitation needed, but clear articulation of the concept and reasoning

## What This Is NOT

- There is no written deliverable to submit
- There is no code to write from scratch
- There is no deadline pressure — take the time you need

## How to Work Through This

1. **Setup** — Follow the environment setup in README.md (two commands + AWS profile)
2. **Learn** — Use `/learn-this-project` Guided Tour mode, run each script, check AWS Console
3. **Test** — Switch to Quiz mode, go category by category
4. **Fill gaps** — Any question you can't answer clearly → go back to the relevant script, re-read the folder's README.md, re-run, then retry the question
5. **Cleanup** — Run `python examples/cleanup_all_tables.py` when done

## Suggested Order

Folders 00–08 can be done in any order, but for a first pass:

1. `00-minimal-poc` — Get the skeleton in your head
2. `01-attributes` — Understand the type system
3. `02-table-management` — Know how tables are created and billed
4. `03-crud-basic` — Master single-item operations
5. `04-batch-operations` — Scale to many items at once
6. `05-query-and-scan` — The most important performance topic
7. `06-condition-expression` — Concurrency safety without locks
8. `07-transactions` — When you need multi-item atomicity
9. `08-gsi-and-lsi` — Escape hatch for non-key queries
10. `09-pipeline-metadata-demo` — See it all come together (sequential: s01→s04)
11. `10-single-table-one-to-many` — 1:N modeling (sequential: s01→s03)
12. `11-single-table-many-to-many` — M:N modeling, three approaches

## Success Looks Like

You're sitting in a technical interview. The interviewer asks: "When would you use a GSI vs an LSI?" or "Walk me through how optimistic locking works in DynamoDB" or "Why is single-table design a thing?" — and you can give a clear, confident, 30-second answer that demonstrates real understanding, not memorized bullet points.
