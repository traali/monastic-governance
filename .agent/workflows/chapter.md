# Workflow: Chapter (Session Opening Rite)

The opening rite for any agent session in a Monastic repository. Takes ~10 seconds.

---

## Steps

1. **Read `AGENTS.md`**
   Verify non-negotiable invariants, stack rules, and testing requirements.
2. **Read the tail of `ROLL.md`**
   Read the last ~10 entries to understand recent architectural decisions, rule dispensations, and dead ends.
3. **Read the Task**
   Read the user prompt, issue, or handoff specification.
4. **Select Accountable Office & AI Model Tier**
   Select the appropriate Office and assign a matched model tier (`flash`, `pro`, `inherit`):
   - **Cellarer:** Infrastructure, serverless, KV sync, package configs (`pro` / `flash`)
   - **Scriptorium:** Parsers, external APIs, NLP extractors, file ingestion (`pro` / `flash`)
   - **Prior:** Core domain state, databases, conflict reasoning (`pro` / `inherit`)
   - **Master of Works:** UI components, styling, responsiveness, accessibility (`inherit` / `flash`)
   - **Sacrist:** Test suites, mock fixtures, regression prevention (`flash` / `pro`)
   - **Visitor:** Clean-room adversarial audit against `AGENTS.md` (`pro` / `inherit`)
5. **Plan Before Execution**
   Formulate a concise plan. For major architectural changes, write an implementation plan before writing code.
