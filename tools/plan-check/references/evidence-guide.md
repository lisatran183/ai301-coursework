# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

Used by: diagnosis-matches-evidence, targets-the-cause

- Where it lives: in a package, the Candidate plan's `Cause:` line, read against the Repro evidence block (its numbered Steps, Expected, and Actual) and the Issue section. In live mode, the cause in the draft plan.md, read against the student's posted repro comment on the issue thread.
- What good looks like: the cause names a mechanism that explains every repro observation, including the detail that narrows it down. In calib-01, the cause (the view's model is not refreshed after a push) explains both why the color stays stale and why leaving and re-entering the view fixes it. The change then acts on that mechanism (adding the refresh), not on what the user sees.

## Scope

Used by: scope-is-bounded

- Where it lives: in a package, the In and Out parts of the Candidate plan's `Change:` line. In live mode, the scope section of plan.md (what will change, what will not) and its files to touch list.
- What good looks like: In describes one change that fixing this issue needs, and Out names at least one nearby thing left alone on purpose. In calib-01, In is one callback in one file, and Out rules out changes to how push status is computed and to other views' refresh behavior.

## Executability

Used by: stranger-can-start

- Where it lives: in a package, the Candidate plan's `Change:` line. In live mode, the approach and files to touch sections of plan.md. Repo facts do not list the file tree, so a file not appearing there is not a problem.
- What good looks like: a named file, function, or component plus the action to take there, specific enough to open the file and start. In calib-01, the change names `pkg/gui/controllers/sync_controller.go`, the push completion callback, and what to add to it.

## Test plan

Used by: test-plan-proves-fix

- Where it lives: in a package, the Candidate plan's `Test:` line, read against the Repro evidence's Steps and Actual result. In live mode, the test plan section of plan.md, read against the student's posted repro steps.
- What good looks like: it re-runs the repro and names the exact step where the result should now differ from Actual. In calib-01, the test says that at step 3 the color must flip without leaving the view, which is exactly what the Actual result says does not happen today. A test that would pass with the bug still there proves nothing.

## Honesty

Used by: unknowns-are-honest

- Where it lives: in a package, every factual claim in the Candidate plan and Candidate plan comment, checked against the Repro evidence and Issue. Packages have no risks section, so look for claims instead. In live mode, the risks and unknowns section and the Deviations section of plan.md, plus claims in comment.md.
- What good looks like: anything the repro showed is stated plainly, and anything not yet checked is worded as an assumption or open question. False confidence looks like a cause stated as confirmed with no evidence, testing that never happened, or "this fixes every case".

## Comms

Used by: fits-thread-and-conventions

- Where it lives: in a package, the Candidate plan comment, read against Thread highlights and the Repo facts block (especially the contribution policy and any AI disclosure rule). In live mode, comment.md read against the issue's GitHub thread and the repo's CONTRIBUTING file and templates.
- What good looks like: a standalone plan in the author's own words that responds to what the thread and repo actually say. In calib-01, the thread is empty and the repo says outside PRs get limited review time, so the comment keeps the fix to one change and names that review note. Boilerplate that ignores a maintainer instruction, repeats a rejected approach, or just says "same as above" fails.
