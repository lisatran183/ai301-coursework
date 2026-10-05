# Voice guide: how I talk upstream

## Who I am in threads

I'm coming into open source from an analytics and logistics background, not years of software experience, so I read code carefully before I claim anything about it. In this repo I'm here to reproduce one issue, not to fix it or redesign it. Readers should expect precise, evidenced statements from me, never a confident guess dressed up as a finding.

## Rules I write by

### Rule: no fix promises

I only ever promise investigation and a report, never a fix or a timeline, because I don't yet know if the fix is simple.

- Wrong: "I'll have a patch up by tomorrow."
- Right: "I'm looking into this and will report what I find."

### Rule: say what I actually ran

If I only ran the steps on one OS or one version, I say that instead of implying I tested broadly.

- Wrong: "Confirmed, this is broken."
- Right: "Confirmed on macOS 14 / Node 20; haven't tried other environments."

### Rule: cannot-reproduce is a real result

If I can't reproduce the bug after a genuine attempt, I say so plainly instead of stretching an adjacent symptom into a match.

- Wrong: "I saw something kind of similar, so I think this checks out."
- Right: "I followed the steps as written and could not reproduce the described behavior. Here's exactly what I did and what happened instead."

### Rule: disclose AI assistance when the repo asks for it

If a repo's contribution policy asks for AI-use disclosure, I say specifically what the tool did and what I verified myself, not just that I used one.

- Wrong: "Used AI assistance for this report."
- Right: "Used an AI-based grading skill to check this report against my rubric before posting; all reproduction steps were run and the report itself written by me."

## Things I never post

- A claim comment that names a fix or a date before I've reproduced anything
- "Same as above, can confirm" on someone else's claim or repro, my proof goes up in my own words even on a shared issue
- Vague language like "seems broken" or "doesn't work" without the actual observed output attached
- A repro report I haven't run my own skill against first
