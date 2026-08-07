# tasks

Team backlog and planning for LangWatch.

## Which repo does an issue go in?

Three repos, split by who reads them.

| Repo | Visibility | What goes here |
| --- | --- | --- |
| `langwatch/tasks` | public | Team backlog and planning. Proposals, RFCs, spikes, tech debt, refactors, deferred follow-ups, roadmap items, internal process and docs. |
| `langwatch/langwatch` | public | Real bugs, and anything an outside contributor could pick up. |
| `langwatch/langwatch-saas` | private | Anything that names a customer, partner or vendor. |

Quick test:

- Does it describe a defect with a way to reproduce it? `langwatch/langwatch`.
- Could someone outside the team pick it up and ship it? `langwatch/langwatch`.
- Does it name an account? `langwatch/langwatch-saas`, and scrub names if a public copy is needed.
- Is it "we should do this at some point"? Here.

Security material stays in `langwatch/langwatch` even when it reads like backlog.

## Why this repo exists

`langwatch/langwatch` had around 650 open issues. Most were not bugs, they were
backlog. That made the real bug count impossible to read and gave a bad first
impression to anyone visiting the repo. Splitting by audience fixes both without
throwing anything away.

## Moving an issue here

Use `gh issue transfer`. It keeps comments, labels, issue type and the original
created date, and it leaves a redirect from the old URL so existing links and
cross-references still work.

```
gh issue transfer <number> langwatch/tasks --repo langwatch/langwatch
```

Always pass `--repo`. Without it `gh` resolves the source from your current
working directory, which is usually not the repo you meant.

**Mirror labels first.** If a label does not exist here, the transfer drops it
silently. Create the missing labels in this repo before transferring, copying
the name, colour and description from the source.

Do not use project draft items for this. There is no way to convert a draft back
into an issue, so it is a one way door, and drafts have no repo, no labels and
no issue type.

## Board

Issues here are tracked on the LangWatch Kanban board, org project 5.

- New issues enter at Status `Ready`.
- Transferred issues keep their board membership and field values.
- If you file directly here and the issue does not appear on the board, add it
  by hand. Auto-add is configured per repo and does not cover every repo.

## Two traps when cleaning up

**Do not key staleness off `updated_at`.** A scheduled labeling agent touched
hundreds of issues in bulk, which moved `updated_at` on roughly half the open
backlog. Any "not updated in 30 days" rule reads all of those as active work.

Key staleness on human, non-cosmetic timeline events instead. Count comments,
assignments, commit and pull request references, and reopens. Ignore `labeled`,
`unlabeled`, `milestoned`, board status moves, `renamed`, `pinned`, `mentioned`,
and cross-references authored by bots.

**Creating an issue leaves no timeline event.** An issue filed yesterday with one
bot label on it looks maximally stale by any timeline test. Guard on `created_at`
as well, or you will sweep fresh work into the backlog.

## Closing things

Close an issue as completed only with positive evidence in current code that it
is done. Absence of evidence is not evidence. If a merged pull request looks like
it addressed the symptom but the code has since been rewritten, say so in a
comment and leave the issue open with what to re-check.
