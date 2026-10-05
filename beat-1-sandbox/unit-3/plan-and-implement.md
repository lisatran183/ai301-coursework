# Unit 3 - Plan and Build

## Posted upstream

**GitHub username**

lisatran183

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/15#issuecomment-5987226390

## Plan

Following up on my reproduction report. That report couldn't hit the bug through `POST /reviews`, because the endpoint never builds an `Orchestrator`. So I drove `Orchestrator.run` directly with a small script, using stand-in tools that return a new `call_number` on every real call:

Ran on macOS, Python 3.12.5, at commit 2f4e82f, with stand-in tools and an in-memory stand-in for the Redis client.

```
run 1 readme_scorer: {'tool': 'readme_scorer', 'call_number': 1}
run 2 readme_scorer: {'tool': 'readme_scorer', 'call_number': 1}
readme_scorer real calls: 1
tools in saved session after run 2: ['market_analyzer', 'readme_scorer', 'skill_extractor']
```

Run 2 got run 1's cached result (`tool_cache_hit` in the log), and `skill_extractor` stayed in the saved session even though run 2 didn't use it.

**Cause:** `Orchestrator` creates one `ContextManager` in `__init__` and `run()` never empties it. `run()` also merges new results into the profile's old saved session instead of starting fresh.

**Change:**
- Add `ContextManager.clear()` in `agent/memory/context_manager.py`.
- At the top of `run()` in `agent/orchestrator.py`, clear the context and call `session_store.delete(profile_id)`, then start from empty state.
- Add `tests/unit/test_orchestrator.py` that mirrors the repro.

**Not changing:** memoization inside a single run, `SessionStore` itself, the tools, existing lint findings, or wiring `Orchestrator` into `POST /reviews` (separate gap).

**Test:** re-run the script. I expect run 2 to show `call_number: 2`, two real calls, no cache hit, and `skill_extractor` gone from the saved session. The new test should fail before the change and pass after, and `make check && make test-unit` should pass.

**Unknowns:** nothing else in the repo uses `Orchestrator` or `SessionStore` today, but I don't know if carry-over was meant for a future caller, and I haven't ruled out one orchestrator running two reviews at once. I also haven't tested against real Redis.

I'll work on `fix/15-clear-orchestrator-state` and follow the commit format in CONTRIBUTING.

I used an AI-based grading skill to check this plan against my rubric before posting. The reproduction was run by me, and this plan was written by me.

---

## Your branch

**Branch**

fix/15-clear-orchestrator-state

**Evidence**

My Unit 2 repro went through `POST /reviews`, which never builds an `Orchestrator`, so it couldn't reach the bug. As step 11 allows, I turned its inputs (same profile, same input, two runs) into a script and a unit test that run through the real `Orchestrator` and `SessionStore` code. Log lines are filtered out below so the results are readable; the full output is in my saved files.

Before (unfixed code, commit 2f4e82f):

```
$ python repro_issue_15.py > before.txt 2>&1
$ grep -v "\[info" before.txt
=== Part A: same profile, same input, two runs ===
run 1 readme_scorer: {'tool': 'readme_scorer', 'call_number': 1}
run 2 readme_scorer: {'tool': 'readme_scorer', 'call_number': 1}
readme_scorer real calls: 1
run 2 cached_results entries: 2

=== Part B: second run drops a tool, check saved session ===
tools in saved session after run 2: ['market_analyzer', 'readme_scorer', 'skill_extractor']

$ python -m pytest tests/unit/test_orchestrator.py -q
FAILED tests/unit/test_orchestrator.py::test_second_run_with_same_input_calls_tool_again
FAILED tests/unit/test_orchestrator.py::test_saved_session_drops_tools_not_used_in_latest_run
2 failed in 1.73s
```

After (fix on fix/15-clear-orchestrator-state, commit 97f813b):

```
$ python repro_issue_15.py > after.txt 2>&1
$ grep -v "\[info" after.txt
=== Part A: same profile, same input, two runs ===
run 1 readme_scorer: {'tool': 'readme_scorer', 'call_number': 1}
run 2 readme_scorer: {'tool': 'readme_scorer', 'call_number': 2}
readme_scorer real calls: 2
run 2 cached_results entries: 2

=== Part B: second run drops a tool, check saved session ===
tools in saved session after run 2: ['market_analyzer', 'readme_scorer']

$ python -m pytest tests/unit/test_orchestrator.py -q
..                                                                       [100%]
2 passed in 0.46s
```

## Eval iterations

**Run history**

- Run 1 (full run): agreement 18/20 scored items, bar 18/20 PASS. Categories: clear-accept 5/7, scope-creep 4/4, thread-convention 2/2, unbuildable 3/3, wrong-cause 4/4. Misses: pkg-03 (failed fits-thread-and-conventions) and pkg-14 (failed unknowns-are-honest), both gold accept.

Run 1 was my only full run, and it is the run recorded in eval-run.txt. I did not re-grade with --only, because the run already met the bar with every category matched.

**Package analysis**

pkg-14 (zellij-org/zellij#5174, category clear-accept). My rubric decided reject, and the gold label is accept. The only failing check was unknowns-are-honest.

Why my rubric read it that way: that check passes only if "every claim stated as fact is backed by the Repro evidence or Issue". The plan's diagnosis states its mechanism flatly: "the reattach path wires the client's stdin to the session before the query responses have been consumed". It also says 0.44.1 "predates the reattach-path change in 0.44.2" and that "the leak's origin is visible in `zellij --debug` output". None of those three appear in the Repro evidence, which only shows when the leak happens: on every reattach, never on a fresh create, not on 0.44.1, and clean once after a cache clear. eval-run.txt records only which check failed, not the quote behind it, so this is my reading, but these are the claims a strict reading of my pass condition catches.

Why the gold label is right: a diagnosis is always a claim about a cause, and this one is tightly grounded. It explains every repro observation, including the cache control, and it is honest where it counts. It says exact functions are "to be pinned in the PR after tracing", defers the Windows variant because "I cannot test Windows", and names the risk of eating a keystroke during the handshake. My check cannot tell a well-grounded diagnosis apart from an unverified claim, so it holds a plan for stating its cause with confidence. A better split would be to judge the stated cause only under diagnosis-matches-evidence, and keep unknowns-are-honest for claims about work done or results seen.

**Check rationale**

> | unknowns-are-honest | The Candidate plan and Candidate plan comment, read against the Repro evidence and the Issue | Pass if every claim stated as fact is backed by the Repro evidence or Issue, and anything unverified is worded as an assumption or open question. Fail if the plan or comment states something unverified as certain (a cause never confirmed, testing that did not happen, "this fixes every case"). A missing risks section alone is not a fail. | required |

This check started from the template's failure family "the unknowns are dressed up as certainty". My first draft pointed it at "the plan's risks and unknowns" section. When I read calib-01, I saw that packages have no risks section at all, so that version would have failed every package. I rewrote the evidence to be every factual claim in the Candidate plan and Candidate plan comment, checked against the Repro evidence and the Issue, and added the last sentence, "A missing risks section alone is not a fail.", so the check judges overclaiming instead of a missing heading. I kept it required rather than preferred, because a plan that states a guess as fact sends the build in the wrong direction.

**Trade-offs**

Keeping unknowns-are-honest required costs me pkg-14. It is a clear-accept package, gold says accept, and my run held it with "failed: unknowns-are-honest". I chose not to loosen the check after run 1. I was already at 18/20 with every category matched, and loosening a required check can flip packages that agree now, especially in thread-convention, which has only 2 packages. So I accept that this check will sometimes hold a sound plan that my grader reads as stating something unverified as fact, in exchange for catching plans that really do overclaim.