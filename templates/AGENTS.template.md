# AGENTS.md — The Rule of [Your Project Name]

The canonical, tool-agnostic rule for all AI agents and contributors working in this repository.

---

## §0 Precedence
1. `AGENTS.md` (this file) is the supreme project rule.
2. Native tool configs (`CLAUDE.md`, `.cursorrules`, etc.) are thin pointers to this file and must contain no independent rules.
3. In conflicts between code comments and `AGENTS.md`, `AGENTS.md` wins.

---

## §1 Identity & Architecture
- [Briefly state what your system does].
- **Architecture:** [e.g. Offline-First IndexedDB, Client-Side Compute, Edge API Proxy, Zero-Auth Sync].

---

## §2 Stack & Non-Negotiables

| Use | Never |
|---|---|
| Strict TypeScript (no `any` types) | Untyped `any` casting, implicit dynamic types |
| Accessible UI primitives + Design Tokens | Unstyled raw components, ad-hoc inline styles |
| Indexed persistence for primary domain data | Direct un-indexed localStorage for core state |
| Deterministic automated tests | Untested regular expressions or date manipulation |
| Zero-Secret Commitment | Hardcoded API keys, tokens, or environment secrets |

---

## §3 Testing & Quality Gates
- **Unit & Integration Tests:** All parsers, date calculations, and business logic must have deterministic test fixtures. Zero dynamic/relative dates in mocks.
- **Pre-visitation Gate:** Run `npm run visit` before requesting visitation.
- **Neighbor check:** `npm run visit` must run `scripts/check-neighbors.mjs` so peer monasteries, canonical contract fields, and 5-point plans stay in line. Future contract breaks fail this gate.
- **Definition of Done:**
  1. `npm run lint` reports zero errors.
  2. `npm run test` passes with 100% green tests.
  3. `npm run build` compiles clean production bundle.

---

## §4 Security & Hardening
- **Zero Secrets:** Never commit credentials, private keys, or API tokens in client bundles or repositories.
- **Input Sanitization:** All external feeds, freeform user inputs, and uploaded files must be defensively sanitized.
- **Payload Limits:** Strict size bounds on all incoming network requests.

---

## §5 Design & Usability
- **Mobile-First:** Target 360px–430px viewports first; adapt cleanly to desktop.
- **Touch Targets:** All interactive buttons and triggers must have minimum 44px height (`min-h-[44px]` / `touch-target`).
- **Fluid Typography:** Responsive text scaling without manual breakpoint jumps.

---

## §6 Visitation (Separation of Duties)
- The author who wrote a change does NOT perform its final audit.
- An independent **Visitor subagent** receives only: `AGENTS.md`, the git diff, and the test results (no conversation history).
- **Verdicts:** `PASS` · `PASS WITH FINDINGS` · `BLOCK`
- **Finding Classes:**
  - `blocking`: Security vulnerability, data loss, contract breach. Must fix before merge.
  - `advisory`: Rule violation without data loss. Must fix or log in `DEBT.md`.
- **Fault Attribution:**
  - `house`: Code violates the Rule. Fix code.
  - `RULE`: The Rule is impractical or obsolete. Propose an amendment in `ROLL.md`.

---

## §7 Volatile Facts (Not in this file)
Volatile and transient facts must NOT be stored in `AGENTS.md`. Consult their single source of truth:
- Library versions: `package.json`
- Recent changes: `CHANGELOG.md` and git commit history
- Architectural decisions: `docs/` and `ROLL.md`
