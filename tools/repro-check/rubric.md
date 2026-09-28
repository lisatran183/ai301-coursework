# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment-recorded | The repro report's environment section | Names the OS, versions, and dependencies someone would need to set up before they could attempt to reproduce | required |
| repro-steps-clear | The repro report's steps | A stranger could follow the steps exactly as written and land on the same outcome, with no missing setup or ambiguous actions | required |
| behavior-matches-issue | The repro report's observed behavior vs the issue's description | Shows actual observed output or behavior that speaks to what the issue describes, not a restatement of the issue text | required |
| honest-uncertainty | The repro report's conclusion | An honest, evidenced "cannot reproduce" passes; a confident reproduction of behavior that doesn't match the issue fails | required |
| ai-disclosure | The repo's stated contribution policy (Repo facts) vs the claim and repro comments | Fails only if the policy requires disclosing AI assistance and the comments don't disclose it. No stated policy passes automatically | required |
| claim-specificity | The claim comment | Names the issue's specifics and promises investigation only. Fails if it promises a fix, a timeframe, or a to-be-reserved status before any reproduction work is shown | required |

## Verdict rule

Accept only if every required check passes. Any fail or unclear on a required check rejects the package. Preferred checks never change the verdict; they only note strengths.
