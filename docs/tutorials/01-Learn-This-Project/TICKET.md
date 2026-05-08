# TICKET: DynamoDB + pynamodb Basic Operations — Learning Checklist

## Objective

Complete a deep, hands-on pass through all 12 DynamoDB example modules and validate your understanding through quiz, mock interview, and demo rehearsal.

Read [Tutorial](https://github.com/easyscale-academy/learn_dynamodb_basic_opeartions-project/tree/01-Learn-This-Project/)

## Checklist

### Setup
- [ ] Clone the repo and switch to the `01-Learn-This-Project` branch
- [ ] Run `mise install && mise run inst` to set up the environment
- [ ] Configure `.env` with your AWS profile (copy from `.env.example`)
- [ ] Verify: `python examples/00-minimal-poc/s01_minimal_poc.py` runs without errors

### Absorb (learn the content)
- [ ] Run `/learn-this-project-absorb`, complete the full walkthrough
- [ ] Run each example script yourself (folders 00–11), read the output
- [ ] Understand why `save()` (PutItem) differs from `update()` (UpdateItem) — this is a common source of bugs
- [ ] Understand the cost difference between `query` and `scan` — check ConsumedCapacity in folder 09
- [ ] Understand the single-table design mental shift: design for access patterns, not entity normalization

### Quiz (verify understanding)
- [ ] Run `/learn-this-project-quiz`, complete at least one full round
- [ ] Score 80%+ on a 10-question random round
- [ ] Review and re-study any topics where you scored poorly

### Elevate (see what's beyond)
- [ ] Run `/learn-this-project-elevate`, explore at least 1-2 directions
- [ ] Note down "next small projects" that interest you

### Interview (pressure-test yourself)
- [ ] Run `/learn-this-project-interview`, complete a full mock session
- [ ] Review the debrief, note which questions need more prep

### Demo (learn to present)
- [ ] Run `/learn-this-project-demo`, rehearse at least the 5-minute version
- [ ] Walk through the "do NOT show" checklist

### Mastery Gate
- [ ] You can explain **at least 70%** of the knowledge points from the quiz bank
- [ ] You can answer interview-style questions with a concept and direction (even if not perfect)
- [ ] You have a clear list of "what I'd study next" from the elevate session

### Show Your Work
- [ ] Create your own public repo (renamed, no "learn" prefix)
- [ ] Commit incrementally (15-20+ commits)
- [ ] Delete all teaching artifacts (`docs/learn-this-project/`, skills, `README-cn.md`, `TICKET.md`, etc.)
- [ ] Write your own README

### Cleanup
- [ ] Run `python examples/cleanup_all_tables.py` to remove all tutorial tables from your AWS account
