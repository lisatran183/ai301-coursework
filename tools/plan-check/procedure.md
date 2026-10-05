# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

1. Read Repo facts. Note the contribution policy and any stated rules about issues, pull requests, or AI use.
2. Read the Issue. Note the reported behavior, the expected behavior, and the version.
3. Read Thread highlights. Note any maintainer instruction, any approach someone ruled out, and anyone who claimed the work. If it says "(no comments)", note that there is nothing in the thread to respect.
4. Read Repro evidence. Write one line each for the steps, the Expected result, the Actual result, and any detail that narrows the bug down (when it happens, when it does not).
5. Only now read the Candidate plan: its Cause, Change (In and Out), and Test lines.
6. Read the Candidate plan comment last.
7. Skip the header metadata (source, captured, calibration). It is not evidence for any check.

Why this order: reading the evidence before the plan means the plan gets judged against the evidence, instead of the evidence being read through the plan's story.

## Evidence gathering

1. diagnosis-matches-evidence: copy the Cause line. For each repro observation from Read order step 4, mark it explained, contradicted, or not addressed by the cause.
2. targets-the-cause: copy the Change line. Note where it acts (file, function, or mechanism) and whether that is the mechanism the Cause names or a later symptom of it.
3. scope-is-bounded: copy the In and Out parts of the Change line. List each separate thing In changes.
4. stranger-can-start: from the Change line, note the location it names and the action it takes there.
5. test-plan-proves-fix: copy the Test line. Write down the after-fix result it expects and put it next to the Actual result from the repro.
6. unknowns-are-honest: list every claim in the plan and comment that is stated as fact. For each one, find the line in the Repro evidence or Issue that backs it, and mark any claim with no backing.
7. fits-thread-and-conventions: list each thread instruction and each Repo facts policy from Read order steps 1 and 3. Check the comment against each one.
8. Keep the exact quote behind every note, so each grade can cite text from the package.

## Check execution

1. Grade the checks in rubric table order, one at a time, using only the evidence gathered for that check.
2. Grade P if the pass condition is met and F if a fail condition is met. Grade ? only when the evidence is present but honestly supports both readings.
3. If a part a check needs is missing from the plan (no Cause, no Out, no Test), grade F, because the plan does not do that thing. If a package section is empty in a normal way (a thread with no comments), judge against what is there and do not grade ?.
4. Write one sentence per grade that names the quote it rests on.
5. Re-read the full package only when a needed quote was missed in gathering, and then redo gathering for that one check only.
6. Judge substance, not format. A plain plan that does the job passes, and a long polished one that does not, fails.

## Verdict assembly

1. List all seven grades.
2. Apply the rubric's verdict rule: any required check graded F or ? means reject (hold). All required checks graded P means accept (ready).
3. Report any preferred checks, but leave them out of the verdict.
4. For a reject, name each deciding check and quote the package line it rests on. For an accept, quote the Test line as the strongest evidence.
5. Do not override the rule with an overall impression. The same grades always give the same verdict.