# Workflow: Visitation (Independent Clean-Room Audit)

The outside inspection mechanism. Executed in an isolated subagent context with **NO conversation history from the author**. Run only on a clean tree (`npm run visit` passing).

---

## Preconditions (Do not inspect a broken tree)
- [ ] Working tree committed or ready for audit.
- [ ] `npm run visit` passes: static lint passes (0 errors), all tests green.

---

## Prompt to the Visitor Subagent

```
You are the outside Visitor conducting an independent audit of branch <branch> against AGENTS.md.
You did not write this code.

Your context is: AGENTS.md, the git diff against base <base-sha>, and the test results.
Nothing else — no author reasoning, no conversation history.

Instructions:
1. Read AGENTS.md in full before inspecting the diff.
2. For every finding, cite the exact rule section (§N) and file:line that violates it.
3. Classify each finding as `blocking` (security/data loss) or `advisory`.
4. Assign fault: `house` (code issue) or `RULE` (rule is wrong).
5. Zero findings is a valid and expected outcome. Do not invent findings to appear thorough.
   Do not summarize what went well. Do not compliment the author.
6. Write your report to .agent/visitations/<branch>-<date>.md using the template below.
7. Return your final verdict: PASS | PASS WITH FINDINGS | BLOCK.
```

---

## Report Template (`.agent/visitations/<branch>-<date>.md`)

```markdown
# Visitation: <branch> — <date>
Visitor: <agent-identity> · Implementer: <agent-identity> · Base: <base-sha>

## Verdict
PASS | PASS WITH FINDINGS | BLOCK

## Findings

| # | Class | Fault | Rule § | Location | Claim |
|---|---|---|---|---|---|
| F1 | blocking | house | §2 | src/lib/api.ts:42 | Missing input sanitization on freeform NLP input |
| F2 | advisory | house | §3 | src/lib/db.ts:88 | IndexedDB table missing index on `startDate` |

*(Zero findings is a complete and valid report — remove example rows and write "No findings." here.)*

## Areas Checked
- <explicit list of areas inspected>

## Areas Not Checked
- <explicit list skipped and why>
```

---

## Finding Classes & Fault Attribution

| Class | Meaning | Mergeable without fix? |
|---|---|---|
| `blocking` | Security vulnerability, data loss, contract breach | No — never deferrable |
| `advisory` | Rule violation without data loss consequences | Only with a `DEBT.md` entry (owner + deadline) |

| Fault | Meaning | Next Action |
|---|---|---|
| `house` | Code violates the Rule | Author fixes code, or files a rebuttal (`rebuttal.md`) |
| `RULE` | Rule is wrong, unclear, or contradictory | Author proposes amendment in `ROLL.md`; current Rule stands until amended |
