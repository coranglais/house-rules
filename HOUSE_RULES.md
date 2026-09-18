# House Rules

Working agreements for solo development with AI coding agents.

> **Snapshot:** commentary on `RULES.md` @ `v1` — 2026-08-17 —
> github.com/coranglais/house-rules. Rules are referenced below by their
> permanent IDs (HR-001…HR-021); `RULES.md` is the source of truth for both
> wording and enforcement category.

These are the rules I start every project with. They exist because I am one
person, working in scattered sessions across three machines, directing agents
that hold context I can't see and sometimes report state they aren't actually
reading. A team with code review, CI, and colleagues who remember last Tuesday
would need maybe half of these, and in a different form.

Each rule is grouped by **how it is enforced**, because that turns out to matter
more than what the rule says. A rule the tooling enforces costs nothing to
follow. A rule that depends on me remembering costs attention every time, and
attention is the scarce resource.

The goal is to move rules leftward — from *"I remember to"* toward *"the tooling
won't let me."* Most of the value in this document is in that migration, not in
the list itself.

---

## Part 1 — Structural

Configure once. After that, the mistake is unavailable. These are the rules
worth the most, and the ones worth converting others into.

### Secrets and signing material never enter version control (HR-001, HR-002)

Keystores, key files, service-account JSON, `.env` files: gitignored before the
first commit that could contain them, not after. Passwords in a password
manager, never in a repo, a note file, or a commit message.

**Scope API keys to the tool that needs them.** A per-tool key with a narrow
grant is a smaller blast radius than one global key that everything shares, and
it can be rotated without breaking five unrelated things. (HR-002)

*Enforced by:* `.gitignore` committed at project init, secret-scanning enabled
on the remote.

### Deployment is bound to a branch that isn't `main` (HR-004)

Web deploys run from a dedicated `deploy` branch. `main` is where work
accumulates; `deploy` is what the world sees. Configuring the host to build only
from `deploy` means an unfinished commit on `main` physically cannot ship.

The general form: **if an action is irreversible or public, bind it to an
explicit, deliberate act** — a branch switch, a tag, a manual promotion. Never
to the default path you're already on.

*Enforced by:* host build configuration, not discipline.

### The analyzer is a gate, not a suggestion (HR-019)

Zero warnings, and the build doesn't proceed until that's true. "Analyzer-clean"
as an aspiration decays; as a gate it doesn't.

*Enforced by:* pre-commit hook or CI check.

### Commit messages are machine-parseable (HR-013)

Conventional Commits, always. Two reasons, and the second is the real one:
humans scan a log faster when subjects are typed, and **tooling can consume the
history**. My release pipeline reads commits to generate release notes. That
only works because every commit conforms.

*Enforced by:* `commit-msg` hook rejecting non-conforming subjects.

---

## Part 2 — Agent instructions

These live in `CLAUDE.md` (or your agent's equivalent) and they work insofar as
the agent reads and follows them. That's a real effect and worth having. It is
not the same as the mistake being impossible — agents drift, contexts go stale,
and instructions get outweighed by the momentum of a task.

### Recon before doing (HR-005, HR-006)

Any task involving judgment runs in two phases. **Phase 1 is read-only:** the
agent reports what it found, what it's uncertain about, and what decisions need
making. It stops there. **Phase 2 implements**, after I've read Phase 1 and said
go.

This is not ceremony. The recon is where you find out that the fix you're about
to build on top of isn't actually in the tree, or that the thing you assumed was
committed is sitting unstaged.

**The recon must be allowed to stop.** A recon phase that always ends in "looks
good, proceeding" has become a formality. Give it explicit stop conditions and
mean them. (HR-006)

### Verify against the tree, never against another document (HR-007)

The single most valuable rule here.

Documentation describes what was intended. Code describes what is. When an agent
reports that a fix is present, the acceptable evidence is a file and a line
number — not a backlog entry, not a plan document, not its own recollection.

*The scar, twice over:* a backlog item confidently described a fix that had
never landed. And separately, my export code has stamped every exported file
with `schemaVersion: 1` since the database schema moved to version 2 — a
hardcoded literal that no document contradicted because every document described
the intent. Those files are in users' hands and immutable. The wrong number is
permanent.

The corollary — recording *when* a claim was checked — is on me rather than on
the agent, so it lives in Part 3 (HR-016).

### The human does all pushes (HR-009)

Agents commit freely. Agents never push. The push is the review gate — the
moment I read what accumulated and decide it's real.

This one is doing more work than it appears to. It means local history is cheap
and disposable, so commits can be atomic and frequent without anyone worrying
about polluting a shared branch. The gate at the end is what makes the freedom
at the start safe.

### Agents don't touch remote or external state (HR-010)

No force-push, no history rewriting, no branch deletion, nothing that mutates a
remote. And — increasingly important as more configuration lives in vendor web
consoles — **no changing settings in cloud consoles, app stores, payment
dashboards, or DNS.** An agent can tell me exactly what to click. It doesn't
click.

### Verify the branch before committing (HR-011)

Trivial, and it catches a real class of error when work spans sessions and
machines.

---

## Part 3 — Human-only

No automation catches these. They cost attention every time, permanently. Worth
knowing which rules those are, so the attention goes where it's actually needed.

### Actually read the recon (HR-021)

The gate only works if someone walks through it. Approving Phase 2 without
reading Phase 1 converts the entire two-phase discipline into overhead.

### Don't trust an agent's report of state it isn't currently reading (HR-008)

Anything with cheap ground truth — `git status`, a file on disk, a command's
actual output — gets verified directly rather than accepted from a summary.

*The scar:* in a single session, my agent reported the commit count ahead of the
remote wrong three times, each time from stale context rather than from running
the command. Every number was plausible. One of them sent me looking for a
problem that didn't exist.

The agent-side instruction exists too (HR-008 is in the fragment), but the rule
lands here because the failure mode is me accepting the summary. Nothing catches
that except me re-running the command.

### Gates fail closed, and exceptions are never granted in the moment (HR-020)

When a check fires inconveniently, fix the condition — don't waive the check.

*The scar:* my agent's pre-build recon stopped because the working tree was
dirty. The offender was one untracked documentation file, obviously harmless.
The tempting fix was to tell the agent that untracked docs don't count. I
committed the file instead. A carve-out for "untracked docs" is correct exactly
until the untracked file is a source file someone forgot to add, and by then the
carve-out is load-bearing and invisible.

The gate itself can be structural. Not reaching for the waiver isn't — I'm the
one holding the pen that would write the "unless" clause, and that's why this
sits in Part 3 rather than Part 1.

### Commit messages explain *why* (HR-014, HR-012)

The subject says what changed. **The body says why, for whoever reads it next —
which is usually me, months later, having forgotten everything.**

*The scar:* a bug I spent a day diagnosing turned out to be one my past self had
already diagnosed and solved, in a project I'd stepped away from for six weeks.
The fix was in the code. The reasoning was in my head, and my head had moved on.
Verbose commit bodies are the cheapest durable documentation available, and they
live next to the thing they explain.

Atomic commits are the other half of this: one logical change per commit, so the
message can actually be about something. (HR-012)

### Decisions get logged, and supersession is explicit (HR-017)

When a considered decision is reversed, the old entry **stays** and the new one
says "Supersedes: [entry]." Deleting the old decision destroys the reasoning
trail, and the reasoning is what stops you re-deriving the same conclusion — or
worse, re-deriving the opposite one — in six months.

Log the *reasoning*, not just the conclusion. The conclusion is often obvious in
hindsight; the constraints that produced it are not.

### Environment-specific findings go in the repo (HR-018)

Non-obvious build flags. The symptom of a corrupted build directory. The
platform quirk that only bites on one OS. These are blindingly obvious for about
three weeks and then completely gone.

*The scar:* verifying a signed artifact required a specific version flag that,
without it, made the tool fail with an unrelated-looking parse error. I lost a
day to it once. The flag lived in my head for a month afterward and then I had
to work it out again.

### Anything consuming a permanent resource requires a deliberate human act (HR-003)

Some things can't be taken back. Build numbers a store has seen are reserved
forever, including from drafts you discarded. Published artifacts, exported files
in users' hands, released store copy, public API shapes — all one-way doors.

**No automation writes to these without a human in the loop**, and the human
should know at the moment of acting that the door only swings one way.

### Docs are canonical in the repo; every copy is a dated snapshot (HR-015)

Name the source of truth. Every other copy — an attachment in a chat tool, a
mirror on a website, a vendored copy in another project — is a snapshot, and it
**records the date and commit it was taken from.**

Copies drift silently. A copy with a date on it drifts visibly.

### A fact checked against code records *when* it was checked (HR-016)

The corollary to "verify against the tree." My backlog carries a `verified`
field for exactly this. A claim verified eight months ago and a claim verified
this morning are different kinds of claim and should not look identical.

The agent can be asked to stamp the date, and it usually does. Noticing that a
stamp has gone stale is the part that never gets automated.

---

### Values with an owner are read, not copied (HR-022)

Where something else already declares a value — an ORM's schema version, the
version string in `pubspec.yaml`, a path some config file owns — the code reads
it. A literal that restates it is a copy, and copies drift silently, same as
documents do.

*The scar, and it's the other half of the HR-007 one:* my Flutter app's JSON
export wrote `metadata.schemaVersion` as a literal `1` while the ORM schema had
moved to 2, and `appVersion` as a literal `'1.0.0'` while `pubspec.yaml` said
`1.0.0+3`. Neither literal had any link back to its source, so nothing could
complain. There was no disagreement anywhere to detect — just two numbers that
had been right once.

The sharper detail is that the metadata map was built twice, in two different
methods, so the same wrong literals existed in duplicate. The fix wasn't
correcting the numbers; it was collapsing the two construction sites into one
that reads both values from the things that own them. Correcting a literal
leaves you holding a literal.

This one was caught before release, which is the only reason it's a rule and not
a permanent fact. Export files are immutable once written — every file exported
before the fix would have carried the wrong numbers forever, in users' hands,
with no way to reach back and amend them (HR-003).

*Why it's here and not in Part 1:* I don't know of a lint that catches this in
general. The literal is well-formed, plausible, and syntactically unrelated to
the value it duplicates — there's nothing for a checker to compare it against.
Asking "who owns this number?" of every new constant is the only check I have,
and it costs attention every time.

---

## Applying this to a new project

1. Vendor this file to `docs/HOUSE_RULES.md` with a header line recording the
   date and source tag.
2. Point your agent config (`CLAUDE.md` or equivalent) at it, and inline the
   Part 2 rules directly — agents follow instructions in front of them more
   reliably than instructions behind a link.
3. Set up the Part 1 enforcement *before* the first real commit. Gitignore,
   branch protection, deploy branch binding, commit-msg hook. It takes twenty
   minutes and it's the only part that keeps working while you're not paying
   attention.

---

## A note on how to read this

Every rule here is generic advice until you attach the scar. "Verify against the
tree" is a shrug. "My export code has stamped the wrong schema version into
every file users have ever downloaded, and no document caught it because every
document described the intent" is a reason.

If you adopt these, adopt the ones where you can name your own cost. The rest
will feel like ceremony, and rules that feel like ceremony get waived exactly
when they matter.
