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
**Pull Request:** https://github.com/iamvisheshsrivastava/geospatial/pull/54

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

**Unit 7: Test implementation, query parameter calibration, and pull request delivery**

I implemented four comprehensive unit tests in `tests/test_api.py` covering the happy path, corrupted file uploads, oversized payloads, and temporary file lifecycle management for `POST /change-detect`. During initial test execution, the threshold assertion failed because FastAPI interprets unannotated primitive arguments alongside file uploads as query parameters rather than form data; calibrating the test request to pass parameters via query string resolved the discrepancy and brought all 20 tests in `test_api.py` and all 35 tests in the repo suite to green. I opened PR #54 against upstream with detailed before-and-after test execution evidence.

**Unit 8: PR monitoring, communication playbook, and status tracking**

I created a custom PR communication playbook in `~/.claude/pr-monitor/playbook.md` defining response protocols, a 5-day staleness threshold derived from historical repository merge activity, and walk-away criteria. While PR #54 is currently awaiting initial maintainer review with passing security checks, the playbook establishes deterministic guidance for addressing feedback without defensive friction. No nudge is drafted yet as the PR was opened today and is well within normal maintainer response windows.

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

The root cause of missing test coverage is historical omission: `src/api/main.py:749` exposes `change_detect` accepting `before: UploadFile`, `after: UploadFile`, and `threshold: float = _DEFAULT_CHANGE_THRESHOLD`, but `tests/test_api.py` only covered the older `/predict`, `/anomaly`, and async batch endpoints. The endpoint relies on `src.change_detection.detect_change` which executes deep-feature extraction with a ResNet-50 backbone. Because real inference requires PyTorch weights and image preprocessing, testing this endpoint requires mocking `detect_change` while testing the real FastAPI request processing, parameter parsing, error handlers, and file unlink cleanup in the `finally` block.

**Proposed Solution**

Add targeted unit tests directly in `tests/test_api.py` without modifying application production code:
1. Use `monkeypatch.setattr("src.change_detection.detect_change", ...)` to provide a deterministic mock dictionary without downloading or executing real ResNet weights.
2. Use the existing `_png_bytes()` helper to generate valid image file payloads for multipart upload simulation via `TestClient`.
3. Test negative cases (corrupted non-image bytes returning 400, oversized uploads returning 413).
4. Wrap `main._save_upload` with a spy function to verify that all temporary disk paths created during execution are cleanly unlinked even when inference fails.
5. Intentionally NOT modify `src/api/main.py` or any ML inference pipelines, preserving existing endpoint behavior and contracts.

**Implementation Plan**

1. Create feature branch `fix/39-change-detect-tests` off `main`.
2. Implement `test_change_detect_happy_path` verifying HTTP 200 and schema consistency with custom and default thresholds.
3. Implement `test_change_detect_rejects_corrupted_upload` for invalid byte payloads on both before and after files.
4. Implement `test_change_detect_rejects_oversized_upload` verifying HTTP 413 when files exceed configured maximums.
5. Implement `test_change_detect_cleans_up_temp_files` tracking file descriptors to prove zero leaked temporary files.
6. Run `PYTHONPATH=. .venv/bin/pytest tests/test_api.py -v` and `PYTHONPATH=. .venv/bin/pytest -q`.

Deviations: During step 2, initial assertions assumed `threshold` was transmitted via form-data body; debugging the endpoint revealed FastAPI treats primitive scalar arguments as query parameters when accompanied by `UploadFile`s. Updated the test to use `params={"threshold": "0.5"}`.

**Toolkit**

- Tool: plan-check and code analysis
- What it produced: Mapped endpoint failure modes and designed a 4-test suite asserting status codes, payload shapes, and temporary file disposal.
- Difficulties: none met
- What changed: Calibrated the plan to explicitly test both explicit query parameter thresholds and default threshold fallback.

---

## Testing Strategy

The testing strategy validates the endpoint through four observable and reproducible checks in `tests/test_api.py` run via `pytest`:
1. **Happy path & schema validation**: Submitting two valid PNG images with a query threshold produces HTTP 200 and a response containing `change_score`, `is_changed`, `threshold`, and `change_map`. Submitting without a threshold parameter correctly applies the default threshold of `0.15`.
2. **Corrupted payload isolation**: Uploading non-image string bytes (`b"not an image"`) for either the `before` or `after` image triggers the endpoint's exception handler and returns a clean HTTP 400 error (`"Change detection failed..."`), never an unhandled 500.
3. **Payload size gating**: Uploading a file exceeding `settings.max_upload_mb` triggers `_read_upload_capped` and raises HTTP 413.
4. **Temporary file cleanup**: Spying on `main._save_upload` and injecting a simulated runtime failure in `detect_change` proves that all generated temporary files on disk are unlinked in the `finally` block.

**Execution Before Fix:**
```text
$ grep -i "change[-_]detect" tests/test_api.py
(no matches)
$ PYTHONPATH=. .venv/bin/pytest tests/test_api.py -v
======================== 16 passed, 1 warning in 34.83s ========================
```

**Execution After Fix:**
```text
$ PYTHONPATH=. .venv/bin/pytest tests/test_api.py -v -k "change_detect"
tests/test_api.py::test_change_detect_happy_path PASSED                  [ 25%]
tests/test_api.py::test_change_detect_rejects_corrupted_upload PASSED    [ 50%]
tests/test_api.py::test_change_detect_rejects_oversized_upload PASSED    [ 75%]
tests/test_api.py::test_change_detect_cleans_up_temp_files PASSED        [100%]
================= 4 passed, 16 deselected, 1 warning in 2.12s ==================

$ PYTHONPATH=. .venv/bin/pytest tests/test_api.py -v
======================== 20 passed, 1 warning in 2.30s =========================

$ PYTHONPATH=. .venv/bin/pytest -q
35 passed, 4 warnings in 2.69s
```

**Toolkit**

- Tool: pytest test runner with TestClient
- What it produced: 4 new passing tests, bringing `test_api.py` coverage from 16 to 20 tests and the full suite to 35 passing tests.
- Difficulties: none met
- What changed: nothing

---

## Implementation Notes

**2026-11-10**

- Set up isolated Python 3.11 virtual environment using `uv` and verified initial baseline test pass rate (16/16 in `test_api.py`).
- Posted claim comment on GitHub issue #39 with proposed test approach.
- Created `fix/39-change-detect-tests` branch in local clone.
- Authored initial draft of `test_change_detect_happy_path`. The first test run failed with `AssertionError: assert 0.15 == 0.5`. Diagnosed that `threshold: float = _DEFAULT_CHANGE_THRESHOLD` on line 753 is parsed by FastAPI as a query parameter rather than form-data field when multipart files are present.
- Updated the test invocation to pass `params={"threshold": "0.5"}` and added a secondary assertion verifying the default `0.15` threshold when omitted.
- Implemented `test_change_detect_rejects_corrupted_upload`, testing both `before` and `after` corruption paths to assert HTTP 400.
- Implemented `test_change_detect_rejects_oversized_upload` to assert HTTP 413.
- Implemented `test_change_detect_cleans_up_temp_files` with a spy on `_save_upload` and forced exception to confirm 100% temporary file cleanup in the `finally` block.
- Pushed branch `fix/39-change-detect-tests` to `sudhiracodes/geospatial` and opened Pull Request #54 against `iamvisheshsrivastava/geospatial:main`.

**Toolkit**

- Tool: git, gh CLI, and Python interactive test execution
- What it produced: Branch, 112 lines of test additions across 4 test functions, clean commits, and PR #54.
- Difficulties: Catching the query parameter versus form-data parameter binding in FastAPI.
- What changed: Adjusted test client request invocation to use `params` for scalar primitive values.

---

## Pull Request

**PR Link:** https://github.com/iamvisheshsrivastava/geospatial/pull/54

**PR Description:**

Closes #39. Adds unit test coverage for `POST /change-detect` in `tests/test_api.py`:
- `test_change_detect_happy_path`: verifies 200 response, payload schema (`change_score`, `is_changed`, `threshold`, `change_map`), and threshold parameter handling.
- `test_change_detect_rejects_corrupted_upload`: asserts clean HTTP 400 when invalid non-image bytes are supplied.
- `test_change_detect_rejects_oversized_upload`: asserts HTTP 413 for uploads exceeding `max_upload_mb`.
- `test_change_detect_cleans_up_temp_files`: proves all temporary upload files are deleted in the `finally` block even when inference raises an exception.
Includes before/after test run evidence (16 -> 20 tests in `test_api.py`, 35 passed overall) and AI usage disclosure.

**Maintainer Feedback:**

- 2026-11-10: none yet (PR opened today; awaiting maintainer review)

**Status:** Awaiting review

**Toolkit**

- Tool: gh pr create
- What it produced: Pull Request #54 on iamvisheshsrivastava/geospatial
- Difficulties: none met
- What changed: nothing

---

## Learnings and Reflections

**Technical Skills Gained**

- Writing isolated API unit tests using FastAPI's `TestClient` for multipart file uploads and form handling.
- Employing pytest's `monkeypatch` to mock ML inference functions (`detect_change`) and verify endpoint integration without needing model checkpoint files or GPU hardware.
- Understanding FastAPI's parameter resolution semantics: primitive arguments defined alongside `UploadFile` are bound as query parameters unless explicitly declared with `Form(...)`.
- Testing resource leak prevention and file descriptor lifecycle by simulating exceptions and verifying cleanup in `finally` blocks.

**Challenges Overcome**

- Diagnosing the query parameter binding discrepancy: The initial happy-path test failed with `AssertionError: assert 0.15 == 0.5` when passing `threshold` as form data. Inspecting the function signature revealed it was parsed as a query parameter, which I resolved by supplying `params={"threshold": "0.5"}` and adding a secondary assertion for default threshold fallback.
- Standing up a clean local Python 3.11 environment with 93 dependencies (including PyTorch, torchvision, and rasterio) using `uv` to ensure test isolation.

**What I'd Do Differently Next Time**

- Check the endpoint's OpenAPI schema (`/docs` or `app.openapi()`) prior to drafting test requests to immediately verify whether parameters belong in query strings, form fields, or headers.
- Include a live claim-check probe directly in the initial scout scanning pipeline to avoid manually evaluating issues that were claimed upstream during the gap between scan date and contribution work.

---

## Resources Used

- FastAPI TestClient and Request Files documentation: https://fastapi.tiangolo.com/tutorial/request-files/
- GitHub Issue #39 thread: https://github.com/iamvisheshsrivastava/geospatial/issues/39
- Pytest Monkeypatch Fixture documentation: https://docs.pytest.org/en/stable/how-to/monkeypatch.html
