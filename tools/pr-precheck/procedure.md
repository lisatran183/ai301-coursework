# Procedure: how this tool grades a PR package

## Read order

1. Read the repo's standards first. Eval: the Repo facts block's pull request and contribution policy lines. Live: `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, and the house rules in `scope.md`. Write down every section or checklist item the template asks for, every check it says to run, and any disclosure the policy requires (such as AI use). If the policy states no disclosure rule, note that.
2. Read the Issue and its thread. Note the reported behavior and the expected result, word for word if the issue gives one.
3. Read the plan before anything in the PR. Eval: Plan context. Live: `plan.md`. Write down four lists: the Files it will change, what is in scope, what is out of scope, and each item in its Test plan. Then read any `## Deviations` notes and add what they change to those lists.
4. Read the Commits and the Diff. Eval: the Commits and Diff sections. Live: `git log main..HEAD --oneline` and `git diff main...HEAD`. List every changed file and, under each, a one-line note per hunk saying what it does.
5. Read the Test evidence. Eval: the Test evidence section. Live: `test_evidence.md` and the Testing section of `pr_draft.md`. List each command shown and whether real printed output appears under it. Mark any result that is only a sentence or a note after `#`.
6. Read the PR title and description last. Eval: the Title and Description sections. Live: line 1 of `pr_draft.md` and everything under it. List every factual sentence and every ticked checklist box as a separate claim.
7. Skip the header metadata (source, captured, calibration). It is not evidence for any check.

Why this order: the plan comes before the diff so the diff gets judged against the plan, not the other way around. The diff and evidence come before the description so each claim gets checked against what is actually there, instead of the diff being read through the description's story.

## Evidence gathering

1. diff-matches-plan: put the changed-file list from Read order step 4 next to the plan's Files list and Deviations from step 3. Mark each changed file as named, covered by a named folder, covered by a deviation, or not named. Then go the other way: mark each planned change as present in the diff, disclosed as left out, or missing.
2. description-matches-diff: take the claim list from Read order step 6. For each claim about what changed, what was added, or what was left alone, find the diff line or file that makes it true, or mark it unsupported. Then list any change in the diff that no claim mentions.
3. fix-is-proven: from the Test evidence, copy the before and after for the issue's repro, whether shown as printed output, an exit code, or a written note of what was observed. Put the after next to the Issue's expected result and the before. Mark whether the after names something concrete and observable or is only a judgment ("works", "matches"). Separately, list any new or changed test in the diff that asserts the issue's expected result, and whether a suite run that includes it is reported by its named command.
4. test-plan-covered: take the Test plan items from Read order step 3. For each, find the command and printed output in the evidence that covers it, or a sentence in the description or Deviations that says it was left out and why. Mark each item covered, disclosed, or missing.
5. repo-checks-run: find the repo's test suite run in the evidence (or the suite the plan's Test plan names). Record its exact command and its outcome, or mark it not named. Note any other listed checks (format, lint, type checks) and how they are reported. In live mode on Path Review, mark whether each of the four template checks has pasted output.
6. diff-is-clean: go through the hunk notes from Read order step 4. Mark any hunk that is a debug print, commented-out code, a stray or generated file, formatting-only churn, or a refactor or cleanup the issue does not need.
7. template-asks-met: take the template asks from Read order step 1. For each, find the section or item in the description that fills it, a "not applicable" with a reason, or mark it missing. Check that the issue is named. For each ticked box, find the diff line or evidence that makes it true, or mark it false.
8. policy-disclosure-met: take the disclosure rules from Read order step 1. For each, find the sentence in the description that meets it, or mark it missing. If there were no disclosure rules, record that.
9. title-states-change: copy the title. Note in one line what the diff does, and mark whether the title says the same thing.
10. Keep the exact quote or diff line behind every note, so each grade can cite text from the package.

## Check execution

1. Grade the checks in rubric table order, one at a time, using only the evidence gathered for that check.
2. Grade `pass` if the pass condition is met and `fail` if a fail condition is met. Grade `unclear` only when the evidence is present but honestly supports both readings.
3. If something the check needs is missing from the PR itself (no test evidence, no after-fix output, a template section left out), grade `fail`, because the PR does not do that thing. If a package part is empty in a normal way (a thread with no comments, a policy with no disclosure rule), judge against what is there and do not grade `unclear`.
4. Before failing diff-matches-plan, description-matches-diff, or test-plan-covered for something left out or changed, check the Deviations notes and the description for a plain disclosure. If both say what changed and why, grade against the disclosed version. Do not apply this to diff-is-clean: an unrelated hunk fails even when explained.
5. Write one sentence per grade that names the quote or diff line it rests on.
6. Re-read the package only when a needed quote was missed in gathering, and then redo gathering for that one check only.
7. Judge substance, not format. A short PR that does the job passes, and a long polished one that drifts, fails.

## Verdict assembly

1. List all nine grades in rubric order.
2. Apply the rubric's verdict rule: any required check graded `fail` or `unclear` means reject (hold). All required checks graded `pass` means accept (submit).
3. Report the preferred checks, but leave them out of the verdict.
4. For a reject, the deciding check is the first required check in rubric order graded `fail` or `unclear`. Quote the package line it rests on in that check's evidence line, and list the other failing checks in the summary.
5. For an accept, quote the after-fix output from fix-is-proven as the strongest evidence.
6. Do not override the rule with an overall impression. The same grades always give the same verdict.