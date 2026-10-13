# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/197

**Branch**

fix/15-clear-orchestrator-state

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Partial run (`--only pkg-01,pkg-02`): 1/2. pkg-02 (clear-accept) was rejected with `failed: test-plan-covered, repo-checks-run`. My rubric treated `` `pytest tests/ -q` passes (1462 passed, 12 skipped) `` as a sentence claiming a pass instead of evidence. I changed repo-checks-run so an exact command plus the result it printed counts, and let a new test in the diff cover a test-plan item.
2. Partial run (`--only pkg-01,pkg-02,pkg-04`): 3/3. pkg-01 (standards-wall) and pkg-04 (not-tested) were canaries for the loosened checks, and both still rejected.
3. Full run: `agreement: 14/20 scored items  (bar: 18/20: below the bar)`. Every reject agreed, but `clear-accept 1/7`. All six misses failed on fix-is-proven, test-plan-covered, or repo-checks-run.
4. Partial run (the six misses plus canaries `pkg-04,pkg-07,pkg-10,pkg-14`): 10/10, `clear-accept 6/6  not-tested 4/4`.
5. Full run, saved to `eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with `categories: clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`.

**Package analysis**

pkg-11 (BurntSushi/ripgrep#3477, clear-accept). Gold label: accept. My rubric rejected it in run 3 (`failed: fix-is-proven`) and accepted it in run 5.

The after-fix evidence is a note, not printed output: `$ rg hi                      # after: searches normally, no error`. My first fix-is-proven came straight from the Unit 4 activity, where calib-03's note `# after: matches the issue's expected output byte for byte` didn't count as proof. So my rubric read pkg-11's note the same way and failed it.

What I missed is that pkg-11 proves the fix a second way. Its diff adds a test that asserts the expected result, `assert gi.matched("foo ", false).is_ignore());`, and the evidence names the suite run that includes it: `` `cargo test -p ignore` passes (214 tests) ``. Its note also names something concrete you can observe (the parse error is gone), while calib-03's note is only a judgment. The gold label's definition of not-tested, `no observable before/after, 'tests pass' with nothing named, evidence exercising the unchanged path, or repo checks never run`, doesn't fail either of those. I rewrote fix-is-proven to accept a concrete observed result or an asserting test inside a named suite run, and pkg-11 now accepts.

**Check rationale**

| fix-is-proven | The Test evidence's before and after, read against the Issue's expected result and the plan's Test plan, plus any new or changed test in the Diff | Pass if either: (a) the evidence shows the issue's repro run before and after the fix, and the after names a concrete observable result (printed output, an exit code, a specific error now gone, specific on-screen text or behavior) that matches the expected result and differs from the before; or (b) a new or changed test in the diff asserts the issue's expected result, and a suite run that includes it is reported by its named command. Fail if the after is only a judgment with nothing observable ("works", "fixed", "matches the expected output byte for byte"), if there is no before/after contrast and no asserting test, or if the evidence only exercises a path the change does not touch. | required |

It reads this way because my first version was too strict. It said: "Fail if the after result is only described in a sentence or a note after `#`, if no output is shown". That rule came from calib-03 in the activity, but in my first full run it rejected three good PRs on fix-is-proven: pkg-08 (a terminal UI, where the after is written-down on-screen text), pkg-11 (a `#` note plus an asserting test), and pkg-13 (the after is in Lua comments). The line I actually needed isn't "pasted vs. written down." It's "observable vs. judgment." So I kept calib-03's "matches the expected output byte for byte" as the named example of what fails, and added path (b) for PRs that prove the fix with a test in the diff.

**Trade-offs**

Loosening fix-is-proven and repo-checks-run, and moving test-plan-covered from required to preferred, risked letting real not-tested PRs through as accepts. So before the confirming full run, I re-ran all four not-tested packages as canaries with `--only pkg-05,pkg-08,pkg-11,pkg-13,pkg-16,pkg-19,pkg-04,pkg-07,pkg-10,pkg-14`. All four still rejected (`not-tested 4/4`), and the full run kept every category matched.

What I accept it will miss: with test-plan-covered as preferred, a PR that skips a control from its plan's test plan without saying so can still be accepted, as long as the main repro is proven. I chose that because pkg-13 and pkg-19, both gold accepts, cover their controls with only a sentence: pkg-13 says "No-ampersand control unchanged." and pkg-19 says "Controls (double-quoted twin; single-quoted without the trailing blank line) unchanged." Making that check required rejected good PRs. Also, path (a) trusts a written note like "no error" if it names something concrete, so a dishonest note would pass. My rubric can't tell a true note from an invented one without real output.