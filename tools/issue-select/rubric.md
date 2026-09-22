# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Maintainer active | Repo-facts block: date of last commit to default branch | Last commit to default branch is within 90 days of today | required |
| Repo in use | Repo-facts block: stars, open issue count, or recent merged PRs | At least 1 PR merged in the last 90 days, OR 5+ commits in the last 90 days | required |
| Scope is bounded | Issue body | Issue describes a single, well-defined bug, feature, or doc fix with clear acceptance criteria — not an umbrella/tracking list of multiple sub-tasks, not an unresolved design debate, and not a one-line wish with no spec | required |
| Not already claimed | Issue assignee field, linked/referenced PRs | Issue has no assignee AND no open (unmerged) PR already linked to or referencing it. Claim comments alone do not count. | required |
| AI-contribution policy allows this | CONTRIBUTING.md, CODE_OF_CONDUCT, or repo README contributor section | Repo's contributing documentation does not explicitly prohibit AI-generated or AI-assisted contributions | required |
| Clear reproduction steps | Issue body | Issue includes specific steps to reproduce, an expected vs actual result, or a code snippet/error message | preferred |
| Maintainer responsiveness | Comment thread | A maintainer (repo owner or listed collaborator) has commented on this specific issue within the last 30 days | preferred |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->

Accept if every required check passes. Reject if any required check fails  or is unclear. Preferred checks never change the verdict; they only order accepted issues by count of preferred passes, ties broken by maintainer responsiveness recency.
