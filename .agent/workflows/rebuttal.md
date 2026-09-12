# Workflow: Rebuttal (Right of Appeal)

The author may rebut any finding from a Visitation with one written response declaring **exactly one valid ground**.
Appeal is the mechanism by which false positives are removed and outdated rules are corrected.

---

## The 4 Valid Grounds of Appeal

| Ground | Name | Claim | Decided by | Action |
|---|---|---|---|---|
| **Ground 1** | **Misread** | Visitor's factual claim about the code is wrong | Original Visitor re-reading cited lines | Finding withdrawn if verified |
| **Ground 2** | **Out of Scope** | Pre-existing defect, not introduced by this diff | Check git diff history | Finding withdrawn; logged in `DEBT.md` |
| **Ground 4** | **Rule Wrong** | Valid code, but the rule in `AGENTS.md` is contradictory or obsolete | Author proposes rule amendment | Rule amended in `ROLL.md`; finding falls |
| **Ground 5** | **Deferred** | Valid advisory finding, deferred to future sprint | Author + Stakeholder | Advisory only — merge with `DEBT.md` entry (owner + deadline) |

*(Ground 3 is reserved for blind second-visitor tiebreaks in multi-author teams).*

---

## Filing a Rebuttal

Record the appeal directly in the visitation report or in the pull request conversation:

```markdown
### Rebuttal to Finding F1
- **Ground:** Ground 1 (Misread)
- **Evidence:** Line 42 in `src/api.ts` uses `sanitizeInput(raw)` before ingestion. See test fixture in `tests/api.test.ts:18`.
- **Request:** Re-inspect cited line and withdraw finding.
```
