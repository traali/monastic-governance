# ROLL.md — The Chronicle of Monastic Governance

Append-only record of blueprint releases, architectural decisions, and rule amendments.

---

## 2026-09-12 — Foundation of the Monastic Governance Blueprint
- **Actor:** Archon (`traali`)
- **Action:** Created universal repository template containing `AGENTS.md`, `.agent/workflows/`, CI workflows, and the all-in-one specification.
- **Verdict:** PASS
- **Rationale:** Give the developer and AI agent community an open-source, battle-tested standard for high-discipline agentic coding.

## 2026-09-12 — Align Template with Live Sports Federation
- **Actor:** Archon (`traali`)
- **Action:** Matched this blueprint to production: 7 sovereign monasteries + `sports-federation` kattorepo; added `handoff.md`; CI now runs `npm run visit` like pelipaiva.
- **Verdict:** PASS
- **Rationale:** Case study listed a 6-repo mix that did not match GitHub. Gold standard remains pelipaiva / football-stats.

## 2026-09-12 — Chapter of Faults & CI lockfile fix
- **Actor:** Sacrist & Archon
- **Action:** Visit workflow no longer requires `package-lock.json` (`npm ci` if present, else `npm install`). Added `docs/ISSUE_DIVISION.md` so federation gaps are filed in the owning monastery (`house` / `RULE` / `treaty` / `congregation`).
- **Verdict:** PASS
- **Rationale:** Template CI was red on every push. Cross-repo bugs were landing in the wrong house.
