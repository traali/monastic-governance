# Chapter of Faults — How gaps are divided between monasteries

Sovereign remotes. No shared working tree. A finding is assigned to **one** monastery (or the kattorepo / this template) using the same `house` vs `RULE` split as Visitation.

## 1. Classifier (Visitor / Legate / Abbas Primas)

| Class | Owner repository | Default office | Examples |
|---|---|---|---|
| `house` | The monastery whose code, tests, CD, or UI is wrong | Cellarer / Scriptorium / Prior / Master of Works / Sacrist | Failing `npm run visit`, stale Pages deploy, title `web`, flaky SLA tests |
| `RULE` | The monastery whose `AGENTS.md` is wrong; **or** `monastic-governance` if the template itself is wrong | Archon | Word-cap breach, volatile facts in the Rule, missing rite |
| `treaty` | `sports-federation` (`contracts/`) | Legate | Missing interface, breaking field, adapter not satisfying a canon |
| `congregation` | `sports-federation` | Abbas Primas | Golden-test drift, `GLOBAL_ROLL.md`, deployment matrix, sibling-path leftovers in federation runners |

Never file a `house` bug in the kattorepo because it is easier. The house that owns the file owns the issue.

## 2. Issue shape (required fields)

```markdown
## Fault
- **Class:** house | RULE | treaty | congregation
- **Monastery:** pelipaiva | football-stats | floorball-stats | basketball-stats | volleyball-stats | Parkkis | weather-stats | sports-federation | monastic-governance
- **Office:** Cellarer | Scriptorium | Prior | Master of Works | Sacrist | Legate | Archon | Abbas Primas
- **Severity:** blocking | advisory
- **Evidence:** URL, SHA, prod build commit, CI run

## Acceptance
- [ ] `npm run visit` green on the owning remote
- [ ] Prod build commit matches HEAD (or N/A for template/kattorepo)
- [ ] `ROLL.md` / `GLOBAL_ROLL.md` entry appended
```

File the issue **in the owning repository**. The kattorepo keeps a ledger (`docs/GAPS.md`) that *points* at those issues. Do not duplicate the work item.

## 3. Cross-cutting

If a change needs two houses (e.g. Pelipäivä drawer + weather-stats Pages URL):

1. One `congregation` parent in `sports-federation`.
2. One `house` child in each monastery.
3. Parent stays open until every child is closed.

## 4. Lefthook / git-guard (independent remotes)

`../scripts/git-guard.mjs` is a **monorepo leftover**. Each monastery must vendor `scripts/git-guard.mjs` and point Lefthook at `node scripts/git-guard.mjs`. Federation runners that `join(ROOT, 'pelipaiva')` only work in a local checkout of siblings; they are not GitHub-remote truth.
