# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

---

## Your identity upstream

**GitHub username**

lisatran183

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/15#issuecomment-5864199627

Hi, I'd like to claim this issue.

I've read through the report: with one orchestrator handling two reviews, tools whose input hasn't changed keep returning the first run's result instead of a fresh one, which points at the ContextManager not being cleared between runs.

I'm going to start by tracing how session_store.py and the related state-handling files manage context across multiple review runs, to confirm where the stale result is actually coming from. I'll post a reproduction report here with what I find.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/15#issuecomment-5880077408

## Reproduction report

**Environment:** macOS, Python 3.12.5, Docker Compose stack (Postgres 16, Redis 7, ChromaDB 0.4.22), backend running locally via `make run` at `localhost:8000`, `LLM_PROVIDER=mock`.

**Steps:**
1. Created a profile via `POST /profiles` with `github_username=octocat`, `portfolio_url=https://github.com/octocat`. Returned `profile_id: 872d4ac7-603e-407d-b4bb-5292877154e`.
2. Created a review via `POST /reviews` with that `profile_id`. Returned `review_id: 9016c9fa-aa21-4c89-aa18-7b30eaf4505c`. Polled `GET /reviews/{review_id}` until `complete`.
3. Immediately created a second review via `POST /reviews` with the identical `profile_id`, no other input changed. Returned `review_id: 941ff0b9-1d01-43aa-b430-d335456699ed`. Polled until `complete`.
4. Compared the two reviews' `sections` content: word-for-word identical between both runs.
5. Traced the code path behind `POST /reviews` to explain that result. `_run_rag_retrieval_generation` in `core/services/review_service.py` (around line 328) returns a hardcoded placeholder dict, and the pipeline invoked by this endpoint never instantiates `Orchestrator` or `ContextManager` at all.

**Conclusion: I could not reproduce the issue through this path.** The identical output across two runs is fully explained by the placeholder in `_run_rag_retrieval_generation`, not by a stale `ContextManager`, since no `Orchestrator` or `ContextManager` is constructed anywhere in the `POST /reviews` flow I exercised. This means the HTTP API's review-creation endpoint currently can't exercise the bug described in the issue at all, since the orchestrator code path it's meant to hit isn't wired up yet at this endpoint.

To actually reproduce the described behavior, `Orchestrator.run` would need to be driven twice against one shared `ContextManager` directly (e.g. via a script that imports and calls those classes), rather than through `POST /reviews`. I haven't done that yet, that's the next step.

Used an AI-based grading skill to check this report against my rubric before posting; all reproduction steps were run and this report itself was written by me.

## Eval iterations

**Run history**

19/20 scored items (bar: 18/20, PASS) -> revised `claim-specificity` check to `required` after pkg-19 disagreed -> 19/20 scored items, confirmed (bar: 18/20, PASS)

**Package analysis**

pkg-19 — my rubric's first-run verdict: accept; gold label: reject (disagreed). The candidate claim comment promised "I will fix it within 2 days guaranteed" and asked to "keep this issue reserved for me" before any reproduction was posted, which the gold label rejected on. My `claim-specificity` check was originally marked `preferred`, so even though it should have caught the fix-promise language, a preferred check can never flip the verdict per my verdict rule. I revised the check to `required` and tightened its pass condition to explicitly fail on a promised fix, timeframe, or reserved status, which fixed pkg-19 without breaking my canary packages (pkg-08, pkg-14).

**Check rationale**

"claim-specificity" | The claim comment | Names the issue's specifics and promises investigation only. Fails if it promises a fix, a timeframe, or a to-be-reserved status before any reproduction work is shown | required

I revised this from `preferred` to `required` and tightened the pass condition after pkg-19 disagreed with the gold label. The original wording ("promises investigation only, never a fix or a date") could catch the pattern in principle, but being weighted `preferred` meant it could never change the accept/reject verdict per my own verdict rule, so the disagreement wasn't a wording problem, it was a weighting problem.

**Trade-offs**

Promoting `claim-specificity` to `required` means any claim comment with even a minor promise-like phrase could now flip a package from accept to reject, when a looser rubric would have let it through on other checks passing. I re-ran two already-agreeing canaries from the `unfollowable-comms` category (pkg-08, pkg-14) with `--only` after the change, and both still agreed with gold, so the stricter check didn't introduce a new false rejection in that category.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.