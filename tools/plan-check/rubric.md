# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-matches-evidence | The Candidate plan's Cause line, read against the Repro evidence (steps, Expected, Actual) and the Issue | Pass if the cause explains what the repro showed, including any detail that narrows it down (when the bug happens and when it does not), and nothing in the Repro evidence contradicts it. Fail if the cause ignores or contradicts a repro observation, or blames something the repro never touched. | required |
| targets-the-cause | The Candidate plan's Change line, read against its Cause line and the Repro evidence | Pass if the change acts on the mechanism the cause names. Fail if it treats the symptom (hides the error, adds a downstream special case, changes the message, adds a workaround) while the cause stays in place. | required |
| scope-is-bounded | The Candidate plan's Change line, including its In and Out parts | Pass if In is limited to what fixing this issue needs and Out names at least one thing deliberately left alone. Fail if In bundles unrelated refactors, features, or cleanup, leaves the boundary open-ended, or nothing is ruled out. | required |
| stranger-can-start | The Candidate plan's Change line | Pass if it names a concrete place to edit (a file, function, or component) and what to do there, so someone new to the repo could start. Fail if the change is vague ("fix the logic", "improve handling") or gives no location. Do not fail only because Repo facts do not list files, since Repo facts describe the repo, not its file tree. | required |
| test-plan-proves-fix | The Candidate plan's Test line, read against the Repro evidence's steps and Actual result | Pass if it re-runs the repro (or a check through the real code) and states a specific observable result after the fix that differs from the Actual result. Fail if it only says "run the tests" or "verify it works", or the expected result would also be true with the bug still there. | required |
| unknowns-are-honest | The Candidate plan and Candidate plan comment, read against the Repro evidence and the Issue | Pass if every claim stated as fact is backed by the Repro evidence or Issue, and anything unverified is worded as an assumption or open question. Fail if the plan or comment states something unverified as certain (a cause never confirmed, testing that did not happen, "this fixes every case"). A missing risks section alone is not a fail. | required |
| fits-thread-and-conventions | The Candidate plan comment, read against the Thread highlights and the Repo facts (especially the contribution policy) | Pass if the comment is a standalone plan, respects what the thread settled (maintainer direction, ruled-out approaches, work others claimed), and follows the policy in Repo facts. Fail if it repeats a rejected approach, ignores a maintainer instruction, takes work someone else claimed, breaks the contribution policy, or only points at another plan ("same as above"). If the thread has no comments, judge against Repo facts alone. | required |

## Verdict rule

Accept (ready) only if every required check passes. Reject (hold) if any required check fails. A ? (unclear) on a required check counts as a fail, because a plan that leaves the grader unsure is not ready to build from. Preferred checks are reported but never change the verdict.
