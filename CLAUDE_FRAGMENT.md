# Agent rules

Source: `RULES.md` @ `v2` — 2026-09-17 — github.com/coranglais/house-rules

<!-- Extracted from RULES.md. Do not edit here — edit RULES.md and re-extract. -->

Paste into `CLAUDE.md` (or your agent's equivalent config), **including the source
line above** — so the copy in your project says where it came from.

---

**Recon before doing.** Any task involving judgment runs in two phases. Phase 1
is read-only: report findings, uncertainties, and decisions needing human
input, then stop. Phase 2 implements, only after explicit approval. (HR-005)

**The recon may stop.** Stop conditions in the prompt are real. If one fires,
report it and halt — do not work around it, and do not ask to have it waived.
(HR-006)

**Verify against the tree, never against another document.** When reporting that
code does something, cite the file and line. A plan document, a backlog entry,
or your own recollection is not evidence. If a document and the tree disagree,
the tree is right — say so explicitly. (HR-007)

**Do not report state you are not currently reading.** For anything with cheap
ground truth — git status, file contents, command output — run the command
rather than answering from context that may be stale. (HR-008)

**Commit freely. Never push.** The push is the human's review gate. (HR-009)

**Do not mutate remote or external state.** No force-push, no history rewriting,
no branch deletion. No changes to cloud consoles, app stores, payment
dashboards, or DNS. Describe what the human should click; do not click it.
(HR-010)

**Verify the current branch before committing.** (HR-011)

**Atomic commits.** One logical change per commit. (HR-012)

**Conventional Commits for subjects.** History is parsed by tooling as well as
read by humans. (HR-013)

**Commit bodies explain why**, not what. Write for someone returning to this
months from now with no memory of the session. (HR-014)

**Remind the human to re-sync vendored doc copies** after changing any canonical
document in `docs/`. (HR-015)

**Record when a claim was checked against code**, not just that it was. (HR-016)

**Log decisions with their reasoning**, not just the conclusion. A reversal adds
a new entry saying "Supersedes: [entry]" — never edit or delete the old one.
(HR-017)

**Write environment-specific findings into the repo.** Non-obvious build flags,
platform quirks, the symptom of a corrupted build directory: these belong in a
file, not in the session that discovered them. (HR-018)

**Read owned values at runtime; never write them as literals.** Schema versions,
version stamps, build numbers, paths another config already declares: if another
source of truth owns a value, read it from there. A constant that duplicates it
stays correct until the source moves once, and nothing signals when it does.
(HR-022)

**Never renumber or reuse rule IDs.**
