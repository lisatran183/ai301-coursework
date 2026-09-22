# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/15

**Verdict output**

```
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/15",
  "checks": [
    {"name": "Maintainer active", "grade": "pass", "evidence": "Last commit to main 2026-09-16, 6 days before today (2026-09-22)"},
    {"name": "Repo in use", "grade": "pass", "evidence": "19 commits to main since 2026-06-24, clearing the 5+ threshold"},
    {"name": "Scope is bounded", "grade": "pass", "evidence": "One named defect (Orchestrator never empties its ContextManager); the three listed files are related steps in that single fix, which the check explicitly allows"},
    {"name": "Not already claimed", "grade": "pass", "evidence": "assignees: [] , no comments, and the repo contains zero PRs"},
    {"name": "AI-contribution policy allows this", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no AI/LLM/generated-code clause; silence passes"},
    {"name": "Clear reproduction steps", "grade": "pass", "evidence": "Expected vs actual with a trigger: 'one orchestrator handling two reviews returns the first run's result for every tool whose input hasn't changed'"},
    {"name": "Maintainer responsiveness", "grade": "fail", "evidence": "Issue has 0 comments since it was opened 2026-09-10"}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

**Run history**

9/20 -> 15/20 -> 20/20 (final, matches eval-run.txt)

**Issue analysis**

issue-20 — my rubric's verdict: reject; gold label: reject (agree). Early on, my "Scope is bounded" check wrongly accepted issue-20 with "graded accept" and no fail reason. The issue body looked bounded on  its face (it even stated a "Success looks like" criterion), but the gold note called it "a one-line feature wish with no spec and a product decision hiding inside." The gap was that the issue named a placeholder ("Logo asset TBD") and an undefined "who" (which company's logo, for a public open-source tool used by many companies). My check originally only tested for a stated success criterion, which this issue had, so it passed. I rewrote the check to explicitly fail when a feature request leaves a core detail unspecified or marked TBD even if it states a success criterion, which fixed it without breaking the three genuinely-bounded issues (01, 04, 19) that had also been struggling against an earlier, stricter version of the same check.

**Check rationale**

"Scope is bounded" | Issue body and comment thread | Issue is built around one named problem or one named improvement, even if fixing it touches several files, rule types, or requires several related implementation steps listed in the body. Fail if the issue explicitly bundles multiple separate, unrelated tasks (a tracking list or checklist covering unrelated work); is a feature request with an unresolved core detail left unspecified (an unnamed asset, an undefined "who" or "what", a placeholder marked TBD) even if it states a success criterion; or the comment thread shows unresolved design 
debate or abandoned PR attempts. | required

**Trade-offs**

This check accepts issues with multiple listed implementation steps as long as they serve one named problem, rather than requiring a single atomic step. That means a canary re-run with --only on an issue with a long, detailed body (like issue-01, which lists five sub-changes across several doc pages for one GA rollout) could occasionally still accept something a stricter reader would call too large for a first issue, since the line between "detailed steps for one fix" and "several different fixes bundled together" is a judgment call the rubric makes via the maintainer's own framing rather than an independent size check.

---

## Selection rationale

**Selection rationale**

1. Fit: I have limited direct coding depth in agent architectures, but this issue is narrowly scoped to state management in one file (session_store.py), which fits building coding fluency without needing deep familiarity with the review agent's full pipeline.

2. What the verdict identified correctly, and what I weighed that the rubric could not: the verdict correctly caught that the issue is open, unclaimed, and has real expected-vs-actual detail, which is the required signal. It failed "Maintainer responsiveness" (no comments yet), which is a preferred check and doesn't block acceptance — I weighed that this is a repo with zero PRs and zero comments across all three candidates, so responsiveness data essentially doesn't exist yet for any issue here, making that particular signal weak evidence either way.

3. Anticipated difficulty in claiming: since the repo has zero existing PRs and no prior contributor activity to reference, I don't have an established claim-comment pattern to follow, so I'll need to write a clear, self-contained claim comment in Unit 2 that states my planned approach explicitly, since there's no precedent thread to align with.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
