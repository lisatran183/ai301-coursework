# Evidence guide: where proof lives in a reproduction package

## Environment

Where it lives: in an eval bundle, the repo-facts block and the repro report's environment section. In live mode, the repo's own docs (setup/README) and the student's draft comment.

What good looks like: the environment record names the OS, runtime, and dependency versions actually used, and those match (or explicitly note a deliberate difference from) what the issue's target environment calls for.

## Steps

Where it lives: in an eval bundle, the repro report's steps section. In live mode, the issue thread's reproduction steps (if given) and the student's draft report.

What good looks like: a stranger could follow the steps exactly as written, starting from a stated starting state, and land on the same outcome with no missing setup or ambiguous action in between.

## Behavior shown

Where it lives: in an eval bundle, the output excerpts, logs, or screenshots attached to the repro report. In live mode, whatever artifact the student pastes into their draft.

What good looks like: the artifact shows the actual behavior the issue describes happening, not an adjacent symptom or a restatement of the issue text in different words.

## Honesty

Where it lives: in an eval bundle, the repro report's stated conclusion next to its own evidence. In live mode, the student's draft conclusion next to what they actually captured.

What good looks like: the claimed outcome matches what the evidence shows exactly, no more and no less. An honest, well-evidenced "cannot reproduce" counts as good here; a confident claim of reproduction unsupported by the artifact does not.

## Comms

Where it lives: in an eval bundle, the claim comment and repro comment next to the repo's stated contribution policy (including any AI-use disclosure requirement) under repo-facts. In live mode, the student's draft comment next to the issue and the repo's docs.

What good looks like: the comment names the issue's specifics rather than generic boilerplate, promises investigation only (never a fix or a date), and discloses AI assistance when the repo's policy requires it.
