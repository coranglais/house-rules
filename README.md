# house-rules

Working agreements for solo development with AI coding agents.

These came out of AI-assisted development since December 2025 — a lot of
projects, most of the work on one Flutter app begun in March 2026. Working
alone, across three machines, in scattered sessions. The number of projects is
why the rules exist: enough of them in a
short enough span that I needed the process to hold still even when nothing
else did. They are what I start a project with now.

This repo names no private project; instances are described, not named.

They are not a theory of AI-assisted development, and they are dated: some
exist because of a specific tool's behaviour in 2026, and will look silly
later.

## The files

| File                            | What it is                                                                                                                                                 |
| ------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `RULES.md`                      | **The source of truth.** Terse, numbered, stable IDs. Edit here.                                                                                           |
| `CLAUDE_FRAGMENT.md`            | Agent-facing subset, extracted from `RULES.md`. Never edited directly — change `RULES.md` and re-extract.                                                  |
| `HOUSE_RULES.md`                | Annotated essay version, with the scar behind each rule. Commentary on a specific version of `RULES.md`, not a parallel source. Carries a snapshot header. |
| `templates/BACKLOG_TEMPLATE.md` | A backlog format for projects that adopt these rules. Copy once, not synced. Versioned on its own, currently v1.3 (tag `backlog-template-v1.3`).           |
| `docs/BACKLOG.md`               | This repo's own backlog, instantiated from the template: what is known to be wrong, missing, or deferred about the rules and their derived files.          |

If the first three ever disagree, `RULES.md` wins.

## Rule IDs

Rules are `HR-001` through `HR-023` as of `v2.1`. **IDs are permanent** — never
reused, never renumbered. A retired rule keeps its number and gets marked
`Retired`, so a cross-reference from an old vendored copy never silently points
at a different rule.

Since `v2.1`, each rule also carries a **slug** after its ID — kebab-case, two
to four words, naming the rule's subject rather than its current wording. Slugs
are names: assigned once, never changed, never reused; a retired rule keeps its
slug. Cite a rule as `id (slug)` the first time it appears in a prompt, commit,
or doc — `HR-007 (verify-against-tree)` can be read without opening `RULES.md`;
a bare ID can't.

Each rule carries an `Enforced` field — `structural`, `agent`, or `human`:

- `structural` — configured once; the mistake is then unavailable
- `agent` — an instruction in the agent's context; followed insofar as it's read
- `human` — depends on attention every time

Category is a field rather than part of the ID because rules are meant to
migrate toward `structural` as enforcement gets built. That migration is the
point of the project. A rule that can't be moved is a rule to be honest about.

## Versions

`RULES.md` is tagged — `v1`, `v2`, and so on — each time it changes. The derived
files name the tag they were generated from rather than a commit SHA, because a
tag still means something to someone reading a pasted copy in an unrelated
project six months later. The backlog template is versioned on its own series
(`backlog-template-v1.3`), since it changes on a different schedule from the
rules.

From `v2` on, the tags are annotated, and the annotation records what moved:
rules added, rules retired, and rules that migrated to a different `Enforced`
category. That last one is the interesting column. `v1` is a lightweight tag
and carries no message.

**Checking staleness.** In a house-rules checkout, diff `RULES.md` from the tag
named in a derived file's header to the current commit:

```
git diff <tag> HEAD -- RULES.md
```

Empty output means the copy is current. Anything else is exactly what the copy
is missing.

## Using these

1. Vendor `RULES.md` into your project as `docs/RULES.md`, with a header
   recording the date and source tag.
2. Inline `CLAUDE_FRAGMENT.md` into your `CLAUDE.md` (or equivalent). Agents
   follow instructions in front of them more reliably than instructions behind a
   link.
3. Configure the `structural` rules before the first real commit. That's the
   part that keeps working while nobody is paying attention.

Read `HOUSE_RULES.md` first if you want to know *why* any of these exist. The
short version: every rule here is generic advice until you attach the scar, and
rules that feel like ceremony get waived exactly when they matter. Adopt the
ones where you can name your own cost.

## License

This repository is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/);
if you reuse it, please attribute it as "adapted from coranglais/house-rules (CC BY 4.0)".
