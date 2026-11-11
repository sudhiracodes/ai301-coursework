# Contribution: test: no coverage for /change-detect endpoint

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** 1
**Student:** Sudhira Ramesh
**GitHub Username:** sudhiracodes
**Issue:** https://github.com/iamvisheshsrivastava/geospatial/issues/39
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Upgrading issue-select to issue-scout with CLI search queries and ranking filters**

I upgraded the unit-1 issue-select skill into issue-scout by implementing a live GitHub CLI search funnel driven by my profile in Python, FastAPI, and pytest test fixtures. The strategy surfaced 297 candidate issues across three queries, which the ranking filters successfully narrowed to 167 survivors by dropping assigned tasks and noisy threads. While the rubric checks held up cleanly across all top five live candidates, the initial ranking surfaced a CI/workflow runner issue and clustered several issues under a single maintainer. In response, I calibrated the ranking strategy to prioritize application-level pytest and backend bug fixes over CI/DevOps infrastructure tasks.

**Unit 6: Repository memory setup and voice guard integration for issue claim**

I created a CLAUDE.local.md memory file in the repository root configuring the Python 3.11 virtual environment and exact pytest commands so tool agents don't have to discover project context repeatedly. During setup, testing pip commands initially failed because the uv-generated virtual environment lacked pip by default; I resolved this by utilizing uv pip to manage dependency synchronization in under a second. I integrated the voice guard conventions to enforce polite, structured communication before posting my formal claim on issue #39 with concrete reproduction findings and an actionable testing plan.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I searched GitHub using issue-scout with queries grounded in my fit profile targeting Python, FastAPI backend endpoints, and pytest testing fixtures. Across three search queries, the scout surfaced 297 candidate issues, which the ranking filters narrowed to 167 survivors by dropping assigned tasks, noisy comment threads, and stale issues. My scout evaluated this issue (`iamvisheshsrivastava/geospatial#39`) with an `accept` verdict, passing all required criteria: active repository status, pushes within 24 hours, unrestrictive AI policy, zero assignees or competing PRs, and a well-bounded scope in `tests/test_api.py`.

While the scout originally scored AirKube #25 and spec2program #32 slightly higher due to specific keyword matches, live re-verification revealed that AirKube #25 had been closed by maintainers and spec2program #32 was claimed by another contributor with PR #33. Re-evaluating the top survivor candidates showed that `geospatial#39` remained 100% unclaimed, actively maintained with pushes that day, and completely aligned with my interest in building robust API unit test suites with FastAPI `TestClient` and `monkeypatch`. I hope to gain practical experience isolating file-upload endpoints and mocking heavy ML inference boundaries so tests execute quickly and reliably in CI without requiring GPU checkpoints.

Claim comment: https://github.com/iamvisheshsrivastava/geospatial/issues/39#issuecomment-6782873755

**Toolkit**

- Tool: issue-scout (search strategy with GitHub CLI funnel, candidate ranking filters, and rubric.md)
- What it produced: Surfaced 297 candidates, filtered down to 167 survivors, and returned an `accept` verdict for geospatial #39 with passing scores across all required checks.
- Difficulties: Search initially ranked candidates without accounting for rapid upstream claim velocity over multi-week intervals, requiring live status re-verification before claiming.
- What changed: Calibrated the candidate evaluation workflow to check real-time PR links and issue thread state right before claiming, ensuring no duplicate effort on taken issues.

---

## Understanding the Issue

**Problem Description**

The FastAPI application implements a land-cover change detection endpoint (`POST /change-detect`) in `src/api/main.py` that processes two satellite image uploads, extracts ResNet-50 deep features, computes feature-space differences, and cleans up temporary files in a `finally` block. While other inference endpoints (`/predict`, `/anomaly`, `/segment`) have comprehensive unit tests in `tests/test_api.py`, `POST /change-detect` currently has zero test coverage across the entire test suite.

**Expected Behavior**

`tests/test_api.py` should contain unit tests exercising `POST /change-detect` using FastAPI's `TestClient`. It should verify both successful change detection flows (returning HTTP 200 with `change_score`, `is_changed`, `threshold`, and `change_map`) and invalid input scenarios (returning HTTP 400 when corrupted or non-image payloads are uploaded), with dependencies monkeypatched so that tests execute rapidly without real model weights.

**Current Behavior**

Searching `tests/test_api.py` for `change-detect` or `change_detect` returns zero matches. The endpoint's file upload parsing, error trapping, response serialization, and temporary file cleanup routines execute in production without any regression test protection.

**Affected Components**

- `src/api/main.py`: The `change_detect` endpoint function (line 749), its request validation (`UploadFile`), response model (`ChangeDetectionResponse`), and temporary file unlink cleanup.
- `tests/test_api.py`: The API test suite where new unit tests need to be added using `TestClient` and `monkeypatch`.

**Toolkit**

- Tool: Codebase grep and AST inspection via terminal commands
- What it produced: Confirmed that `POST /change-detect` is registered in `src/api/main.py:749` but completely absent from all test files in `tests/`.
- Difficulties: none met
- What changed: nothing

---

## Reproduction Process

**Environment Setup**

- Operating System: macOS (Darwin arm64)
- Runtime: Python 3.11.14 (managed via `uv`)
- Package Manager: `uv` (Fast Python package installer and resolver)
- Project Setup:
  1. Forked repository to `sudhiracodes/geospatial` and cloned locally.
  2. Created an isolated Python 3.11 virtual environment: `uv venv .venv --python 3.11`.
  3. Synchronized project dependencies: `uv pip install -r requirements.txt`.
  4. Executed existing test suite: `PYTHONPATH=. .venv/bin/pytest tests/test_api.py -v`.
- Challenges: Virtual environment created with `uv` does not include standalone `pip` by default; using `uv pip install` resolved dependencies and installed all 93 packages in under 500ms.

**Steps to Reproduce**

1. Search the test directory for any tests targeting the change detection endpoint:
   `grep -rn "change-detect" tests/`
2. Run the API test suite and examine the test names executed:
   `PYTHONPATH=. .venv/bin/pytest tests/test_api.py -v`
3. Inspect `src/api/main.py` lines 748-785 and observe the `change_detect` endpoint definition and its lack of associated test invocations.
4. Observed result: 16 tests pass across `/predict`, `/health`, `/anomaly`, `/models`, `/segment`, and `/pointcloud`, but zero tests touch `/change-detect`.

**Reproduction Evidence**

- **Commit showing reproduction:** none (reproduction is evident from examining test coverage on clean checkout of `main` at `eb85220`)
- **Screenshots/logs:**
  ```text
  $ grep -i "change[-_]detect" tests/test_api.py
  (no output returned)

  $ PYTHONPATH=. .venv/bin/pytest tests/test_api.py -v
  ======================== 16 passed, 1 warning in 34.83s ========================
  ```
- **My findings:** The existing tests in `tests/test_api.py` make effective use of `_png_bytes()` dummy images and `monkeypatch` to mock out PyTorch model inference (e.g. `_FakeClassifier`). This pattern can be directly replicated for `POST /change-detect` by monkeypatching `src.change_detection.detect_change` to return a synthetic dictionary, allowing unit tests to verify endpoint logic, status codes, and temp file cleanup without requiring actual model checkpoints or GPU execution.

**Toolkit**

- Tool: pytest test runner and ripgrep/grep search
- What it produced: Executed all 16 pre-existing tests in `tests/test_api.py` and established a clean baseline showing 100% test pass rate and 0 test cases for `/change-detect`.
- Difficulties: none met
- What changed: nothing

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
