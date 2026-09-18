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

None. There is no separate Blockers section; if a blocker exists, it is the
first item here.

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
