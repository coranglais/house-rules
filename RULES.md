# Rules

Canonical working agreements for solo development with AI coding agents.

**This file is the source of truth.** `CLAUDE_FRAGMENT.md` is extracted from it.
`HOUSE_RULES.md` is annotated commentary on it and carries a snapshot reference.
Both name the tag they were derived from, so a copy that leaves this repo still
says where it came from.

**What belongs in the fragment.** A rule goes in `CLAUDE_FRAGMENT.md` if an agent
could follow it or break it by its own action inside a session. That is a
different axis from `Enforced` — some `human` rules describe work an agent
actually does (writing the commit body, stamping a verification date), and some
`structural` rules still need the agent to know the shape (commit subjects).
Rules that are configured once, or that only I can perform, stay out.

**IDs are stable and permanent.** Never reused, never renumbered — retired rules
keep their number and are marked `Retired`. The essay and any vendored copies
cross-reference by ID.

**Every rule carries a slug** after its id — kebab-case, two to four words,
naming the rule's subject rather than its current wording. Slugs are names:
assigned once, never changed, never reused; a retired rule keeps its slug. The
first mention of a rule in any prompt, commit, or doc is `id (slug)`, or
`(id, slug)` inside an existing parenthetical — a bare id is a pointer only a
machine can follow.

**`Enforced` is a field, not part of the ID**, because rules are meant to move:

- `structural` — configured once; the mistake is then unavailable
- `agent` — an instruction in the agent's context; followed insofar as it's read
- `human` — depends on attention every time

Moving a rule from `human` or `agent` to `structural` is the main way this
document improves. A rule that can't be moved is a rule to be honest about.

---

## Secrets and irreversibility

**HR-001 (secrets-gitignored)** — Secrets, keystores, key files, and
service-account credentials are gitignored before the first commit that could
contain them. `Enforced: structural`

**HR-002 (scoped-api-keys)** — API keys are scoped to the tool that uses them.
No shared global key. `Enforced: structural`

**HR-003 (reserved-resources)** — Anything that consumes a permanently reserved
resource requires a deliberate human act: published build numbers, released
artifacts, exported files in users' hands, public API shapes. No automation
writes to these unattended. `Enforced: human`

**HR-004 (irreversible-explicit-act)** — Irreversible or public actions are
bound to an explicit act — a dedicated branch, a tag, a manual promotion — never
to the default path. Web deploys build from `deploy`, never `main`.
`Enforced: structural`

---

## Agent conduct

**HR-005 (recon-before-doing)** — Recon before doing. Tasks involving judgment
run in two phases: read-only findings, then implementation after human approval.
`Enforced: agent`

**HR-006 (recon-may-stop)** — The recon phase must be able to stop. Stop
conditions are explicit and are not waived in the moment. `Enforced: agent`

**HR-007 (verify-against-tree)** — Verify against the tree, never against
another document. Claims that code does something are evidenced by file and
line, not by a plan, a backlog, or recollection. `Enforced: agent`

**HR-008 (stale-state-reports)** — Don't accept an agent's report of state it
isn't currently reading. Anything with cheap ground truth gets verified by
running the command. `Enforced: human`

**HR-009 (human-pushes)** — Agents commit. The human pushes. The push is the
review gate. `Enforced: agent`

**HR-010 (human-clicks)** — Agents do not mutate remote or external state: no
force-push, no history rewriting, no branch deletion, no changes to cloud
consoles, app stores, payment dashboards, or DNS. An agent may say what to
click; it does not click. `Enforced: agent`

**HR-011 (branch-before-commit)** — Verify the current branch before committing.
`Enforced: agent`

---

## Commits and history

**HR-012 (atomic-commits)** — Atomic commits. One logical change per commit.
`Enforced: human`

**HR-013 (conventional-commits)** — Commit subjects follow Conventional Commits,
so history is scannable by humans and parseable by tooling.
`Enforced: structural`

**HR-014 (commit-bodies-why)** — Commit bodies explain *why*, for whoever reads
them next. `Enforced: human`

---

## Documentation

**HR-015 (docs-canonical-in-repo)** — Docs are canonical in the repo. Every
other copy is a snapshot and records the date and commit it was taken from.
`Enforced: human`

**HR-016 (checked-fact-dated)** — A document recording a fact checked against
code records *when* it was checked. `Enforced: human`

**HR-017 (decisions-logged)** — Decisions are logged with their reasoning.
Reversals supersede rather than overwrite; the superseded entry stays.
`Enforced: human`

**HR-018 (env-findings-in-repo)** — Environment-specific findings — non-obvious
flags, platform quirks, the symptom of a corrupted build directory — live in the
repo, not in memory. `Enforced: human`

---

## Quality gates

**HR-019 (analyzer-clean)** — The analyzer runs clean. Zero warnings, and the
build does not proceed otherwise. `Enforced: structural`

**HR-020 (gates-fail-closed)** — Gates fail closed. When a check fires
inconveniently, fix the condition, not the check. `Enforced: human`

**HR-021 (read-the-recon)** — Read the recon before approving. A gate nobody
walks through is overhead. `Enforced: human`

**HR-023 (migration-verification)** — A migration or data change is verified
against a restore of the production dump: restore the production dump at the
deployed commit, then migrate to the new one. Never against an empty database —
an empty database has no rows for a migration to get wrong. `Enforced: human`

---

## Source of truth

**HR-022 (read-owned-values)** — Values owned by another source of truth are
read at runtime, never written as literals: schema versions, version stamps,
build numbers, paths that another config already declares. A constant that
duplicates a value owned elsewhere stays correct until the source moves once,
and nothing signals when it does. `Enforced: human`

---

## Adopting this

1. Vendor this file to `docs/RULES.md`, header recording the date and source tag.
2. Inline `CLAUDE_FRAGMENT.md` into `CLAUDE.md` — agents follow instructions in
   front of them more reliably than instructions behind a link.
3. Configure the `structural` rules before the first real commit. That's the part
   that keeps working while nobody is paying attention.
