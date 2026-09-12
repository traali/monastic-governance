# AGENTS.md — The Rule of Monastic Governance

The canonical, tool-agnostic rule for all AI agents and contributors working in this repository.

---

## §0 Precedence
1. `AGENTS.md` (this file) is the supreme project rule.
2. Native tool configs (`CLAUDE.md`, `.cursorrules`, etc.) are thin pointers to this file and must contain no independent rules.
3. In conflicts between code comments and `AGENTS.md`, `AGENTS.md` wins.

---

## §1 Identity & Purpose
- This repository is the canonical reference implementation, template, and documentation blueprint for the **Monastic Governance Model for AI Coding Agents**.
- It provides portable specifications, workflow definitions, and verification scripts that any team or project can adopt.

---

## §2 Non-Negotiables & Invariants
| Use | Never |
|---|---|
| Pure, zero-dependency Node.js ESM scripts | Heavy external build dependencies for governance scripts |
| Concise rule files (< 1,500 words) | Sprawling 50KB system prompts (prompt rot) |
| Strict Markdown & GitHub-Flavored Markdown | Proprietary or tool-locked governance schemas |
| Clean-Room Visitation audits | Authors auditing their own pull requests or commits |

---

## §3 Testing & Quality Gates
- Every template, workflow, and script must execute cleanly without external dependencies.
- `scripts/monastery-visitor.mjs` must run successfully with `node scripts/monastery-visitor.mjs`.
- Definition of Done: 100% valid templates, zero broken markdown links, clean syntax.

---

## §4 Security & Zero Secrets
- No tokens, API keys, or private endpoints in any template or document.
- All templates use generic placeholders (`YOUR-PROJECT`, `XXXXX-X`).

---

## §5 Visitation (Separation of Duties)
- Changes to this blueprint must be independently audited by a clean-room Visitor before merging.
- Verdicts: `PASS` · `PASS WITH FINDINGS` · `BLOCK`.

---

## §6 Volatile Facts (Not in this file)
Volatile facts belong in single sources of truth:
- Git history and releases: `CHANGELOG.md`
- Decisions: `ROLL.md`
