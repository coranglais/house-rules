# Agent rules

Source: `RULES.md` @ `v2.1` — 2026-10-29 — github.com/coranglais/house-rules

<!-- Extracted from RULES.md. Do not edit here — edit RULES.md and re-extract. -->

Paste into `CLAUDE.md` (or your agent's equivalent config), **including the source
line above** — so the copy in your project says where it came from.

---

**Recon before doing.** Any task involving judgment runs in two phases. Phase 1
is read-only: report findings, uncertainties, and decisions needing human
input, then stop. Phase 2 implements, only after explicit approval. (HR-005, recon-before-doing)

**The recon may stop.** Stop conditions in the prompt are real. If one fires,
report it and halt — do not work around it, and do not ask to have it waived.
(HR-006, recon-may-stop)

**Verify against the tree, never against another document.** When reporting that
code does something, cite the file and line. A plan document, a backlog entry,
or your own recollection is not evidence. If a document and the tree disagree,
the tree is right — say so explicitly. (HR-007, verify-against-tree)

**Do not report state you are not currently reading.** For anything with cheap
ground truth — git status, file contents, command output — run the command
rather than answering from context that may be stale. (HR-008, stale-state-reports)

**Commit freely. Never push.** The push is the human's review gate. (HR-009, human-pushes)

**Do not mutate remote or external state.** No force-push, no history rewriting,
no branch deletion. No changes to cloud consoles, app stores, payment
dashboards, or DNS. Describe what the human should click; do not click it.
(HR-010, human-clicks)

**Verify the current branch before committing.** (HR-011, branch-before-commit)

**Atomic commits.** One logical change per commit. (HR-012, atomic-commits)

**Conventional Commits for subjects.** History is parsed by tooling as well as
read by humans. (HR-013, conventional-commits)

**Commit bodies explain why**, not what. Write for someone returning to this
months from now with no memory of the session. (HR-014, commit-bodies-why)

**Remind the human to re-sync vendored doc copies** after changing any canonical
document in `docs/`. (HR-015, docs-canonical-in-repo)

**Record when a claim was checked against code**, not just that it was. (HR-016, checked-fact-dated)

**Log decisions with their reasoning**, not just the conclusion. A reversal adds
a new entry saying "Supersedes: [entry]" — never edit or delete the old one.
(HR-017, decisions-logged)

**Write environment-specific findings into the repo.** Non-obvious build flags,
platform quirks, the symptom of a corrupted build directory: these belong in a
file, not in the session that discovered them. (HR-018, env-findings-in-repo)

**Read owned values at runtime; never write them as literals.** Schema versions,
version stamps, build numbers, paths another config already declares: if another
source of truth owns a value, read it from there. A constant that duplicates it
stays correct until the source moves once, and nothing signals when it does.
(HR-022, read-owned-values)

**Verify migrations against a restore of production, never an empty database.**
Restore the production dump at the deployed commit, then migrate to the new
commit. An empty database has no rows for a migration to get wrong.
(HR-023, migration-verification)

**Never renumber or reuse rule IDs.**
