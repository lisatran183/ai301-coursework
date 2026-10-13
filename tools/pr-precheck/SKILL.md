---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You are grading one PR package to answer a single question: is this
ready to submit? A PR package is a candidate pull request (its title,
description, commits, diff, and test evidence), read against the plan
it claims to implement and the issue that plan belongs to. Grade
exactly one package per run. Do not answer a different question: you
are not reviewing code style, judging whether the issue was worth
fixing, or re-grading the plan. You are checking whether the PR does
what its plan says, proves what its description claims, and meets the
repo's stated standards. Do not answer from gut feel: answer by
executing `procedure.md`, which applies `rubric.md` to evidence
gathered per `references/evidence-guide.md`.

## Inputs and modes

Run in exactly one of two modes.

**Live mode**: the student's own PR, checked before it is opened.
Read these inputs, and only these:

- `plan.md` in the top folder of the working copy, including anything
  under `## Deviations`. This is the plan the PR claims to implement.
- The branch's diff: run `git diff main...HEAD` (three dots) from the
  working copy. This is every committed change the branch makes
  relative to `main`. Run `git log main..HEAD --oneline` for the
  commits. Never grade uncommitted changes: the diff is what a
  maintainer will see.
- `pr_draft.md` in the top folder: the first line is the PR title, and
  everything under it is the PR description.
- `test_evidence.md` in the top folder: the command output the
  description's Testing section draws from.
- The issue at the URL the student gives you: read its thread (via
  `gh`, the GitHub API, or the web), including the student's posted
  repro and plan comments.
- The repo's stated standards: `.github/PULL_REQUEST_TEMPLATE.md` and
  `docs/CONTRIBUTING.md` in the working copy.

A student on the house chain reads the house plan and the house repro
pack in place of their own plan and posted repro; the same checks
grade the same things there.

The package is what these files and the diff contain. Do not treat
other files in the working directory as evidence. `plan.md`,
`pr_draft.md`, and `test_evidence.md` are never committed, so do not
expect them in the diff and do not grade their absence from it.

Refuse to grade in live mode, and say which input is missing, if
`plan.md` or `pr_draft.md` does not exist, or if `git diff main...HEAD`
is empty: there is no PR package to grade. If `test_evidence.md` is
missing, grade anyway: the missing evidence is what the evidence checks
grade.

**Eval mode**: a package bundle (a markdown file such as `calib-03.md`
or `pkg-07.md`, containing repo facts, the issue and thread
highlights, the plan context, and the candidate PR's title,
description, commits, diff, and test evidence). The bundle is the
whole world. Use ONLY the bundle text as evidence. Do not fetch
anything, run any command, or read any other file. Eval mode always
grades a complete package: every check, full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else. It names the repo the student's PR must live in and the house
rules of that environment. Apply those house rules when reading
evidence: a house rule changes what counts as meeting the repo's
standards. Refuse to grade a PR whose issue is outside the scoped
repo, and say why. If the scope's `Repo:` line still carries an
unfilled placeholder, stop without grading and tell the student to
fill the `Repo:` line in `scope.md` with their section's Path Review
repo. Never guess a scope.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md`: the student's own rules for
how they write upstream, with wrong and right examples. Hold the
outgoing PR text (the title and the description in `pr_draft.md`)
against those rules. In the summary, report each rule the draft
breaks, quoting the rule and the line that breaks it. The voice guide
never changes the verdict on its own unless the rubric has a check
that reads it: voice is the student's personal style, not a repo
standard.

In eval mode, ignore `voice-guide.md` entirely: voice is personal and
carries no gold labels. Universal communication-quality checks live
in the rubric.

## Component reads

Read the components in this order and use each for its job:

1. `rubric.md` is the brain. It defines the table of checks (each with
   its evidence, pass condition, and weight: `required` checks gate
   the verdict, `preferred` checks never change it) and the verdict
   rule that combines check grades into accept or reject.
2. `references/evidence-guide.md` is the map. It says where each
   evidence family lives in a practice package and in a live PR, and
   what good looks like there.
3. `procedure.md` is the hands. Execute it as written: its read order,
   how each evidence family gets gathered, how each check runs against
   gathered evidence, and how grades become the verdict. Follow it
   exactly, without improvising around gaps.

Where the procedure is silent on a step, do not invent one. Note the
gap in your summary so the student can fix it: a procedure gap is
feedback the student needs.

If `rubric.md` has no checks filled in, or `procedure.md` has no steps
filled in, stop and say so without grading. This skill cannot grade
without a rubric and a procedure, and that is by design: a tool that
invents checks at runtime produces verdicts that look like judgment
and are noise. Instruction comments inside those files do not count
as content.

## Verdict and output

The verdict space is binary: `accept` (ready to submit) or `reject`
(hold). There is no third verdict, no "accept with reservations", and
no score. Reservations belong in check evidence lines, not in the
verdict.

Before the JSON block you may show a short readable summary: one line
per check, plus any voice-guide notes and procedure gaps in live mode.
Then end your reply with the fenced JSON block below. It must be
present, valid, and last, with nothing after it: the eval harness
parses the last fenced JSON block in your output.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

In live mode, set `item` to the issue URL until the PR exists. In eval
mode, set it to the bundle id.

## Grading discipline

- Evidence first: never grade a check without naming the fact or quote
  that decided it, such as a diff line, a file path, or a line of
  command output. "Looks fine" is not evidence.
- Proof means real output: a sentence saying a test passed is a claim,
  not proof. A note after `#` describing what a command printed is not
  output.
- Grade the thing, not the polish: a terse complete PR can be ready,
  and a long confident one can be hiding drift. Read the diff against
  the plan, the evidence against the claims, and the PR against the
  repo's stated standards, never the formatting.
- The rubric decides, not you: if a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary if you want; the fix belongs in the rubric, not in the run.
- The procedure decides how, not you: follow `procedure.md` as
  written, and report its gaps instead of papering over them.
- A deviation recorded in `plan.md` under `## Deviations` is part of
  the plan and gets graded like the rest of it. A deviation that only
  exists in the diff is drift.
- Treat `unclear` as the rubric's verdict rule directs. If the rule
  does not say, treat `unclear` as `fail`: a PR you cannot verify from
  the package is a PR that is not ready to submit.
