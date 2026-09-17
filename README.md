# house-rules

Working agreements for solo development with AI coding agents.

These came out of about nine months of AI-assisted development — December 2025
onward — across a lot of projects, the biggest of my own being a Flutter app I've been building since March. Working alone, across three machines, in scattered
sessions. The number of projects is why the rules exist: enough of them in a
short enough span that I needed the process to hold still even when nothing
else did. They are what I start a project with now.

They are not a theory of AI-assisted development, and they are dated: some
exist because of a specific tool's behaviour in 2026, and will look silly
later.

## The files

| File                 | What it is                                                                                                                                                 |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `RULES.md`           | **The source of truth.** Terse, numbered, stable IDs. Edit here.                                                                                           |
| `CLAUDE_FRAGMENT.md` | Agent-facing subset, extracted from `RULES.md`. Never edited directly — change `RULES.md` and re-extract.                                                  |
| `HOUSE_RULES.md`     | Annotated essay version, with the scar behind each rule. Commentary on a specific version of `RULES.md`, not a parallel source. Carries a snapshot header. |

If the three ever disagree, `RULES.md` wins.

## Rule IDs

Rules are `HR-001` through `HR-021`. **IDs are permanent** — never reused, never
renumbered. A retired rule keeps its number and gets marked `Retired`, so a
cross-reference from an old vendored copy never silently points at a different
rule.

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
project six months later.

Release notes record what moved: rules added, rules retired, and rules that
migrated to a different `Enforced` category. That last one is the interesting
column.

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
