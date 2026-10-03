# house-rules — Backlog

<!-- format: house-rules backlog template v1.2 -->

Deferred engineering work, parked findings, and known issues. Items enter here
when they are real but not blocking; they leave the live queue when they ship or
are explicitly declined, moving to **Closed** at the bottom of this file with a
`resolution` field. They are not deleted and ids are never reused — the
reasoning is the point, and a reader who finds no trace of a question will
re-derive it from scratch. A declined item whose rationale is *strategic* rather
than merely technical records that rationale in the item's own Decision block
(see Dated addenda); this repo has no separate decisions log.

This file is the queue of record. If it isn't here, it isn't tracked — chat
transcripts are not storage.

**Scope:** engineering items only. Nothing about this repo lives outside it, so
there is no out-of-repo queue to point at.

**Dropped from the template at instantiation:** the milestone tag (this repo has
no release path), the separate decisions log (decisions live in items' Decision
blocks), and the Non-code section (nothing here is out-of-repo). Instantiated
from template v1.2 on 2026-09-17.

**Companion docs:** `README.md` describes the files and the versioning scheme.
There is no spec and no strategy doc; `RULES.md` is the content, and this file
is the list of things known to be wrong, missing, or deferred about it and its
derived files.

## Log

Dated snapshots of what happened to this file as a whole: initial state,
verification passes, bulk triage. **Append-only.** An entry describes the file
as it was on that date and is never edited to stay current; a later entry
supersedes it. A reader who finds "no `agent` values changed" in a March entry
and a changed `agent` value in an April item should not be surprised, and a
parser should not treat any entry here as a claim about the present.

**2026-09-17 — Initial state:** two items, both tree-checked at entry, sourced
from Claude Code observations during the v2 re-sync session (the one that
brought `CLAUDE_FRAGMENT.md` and `HOUSE_RULES.md` up to tag `v2`). No
conflicts.

**2026-09-17 — Template feedback filed:** item 003 added, from the feedback
that came out of instantiating this file. 001 and 002 unchanged.

**2026-10-03 — v2.1 cut filed:** item 004 added under Near-term, from what a
tracker with a non-code section taught between 2026-09-20 and 2026-10-02.
Its claims about this repo were tree-checked at entry; its source items were
read in the tracker's `docs/BACKLOG.md`. 001, 002 and 003 unchanged; 004
cross-references all three.

---

## Field conventions

Each item carries the same fields, in the same order. Machine-parseable by
design: this file is intended to feed an agent-driven triage or backlog
framework later, and the `agent` field is the triage decision that framework
will consume.

- **`id`** — stable, never reused. Zero-padded numeric, assigned in order of
  entry. An item filed retroactively (fix landed before the entry existed)
  still gets the next id; say so in `entered`.
- **`area`** — subsystem. One of: `rules` (RULES.md content), `derived`
  (fragment, essay, vendored-copy headers), `tooling`, `docs`. Nothing else.
- **`priority`** — `blocker` (cannot ship), `near-term` (ship-soon window),
  `deferred` (no date), `latent` (not currently reachable; guard against
  future reachability).
- **`agent`** — delegation triage. `yes` = mechanically verifiable, local
  change, small diff. `no` = requires a judgment call, a product decision, or
  touches something outside the repo. `triage` = not yet decided.
  **A `yes` here never authorizes a push.** Pushes remain the human review gate.
  When a value changes, say why in the field itself (`yes — scoped {date};
  see Decision below`) so the history is on the line, not in git blame.
- **`verify`** — what proves the item done. An item with no verification story
  cannot be `agent: yes`. If an item has two halves with different verification
  stories, that is the signal to split it — or to say explicitly why it is one
  item.
- **`source`** — where the finding came from (audit, dogfooding, code review,
  a doc section, a conversation), so the reasoning is recoverable.
- **`entered`** — when the item was added. Always present.
- **`verified`** — when this item's claims were last checked **against the
  tree**, as distinct from against another document. Cite the file and line
  or the commit that confirms the claim. `n/a` for items with nothing
  in-repo to check — and say what the item is about instead. `not checked` is
  the honest value for an item that has never been tree-checked; it is a valid
  state, not a placeholder to hide.
- **`blocks`** — optional. What this item stands in front of: a feature, a
  doc section, another item. Present only when something real is waiting.
- **`resolution`** — present only on items under Closed: `shipped` or
  `declined`, with the date, the commits, and a one-line outcome. Live items
  omit it. A closed item keeps its original fields as the historical record
  of what was wanted, with `priority` rewritten as `was {value} — no longer
  applicable` so nothing consuming this file mistakes it for live work.

`verified` is the one that earns its place. An item sourced from another doc
and never tree-checked is how an already-fixed bug arrives as a live entry and
how an unbuilt screen gets described as shipped. A stale item looks exactly as
fresh as a live one without it, and an agent consuming this file takes every
entry at face value. **Backlog items sourced from another doc are checked
against the tree before entry, not inherited on the doc's authority.**

### Dated addenda

An item accumulates understanding while it is live: what the bug actually is,
what the tree actually contains, what was decided. That understanding goes in
the item body as dated addenda, appended in order below the original body,
never rewritten. Three kinds, each a bold heading on its own line:

- **`Diagnosis ({date})`** — what investigation found: production data,
  reproduction, the actual mechanism. Replaces hypotheses in the original
  body without deleting them; the reader should be able to see what was
  guessed and what turned out to be true.
- **`Finding ({date})`** — what a tree check found when it disagrees with
  the item's original claim. The claim stays; the finding corrects it.
- **`Decision ({date})`** — a scope or design choice, with the alternative
  considered and why it lost. **For a project without a separate decisions
  log, this block is the decisions log**: a strategic decline lives here on
  the closed item, and the header's decisions-log reference should say so.

The original body is written once, for a reader with none of the conversation.
Addenda are written the same way. An item with three addenda is not messy; it
is an item that was understood in stages, and the stages are the record. An
addendum that would change `agent`, `priority`, `verify`, or `verified` says
so at its end and the field is updated to match. A Finding is a tree check by
definition, so it always updates `verified` to its own date.

### Item template

```markdown
### {id} — {One-line title stating the problem, not the fix}

- **area:** {area}
- **priority:** {priority}
- **agent:** {yes | no | triage} — {reason, if no or triage}
- **verify:** {what proves it done}
- **source:** {origin}, {date}
- **entered:** {date}
- **verified:** {date} — {file:line, commit, or n/a — what}
- **blocks:** {optional}

{Body. What is wrong or missing, where in the tree, what the consequence
is, and anything the fixer needs to know that is not obvious from the diff —
including things NOT to do by reflex. Cross-reference related items by id.
Write it for a reader who has none of the conversation that produced it.}

**Diagnosis ({date})**

{Optional. See Dated addenda.}

**Finding ({date})**

{Optional. See Dated addenda.}

**Decision ({date})**

{Optional. See Dated addenda.}
```

---

## Near-term

There is no separate Blockers section; if a blocker exists, it is the first
item here.

### 004 — Tag `v2` predates the fragment that claims it, and the tracker's lessons are not in the rules or the template; cut v2.1

- **area:** rules — the cut is a `RULES.md` tag. Parts 1 and 2 touch
  `derived` and `docs` (the template); they are one item because they ship
  as one tag.
- **priority:** near-term
- **agent:** triage — parts 1 and 2 are mechanical once scoped; parts 3 and
  4 are judgment calls about rule content, and the tag name is the open
  question 001 already holds.
- **verify:** a `v2.1` tag exists, and at that tag `CLAUDE_FRAGMENT.md` and
  `HOUSE_RULES.md` name `v2.1` and the fragment carries every rule the
  inclusion rule says it should; `templates/BACKLOG_TEMPLATE.md` is at v1.3
  with the three additions in part 2 and its header changelog lists them;
  the secret-ordering rule is in `RULES.md` under a new id, or a Decision
  here says why not; every rule in `RULES.md` has been asked the part 4
  question and each demotion or keep for HR-013 is recorded in a Decision
  here.
- **source:** a tracker with a non-code section: its `docs/BACKLOG.md`
  items 013, 020 and 024 and their closes, work of 2026-09-20 to 2026-10-02;
  relayed by the maintainer 2026-10-03
- **entered:** 2026-10-03
- **verified:** 2026-10-03 — `git rev-parse v2^{commit}` is 921754e;
  `git show v2:CLAUDE_FRAGMENT.md` line 3 reads `RULES.md @ v1` and has no
  HR-022; `CLAUDE_FRAGMENT.md:3` and `:63` at main read `v2` and HR-022
  (c492a56); `HOUSE_RULES.md:5` reads `v2` (9aba781);
  `git diff --stat v2 HEAD` lists no change to `RULES.md`;
  `git ls-remote --tags origin` shows `v2` published;
  `templates/BACKLOG_TEMPLATE.md:4` is v1.2, `:112-116` (`resolution`),
  `:229-233` (transferable mechanism); `RULES.md:86-87` (HR-013),
  `RULES.md:21-25` (the three `Enforced` values). Source items read in the
  tracker at 1b90c4a: `docs/BACKLOG.md:1601` (013), `:1881` (020),
  `:2163` (024).

Four things, learned on one project in two weeks, that belong in one tag.

**1. The `v2` tag and the fragment disagree.** `v2` points at 921754e, the
merge that brought HR-022 into `RULES.md`. The fragment was re-extracted one
commit later, in c492a56, and the essay's snapshot header was bumped in
9aba781. So the tree at `v2` holds a fragment whose provenance line says `v1`
and which has no HR-022, while the fragment at main says it was taken from
`v2`. Anyone who checks out the tag, or vendors the fragment from it, gets the
old one. `RULES.md` itself is unchanged between `v2` and main, so the README's
staleness check is unaffected; the mismatch is only in the derived files.

`v2` is already on the remote. Moving a published tag is the thing not to do
by reflex: a clone that has fetched it keeps the old target silently. A new
tag on a commit where the derived files and their headers agree is the fix,
and "retag" in the source is read here as that.

**2. Backlog template v1.3.** Three additions, each something the tracker's
closes did that the template does not ask for:

- **The transferable mechanism paragraph on closed items states the general
  hazard.** v1.2 already has the paragraph (`:229-233`, "where the fix taught
  something that outlives the bug"). The tracker's closes wrote one on every
  item cited here, and each names a hazard recognizable in unrelated code
  (013: "A refusal to start is only as safe as the order it is deployed
  in"). What v1.3 changes relative to the existing text, whether stronger
  wording, an example, or a slot in the item template, is for triage.
- **A resolution names which verify clause was checked where**: in the
  tests, on a restore, or on production. 020 and 024 both do it ("met on a
  restore of the 2026-10-01 production dump (Finding), not on production").
  The `resolution` description (`:112-116`) asks only for date, commits and
  a one-line outcome, which lets "shipped" cover a clause nobody saw run in
  production.
- **Migrations are tested against a restore of the production dump, started
  from the deployed commit, not against an empty database.** 020 and 024 ran
  in two stages: the deployed commit on the restore, then the new commit on
  the same file. An empty database has no rows for a migration to get wrong.

003 already holds four v1.3 candidates and a trigger ("not on its own"). These
three are not wording-only, so they are the non-wording change that trigger
waits for; 003's four ride along, and the two items close together.

**3. New rule candidate: a required secret ships in a fixed order.** Apply
the variable, confirm the service restarted with it, then push the code that
refuses to start without it. On Railway a variable added in the console is
staged, and only a deploy started from the console applies it; a push starts
a deploy that reads the applied set. Tracker 013 added a fail-closed check on
`SESSION_SECRET`, the variable was staged and not applied, and the
git-triggered deploy crashed. The crash was the check working; the fix is the
order, never a default value.

The source calls this a structural candidate. As `RULES.md:23` defines
`structural` ("configured once; the mistake is then unavailable"), an
ordering of three manual steps is `human` unless something enforces it, such
as a deploy script that refuses when the variable is absent from the applied
set. Triage decides the `Enforced` value, whether the rule names Railway or
states the general case with Railway as the instance, and whether it belongs
in the fragment under the inclusion rule (`RULES.md:10-15`). It takes the
next unused HR id.

**4. Reread `RULES.md` asking "which of these has a scar behind it".** A rule
with an incident behind it stays a rule; one without is demoted to advice.
HR-013 (Conventional Commits, `RULES.md:86-87`) may be one. Two things to
settle first: `RULES.md` has no "advice" category today, only three
`Enforced` values and `Retired`, so demotion needs a form (a section, a
marker, or a move to the essay); and ids are permanent, so a demoted rule
keeps its number. Do not delete.

**The tag name.** Parts 3 and 4 change rule content. 001 describes a
patch-style `v2.1` as a wording-only tag the README does not yet define, and
that reading does not fit a cut that adds a rule and demotes others. Either
the README's Versions section defines `vN.x` to cover this, or the cut is
`v3` and only part 1 is a `v2.1`. 001's one-word HR-015 fix shares whichever
tag this is. 002's revisit trigger (the fragment "found to have actually
drifted") is not met by part 1: the fragment lagged a tag by one commit, it
did not drift from `RULES.md`.

---

## Deferred

### 001 — HR-015 says "commit"; every derived file and the README use a tag

- **area:** rules
- **priority:** deferred
- **agent:** no — the edit is one word, but changing `RULES.md` means a new
  tag, and whether a wording-only change is `v3` or a patch-style `v2.1` is
  a human call about the versioning scheme, not about the wording.
- **verify:** HR-015 reads "tag"; a tag exists whose `RULES.md` contains the
  fix; the README's Versions section says how wording-only tags are handled.
- **source:** Claude Code observation during the v2 re-sync session,
  2026-09-17
- **entered:** 2026-09-17
- **verified:** 2026-09-17 — `RULES.md:96-97` says "date and commit";
  `RULES.md:7` and `RULES.md:136`, `README.md:45`, `README.md:57`,
  `README.md:66`, `CLAUDE_FRAGMENT.md:3`, and `HOUSE_RULES.md:5` all use a
  tag.

HR-015 (`RULES.md:96-97`) says every non-canonical copy of a doc "records the
date and commit it was taken from". Everything else in the repo says tag:
the `RULES.md` preamble at line 7 ("name the tag they were derived from"), its
Adopting section at line 136 ("date and source tag"), the README's Versions
section at line 45 (tag "rather than a commit SHA", with the reason), the
staleness check at line 57 (`git diff <tag> HEAD -- RULES.md`), the README's
Using section at line 66, and both derived-file headers, which read
`RULES.md @ v2`. A reader following HR-015 literally would stamp a SHA on a
vendored copy, and the staleness check the README documents would then not
apply to it.

The fix is one word. The cost is that `RULES.md` is tagged each time it
changes (README line 44), so the word cannot land without a decision about
what the tag is called:

- **New `v3`.** Consistent with the scheme as written. But `v3` looks like a
  content change, both derived-file headers and every vendored copy show as
  stale against it, and the diff a consumer runs shows a single word. The
  release note has to carry the weight of saying "nothing moved".
- **Patch-style `v2.1` for wording-only changes.** Cheap and honest about
  scale, but the README's Versions section does not define such a thing.
  It would need a sentence saying that a `vN.x` tag changes wording only and
  that a copy at `vN` is not stale for rule content. Consumers running the
  staleness check need to know that too, or the non-empty diff reads as a
  rule change.

Either way the derived-file headers move to the new tag. Cross-reference 002:
option (a) there also changes `RULES.md`, and if both land at once they share
a tag.

### 002 — CLAUDE_FRAGMENT.md is hand-maintained with no drift signal

- **area:** derived
- **priority:** deferred
- **agent:** no — scope decision between a generator and a check, and either
  choice constrains how `RULES.md` is written.
- **verify:** n/a until scoped.
- **source:** Claude Code observation during the v2 re-sync session,
  2026-09-17
- **entered:** 2026-09-17
- **verified:** 2026-09-17 — inclusion rule is prose at `RULES.md:10-15`,
  not a field; HR-022 is `Enforced: human` at `RULES.md:130` and present in
  the fragment at `CLAUDE_FRAGMENT.md:59-63`; `git log --all --name-only`
  across all ten commits lists seven paths and no script.

`CLAUDE_FRAGMENT.md` is a curated subset of `RULES.md`, reworded into
agent-facing instructions. Its own header says it was "extracted" and the
README says "re-extract", but there is no extractor: no script has ever been
committed on any branch. The inclusion rule (`RULES.md:10-15`) is a judgment,
"an agent could follow it or break it by its own action inside a session",
and it is explicitly a different axis from `Enforced`. HR-022 is
`Enforced: human` and is in the fragment; HR-019 through HR-021 are not.
So nothing in `RULES.md` today says, per rule, whether the fragment should
contain it, and nothing checks that the fragment matches what it should.

The consequence is that the fragment drifts silently. The v2 re-sync was a
manual read of fifteen entries against the source, and the only reason it
was done at all was that the provenance line said `v1` while the tag said
`v2`. A rule reworded in `RULES.md` without a tag bump, or a rule added and
forgotten, would not trip anything.

Two options, not yet weighed:

- **(a) A generator.** Needs an explicit agent-facing marker added to each
  rule in `RULES.md` first (a `Fragment: yes` field or equivalent), which is
  a `RULES.md` change and so a new tag; see 001. It also means the fragment's
  agent-facing prose has to live in `RULES.md` or be derived from it, since
  the current fragment rewrites each rule rather than quoting it.
- **(b) A drift check.** The prose stays hand-written. A check fails when a
  marked rule is missing from the fragment or a fragment entry cites a rule
  that is not marked. With no marker it can still do less: every `(HR-nnn)`
  the fragment cites exists in `RULES.md`, and the fragment's provenance tag
  is the current tag. That weaker form needs no `RULES.md` change.

Both were reached for once already: the 2026-08-31 session was briefed on the
assumption that a generator existed, and stopped when it found none. Nothing
went wrong, but the assumption was made once and will be made again.

**Revisit trigger:** the first time the fragment is found to have actually
drifted from `RULES.md`, as opposed to lagging a tag bump. Until then the
manual re-sync at each tag is the process.

### 003 — Backlog template v1.3 candidates

- **area:** docs
- **priority:** deferred
- **agent:** yes — all four changes are wording, mechanical, and local to
  `templates/BACKLOG_TEMPLATE.md`.
- **verify:** template at v1.3 contains all four; the header changelog lists
  them; "Changes in 1.3" states they are backward-compatible.
- **source:** Claude Code instantiation feedback on this repo's
  `docs/BACKLOG.md`, 2026-09-17
- **entered:** 2026-09-17
- **verified:** 2026-09-17 — `templates/BACKLOG_TEMPLATE.md:55-57`
  (Companion docs), `:51-53` (Scope), `:53`, `:87-89`, `:156`, and
  `:255-258` (the three milestone-tag sites and the Adapting bullet),
  `:114-115` (`resolution` wording).

Four wording changes surfaced by instantiating v1.2 against a repo with no
spec, no strategy doc, no release path, and nothing out-of-repo. None changes
a field, a section, or a convention; a v1.2 instance would need no edits.

1. **Companion docs** (`:55-57`). The placeholders assume a `PLAN.md` and a
   `STRATEGY.md` exist. Say what to write when neither does, rather than
   leaving the instantiator to invent a sentence.
2. **Scope** (`:51-53`). "Name what lives elsewhere" has no answer when
   nothing does. Same fix: a sentence for the empty case.
3. **Adapting section** (`:255-258`). Dropping the milestone tag touches
   three places: the Scope paragraph (`:53`), the `priority` description
   (`:87-89`), and the item template (`:156`). The bullet that says the tag
   is optional should list them, so the instantiator does not search.
4. **`resolution` description** (`:114-115`). "As the historical record of
   what was wanted" is the last echo of the "What was wanted" paragraph that
   v1.2 removed from Closed. Tidy the phrase so the field description no
   longer names something the format no longer has.

Two observations from the same feedback were considered and are **not**
candidates. Recorded here so they are not re-raised:

- **The conventions text dwarfs a small queue.** True, and deliberate.
  Copy-once means a vendored copy has no template to hand, so the conventions
  travel with the instance. The trade is size against self-containment, and
  self-containment wins.
- **Empty sections hold a "None" line.** Three lines each. Keeping every
  section present, in order, is what makes the section set predictable for a
  parser and for a reader comparing two instances. Not padding.

**Trigger:** bump to 1.3 at the next instantiation, or when a non-wording
change forces a version anyway. Not on its own; four wording fixes do not
earn a tag by themselves.

---

## Latent

None. Each entry here would be a guard against a future change making it
reachable, with the item that would make it so cross-referenced.

---

## Closed

Shipped or declined. Kept rather than deleted, with ids never reused, so that a
question already settled is not re-derived from scratch. Nothing here is live
work.

A closed entry keeps the original fields (with `priority` marked no longer
applicable and `resolution` added), the original body, and any dated addenda
accumulated while the item was live, all in place. The body was written once
and is not restated on close. Where the fix taught something that outlives the
bug, a **transferable mechanism** paragraph follows the addenda, stating the
general hazard in a form the next reader can recognize in unrelated code.

None yet.
