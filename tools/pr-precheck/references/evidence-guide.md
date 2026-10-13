# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

**Where it lives**

- Eval: the Plan context block names the plan's Files, what is in scope, what is not in scope, and the Test plan. The Diff section shows what changed, file by file, and the Commits list shows how. The Description section holds the fidelity claims ("implements the plan exactly", "no options added", the tests and files the checklist says were added).
- Live: `plan.md` in the top folder of the working copy, including anything under `## Deviations`. The diff is `git diff main...HEAD` (three dots), and the commits are `git log main..HEAD --oneline`. The fidelity claims are in the Summary and Changes sections of `pr_draft.md`.

**What good looks like**

- Every changed file in the diff is named in the plan's Files list, sits inside a folder the plan names, or is covered by a deviation note. A file the plan never mentions, with no note, is drift (calib-03's `property.js` refactor is the example).
- Every change the plan asks for shows up in the diff, or the description and Deviations say plainly it was left out and why. A planned change that is just missing, with no note, is drift in the other direction.
- Every claim the description makes about what changed, what was added, or what was left alone is true of the diff. A claimed test or file that is not in the diff is drift, not a wording problem (calib-03's "all-comments control" and "nested chains" fixtures are the example).
- An honest deviation re-ties a mismatch: if `plan.md` and the description both say what changed and why, grade against that.

## Test evidence (harness category: not-tested)

**Where it lives**

- Eval: the Test evidence section shows the commands and their output. Read it against the Plan context's Test plan, the Issue's expected result, and any checks the Repo facts block's PR template asks for.
- Live: `test_evidence.md` next to `plan.md`, and the Testing section of `pr_draft.md`, which should paste from it. Read them against the Test plan in `plan.md` and the checks Path Review's PR template lists: `make test-unit`, `make test-integration`, `make lint`, and `make typecheck`. The before and after are the unit 2 repro steps re-run on `main` and on the branch.

**What good looks like**

- The issue's repro is re-run before and after the fix, and the after names something concrete and observable: printed output, an exit code, a specific error now gone, or specific on-screen text for a terminal UI (pkg-08's toast text). This can be pasted output or a precise note of what was observed.
- Or a new test in the diff asserts the issue's expected result, and a suite run that includes it is reported by its named command (pkg-11's regression test inside `cargo test -p ignore`).
- The repo's own test suite is reported as run by its exact command and outcome. Counts and pasted output are not required in eval mode, and format or lint results can be a sentence (pkg-05's "fmt and clippy clean").
- Test plan items like controls can be covered by a specific statement of their result ("no-ampersand control unchanged"). Gaps here are reported but do not hold the PR.
- Live mode on Path Review is stricter: paste the full output of `make test-unit`, `make test-integration`, `make lint`, and `make typecheck`. A failing check or one that cannot run counts if pasted as is with the reason (`make test-integration` printing "no tests ran" is expected).
- Not good: "tests pass" or "the suite passes" with no command named, an after that is only a judgment with nothing observable (calib-03's "matches the issue's expected output byte for byte"), no before/after contrast and no asserting test, or evidence that only exercises a path the change does not touch.

## Diff quality (harness category: unreviewable)

**Where it lives**

- Eval: the Diff section, hunk by hunk, and the Commits list.
- Live: `git diff main...HEAD` for the hunks and `git diff main...HEAD --stat` for the file list, plus `git log main..HEAD --oneline`. Only committed changes count.

**What good looks like**

- Every hunk serves the fix the issue needs. A reviewer can find the fix right away because nothing unrelated sits next to it.
- Debris tells: leftover debug prints or logging, commented-out code, dead code, stray or generated files, formatting-only churn, and drive-by refactors or renames the issue does not need. Any of these fails, even if the description explains it.
- Not debris in live mode: removing the `@pytest.mark.xfail` marker for your own issue and any `pyproject.toml` suppression that names your issue. Path Review's seeded bugs require that removal once the fix works.
- `plan.md`, `pr_draft.md`, `test_evidence.md`, and `comment.md` must never appear in the diff. If one does, it is a stray file.

## Standards and comms (harness category: standards-wall)

**Where it lives**

- Eval: the Repo facts block's pull requests line (the template's sections and checklist asks) and its contribution policy line (including any AI-use rule). The Description section is where the PR answers them. The Thread highlights show any maintainer direction.
- Live: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` in the working copy, the house rules in `scope.md`, and the issue thread on GitHub. The answers live in `pr_draft.md`: Summary, Issue (`Closes #`), Changes, Testing, Screenshots / Demo, and Notes for Reviewers, which carries the course's AI-use disclosure.

**What good looks like**

- Every section and checklist item the template asks for is filled with real content, or marked not applicable with a reason. A section left as the template's placeholder text counts as missing.
- The issue is named, and every ticked box is true of the diff and evidence. In live mode the CI box stays unticked, since CI only runs after the PR is opened.
- Every disclosure the policy requires is present. In live mode that means Notes for Reviewers says how AI was used on the change. If the policy states no disclosure rule, there is nothing to meet.
- Explicit maintainer direction in the thread is followed, not ignored.
- Not good: a missing changelog entry the template asks for, an empty template section, a ticked box that is false, or an AI disclosure left out when the policy asks for one.