<!--
  BACKLOG_TEMPLATE.md — a structured engineering backlog format.
  Source: house-rules repo, templates/BACKLOG_TEMPLATE.md
  Template version: 1.3 (2026-10-28). v1.0 was extracted from Cuando's
  BACKLOG.md; v1.1 adds what a second instantiation (MGP) needed; v1.2
  tightens two conventions; v1.3 folds in what two further instances (a
  tracker with a non-code section, and this repo's own backlog) needed.

  Copy-once, not synced. Instantiate as docs/BACKLOG.md, fill in the
  {placeholders}, delete this comment and the "Adapting this template"
  section at the bottom. Record the template version in the header line
  below so a later revision of the format can be diffed against it.

  Changes in 1.3:
  - `gates` joins the fields, optional, after `priority`: the release track
    an item must be resolved before. Track names are instance-defined and
    stated in the instance's conventions. Orthogonal to `priority`, and
    distinct from the milestone tag: `gates` names the stage an item stands
    in front of; the tag only groups items that straddle code and non-code.
    An instance using `gates` usually does not need the tag.
  - `id` allows a letter-prefixed sequence for the Non-code section,
    instance-defined, separate from the numeric one, never reused.
  - Every item heading carries a kebab-case slug after the id:
    `### {id} ({slug}) — {title}`. Required on new items; an older item gets
    one the next time it is touched; untouched older items may stay bare.
    Slugs are names, not summaries: two to four words, set once, never
    changed or reused. Cite items as `id (slug)`.
  - `priority` may carry an inline reason after an em dash, as `agent`
    already does. On close the whole original value, annotation included,
    is kept inside `was {value} — no longer applicable`.
  - `resolution` names where each `verify` clause was seen met: tests, a
    restore of production data, or production itself.
  - The transferable-mechanism paragraph gets a slot in the item template
    and a sentence under `resolution`.
  - `verify` cites HR-023 (migration-verification) for migrations and data
    changes rather than carrying the rule; the rule lives in RULES.md.
  - Dated addenda: a correction to the original claim is a Finding. There is
    no fourth kind.
  - `verified`: `pending` is not a value; use `not checked`.
  - `area` values may each carry a one-phrase description.
  - Header gains an instantiation-comment slot below the format line: source
    repo and tag, date, parts dropped, decisions-log location.
  - `entered`: an instance whose founding batch predates the field states
    the id range and batch date once in its conventions.
  - Tree-check rule gains: an instance whose initial population was
    assembled unchecked says so in its first Log entry; that is the one
    deliberate exception.
  - Field values and tags are written unquoted in field lines; a consumer
    keys on the field line (`- **{field}:** ...`), never on a value
    appearing anywhere in the file.
  - Four wording fixes: the Companion docs and Scope paragraphs state the
    empty case; the milestone-tag bullet in Adapting lists the three places
    the tag appears; the `resolution` description no longer names the
    "what was wanted" paragraph that 1.2 removed.
  All fifteen are backward-compatible: every change is an optional field or
  a documented practice, and a v1.1 or v1.2 instance needs no edits.

  Changes in 1.2:
  - Closed: the "What was wanted" paragraph is gone. The original body is
    written once and never rewritten, so restating it on close was a
    duplicate. A closed entry keeps its fields, body, and addenda in place,
    followed where warranted by the transferable-mechanism paragraph.
  - Dated addenda: `verified` joins the fields an addendum may update, and a
    Finding always sets `verified` to its own date.
  - Item template: a Finding placeholder joins Diagnosis and Decision.
  All three are backward-compatible: a v1.1 instance needs no edits.

  Changes in 1.1:
  - Dated addenda convention inside items (Diagnosis / Finding / Decision).
  - Header log is append-only; entries are dated snapshots, never edited.
  - Decisions log may be in-item Decision blocks when no STRATEGY.md exists.
  - Non-code section: guidance on renaming or splitting when it becomes a
    grab bag.
  - "Adapting this template" now opens with the over-fitting note.
  Nothing from 1.0 was removed.
-->

# {Project} — Backlog

<!-- format: house-rules backlog template v1.3 -->
<!-- instantiated from: {source repo}, tag {template tag}; {date}; dropped:
     {parts dropped, or "nothing"}; decisions log: {file, or "Decision blocks"} -->

Deferred engineering work, parked findings, and known issues. Items enter here
when they are real but not blocking; they leave the live queue when they ship or
are explicitly declined, moving to **Closed** at the bottom of this file with a
`resolution` field. They are not deleted and ids are never reused — the
reasoning is the point, and a reader who finds no trace of a question will
re-derive it from scratch. A declined item whose rationale is *strategic* rather
than merely technical also earns an entry in the project's decisions log
(`{docs/STRATEGY.md or equivalent — or "the item's own Decision block", see
Dated addenda}`): this file records that the item is closed, that file records
why the project went the other way.

This file is the queue of record. If it isn't here, it isn't tracked — chat
transcripts are not storage.

**Scope:** engineering items only. {Name what lives elsewhere — release
logistics, console forms, dashboards — and where. If nothing does, say so:
"Nothing about this project lives outside the repo, so there is no
out-of-repo queue to point at."} A few items straddle both and are marked
with the `{milestone-tag}` tag.

**Companion docs:** `{docs/PLAN.md}` is the spec, `{docs/STRATEGY.md}` is the
why. This file is neither — it is the list of things known to be wrong,
missing, or deferred. {If neither exists, name what does describe the
project — a README, the source itself — and say there is no spec and no
strategy doc, so the instantiator is not left to invent a sentence.}

## Log

Dated snapshots of what happened to this file as a whole: initial state,
verification passes, bulk triage. **Append-only.** An entry describes the file
as it was on that date and is never edited to stay current; a later entry
supersedes it. A reader who finds "no `agent` values changed" in a March entry
and a changed `agent` value in an April item should not be surprised, and a
parser should not treat any entry here as a claim about the present.

**{date} — Initial state:** {how the file was assembled, what has and has not
been tree-checked, known conflicts flagged inline by id.}

---

## Field conventions

Each item carries the same fields, in the same order. Machine-parseable by
design: this file is intended to feed an agent-driven triage or backlog
framework later, and the `agent` field is the triage decision that framework
will consume. Fields marked optional are omitted, not left blank, when they
do not apply. Field values and tags are written unquoted on field lines; a
consumer keys on the field line (`- **{field}:** ...`), never on a value
appearing anywhere in the file, so a value named in prose is not a hit.

- **`id`** — stable, never reused. Zero-padded numeric, assigned in order of
  entry. An item filed retroactively (fix landed before the entry existed)
  still gets the next id; say so in `entered`. An instance may key its
  Non-code section with a letter-prefixed sequence of its own (`N01`, `N02`,
  ...); the prefix is defined here once, the sequence is separate from the
  numeric one, and its ids are never reused either.
- **`slug`** — not a field but part of the heading: `### {id} ({slug}) —
  {title}`. Kebab-case, two to four words, naming the item's subject so it
  stays true if the title is later corrected. Set once; never changed or
  reused. Required on every new item; an older item without one gets one
  the next time it is touched. Cite an item as `{id} ({slug})` — a bare id
  is a pointer only a machine can follow.
- **`area`** — subsystem. {Each instance defines its own list here: five to
  eight values, each optionally followed by a one-phrase description of what
  it covers, e.g. `data` (schema, migrations, queries). Keep it to these; a
  value used by one item belongs under a broader one.}
- **`priority`** — `blocker` (cannot ship), `near-term` (ship-soon window),
  `deferred` (no date), `latent` (not currently reachable; guard against
  future reachability). May carry a milestone tag, e.g. `near-term,
  {milestone-tag}`, for items that sit on a release path but are not
  themselves code; the tag name is instance-defined and stated here. May
  carry an inline reason after an em dash (`near-term — gates {event},
  {date}`), as `agent` does, so a move between sections is explained on the
  line. On close the whole original value, annotation included, is kept
  inside `was {value} — no longer applicable`.
- **`gates`** — optional. The release track this item must be resolved
  before. {Track names are instance-defined and stated here.} Absent means
  it gates nothing. Orthogonal to `priority`: `blocker` means cannot ship at
  all, `gates` means cannot ship *to that track*. Resolving a gate means
  closing the item or re-scoping it so the gating part is done; most gates
  are human checks that no test can verify. Recorded by hand; intended to
  feed a release preflight check. Distinct from the milestone tag on
  `priority`: `gates` names the stage; the tag only groups items that
  straddle code and non-code, and an instance using `gates` rarely needs
  it. Instances with a single release track drop the field — it would
  duplicate `blocker` — and say so in the header.
- **`agent`** — delegation triage. `yes` = mechanically verifiable, local
  change, small diff. `no` = requires a judgment call, a product decision, or
  touches something outside the repo. `triage` = not yet decided.
  **A `yes` here never authorizes a push{ to `{deploy-branch}`, if the
  instance names one}.** Pushes remain the human review gate.
  When a value changes, say why in the field itself (`yes — scoped {date};
  see Decision below`) so the history is on the line, not in git blame.
- **`verify`** — what proves the item done. An item with no verification story
  cannot be `agent: yes`. If an item has two halves with different verification
  stories, that is the signal to split it — or to say explicitly why it is one
  item. A migration or data change verifies per HR-023
  (migration-verification): against a restore of production data, not an
  empty database.
- **`source`** — where the finding came from (audit, dogfooding, code review,
  a doc section, a conversation), so the reasoning is recoverable.
- **`entered`** — when the item was added. Always present. An instance whose
  founding batch predates the field states the id range and the batch date
  here once, rather than on each item.
- **`verified`** — when this item's claims were last checked **against the
  tree**, as distinct from against another document. Cite the file and line
  or the commit that confirms the claim. `n/a` for items with nothing
  in-repo to check: console settings, filings, design notes — and say what
  the item is about instead. `not checked` is the honest value for an item
  that has never been tree-checked; it is a valid state, not a placeholder to
  hide. `pending` is not a value; use `not checked`.
- **`blocks`** — optional. What this item stands in front of: a feature, a
  doc section, another item. Present only when something real is waiting.
- **`resolution`** — present only on items under Closed: `shipped` or
  `declined`, with the date, the commits, a one-line outcome, and where
  each `verify` clause was seen met: in the tests, on a restore of
  production data, or on production itself. "Shipped" alone lets a clause
  nobody saw run pass as met. Live items omit it. A closed item keeps its
  original fields in place, with `priority` rewritten as `was {value} — no
  longer applicable` so nothing consuming this file mistakes it for live
  work. Where the fix taught something that outlives the bug, a
  **transferable mechanism** paragraph follows the addenda (see Item
  template), stating the general hazard in a form the next reader can
  recognize in unrelated code.

`verified` is the one that earns its place. An item sourced from another doc
and never tree-checked is how an already-fixed bug arrives as a live entry and
how an unbuilt screen gets described as shipped. A stale item looks exactly as
fresh as a live one without it, and an agent consuming this file takes every
entry at face value. **Backlog items sourced from another doc are checked
against the tree before entry, not inherited on the doc's authority.** An
instance whose initial population was assembled unchecked says so in its
first Log entry; that is the one deliberate exception.

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
  the item's original claim. The claim stays; the finding corrects it. A
  correction to the title, scope, or a number in the original body is a
  Finding; there is no fourth kind.
- **`Decision ({date})`** — a scope or design choice, with the alternative
  considered and why it lost. **For a project without a separate decisions
  log, this block is the decisions log**: a strategic decline lives here on
  the closed item, and the header's decisions-log reference says so. {Each
  instance states here which applies: a named decisions file, or "Decision
  blocks are this project's decisions log."}

The original body is written once, for a reader with none of the conversation.
Addenda are written the same way. An item with three addenda is not messy; it
is an item that was understood in stages, and the stages are the record. An
addendum that would change `agent`, `priority`, `gates`, `verify`, or
`verified` says so at its end and the field is updated to match. A Finding is
a tree check by definition, so it always updates `verified` to its own date.

### Item template

```markdown
### {id} ({slug}) — {One-line title stating the problem, not the fix}

- **area:** {area}
- **priority:** {priority}[, {milestone-tag}][ — {reason}]
- **gates:** {optional: track}[ — {what must be true}]
- **agent:** {yes | no | triage} — {reason, if no or triage}
- **verify:** {what proves it done}
- **source:** {origin}, {date}
- **entered:** {date}
- **verified:** {date} — {file:line, commit, or n/a — what}
- **blocks:** {optional}

{Body. What is wrong or missing, where in the tree, what the consequence
is, and anything the fixer needs to know that is not obvious from the diff —
including things NOT to do by reflex. Cross-reference related items by id
and slug. Write it for a reader who has none of the conversation that
produced it.}

**Diagnosis ({date})**

{Optional. See Dated addenda.}

**Finding ({date})**

{Optional. See Dated addenda.}

**Decision ({date})**

{Optional. See Dated addenda.}

**Transferable mechanism**

{Optional, Closed items only. The general hazard, stated so it can be
recognized in unrelated code.}
```

---

## Near-term

{Items in the ship-soon window. Includes `blocker` items — there is no
separate Blockers section; the priority field carries it. If a blocker exists,
it is the first item here.}

---

## Deferred

{Real, wanted, no date. Items here should still have a verify story, or an
honest `n/a until scoped`.}

---

## Latent

Not currently reachable. Each is a guard against a future change making it
reachable — the entry exists so the guard is remembered, not so the fix is
scheduled. State what would make it reachable, and cross-reference the item
that would make it so.

---

## Non-code

{Out-of-repo obligations that still need tracking: filings, console
declarations, registrations, env vars, policy text. `verified: n/a`
throughout. Rename the section to fit — "Post-launch, non-code",
"Compliance", "Ops" — but keep it separate from the engineering queue so the
two are never mistaken for each other. If it fills with items that share
nothing but being out-of-repo (a logo, an env var, a policy gap), that is
fine at small scale; at larger scale split it by what the items actually are
("Ops", "Design", "Policy") rather than letting one section hold three
unrelated queues.}

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

---

## Adapting this template

Delete this section once instantiated.

- **This template over-fits on purpose.** It carries every field and section
  two projects have needed so that a third project finds the slot already
  named rather than inventing a fourth name for the same thing. Not every
  project needs every part. Drop what is irrelevant at instantiation — the
  milestone tag, the Non-code section, the separate decisions log — and say
  in the header what was dropped, so a reader comparing against the template
  knows it was a choice. What is *not* optional: the field order, the
  `verified` discipline, ids never reused, Closed always last, and the
  dated-addenda headings. Those are the parts a parser or a future agent
  will key on.
- **Keep the field order.** Anything parsing this file will key on it.
- **Define `area` once and resist growing it.** Six to eight values is
  plenty; a value used by one item is a sign the item belongs under a broader
  one.
- **The milestone tag is optional.** Cuando used `launch-path` for items that
  gate a release without being code. Drop it if the project has no such
  boundary; pick a project-specific name if it does. A project already live
  usually has none. Dropping it touches three places: the Scope paragraph,
  the `priority` description under Field conventions, and the `priority`
  line of the item template. An instance that keeps `gates` usually drops
  the tag.
- **`gates` is optional.** Drop it — and say so in the instantiation
  comment — if the project has fewer than two release tracks, since with one
  it duplicates `blocker`. An instance that keeps `gates` usually drops the
  milestone tag.
- **The Non-code id prefix is optional.** Instances that number Non-code
  items in the main sequence keep doing so; renumbering is forbidden.
- **Slug back-fill is incremental.** An older item is named when next
  touched, not in a bulk pass.
- **The decisions log is optional as a separate file.** Cuando has
  STRATEGY.md; MGP records decisions in the item's Decision block. Either is
  fine; the header should say which.
- **Section names are adjustable; section semantics are not.** Live work is
  split by urgency (near-term / deferred) and by reachability (latent), and
  Closed is always last.
- **Two items that share a mechanism are still two items** if they have
  different weights, different verify stories, or different decision-makers.
  Cross-reference by id rather than merging.
- **Retroactive entries are fine.** When a fix lands before an item exists,
  file it into Closed directly with a note in `entered` — the entry carries
  the resolution and the mechanism, which is the point.
- **Assembling from memory is fine; pretending it is verified is not.** A
  file built from chat history and old docs is a legitimate starting point
  if every item says `verified: not checked` and the first Log entry says
  so. The first agent session on the file should be the verification pass,
  and its handoff should reference items by the ids *in the file*, not by
  ids in a draft.
- **A house rule that emerged from an item lives in the house rules**, not
  here. Record the item, cite the rule id, move on.
