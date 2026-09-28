# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

sudhiracodes

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-2374829101

```markdown
I am looking into this issue as part of CodePath AI301. I plan to set up the local development environment, reproduce the `UnknownHashError` exception in `verify_password` with malformed stored hashes, and verify the covering test in `tests/unit/test_security.py` before drafting a pull request.
```

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-2374984210

```markdown
## Reproduction Report

### Environment
- **OS**: macOS 14.5 (Darwin 23.5.0 arm64)
- **Python**: 3.11.9
- **Target repo**: `codepath/pathreview-ai301-fa26-s1` (commit `d9b0e19`)
- **Dependencies**: `passlib==1.7.4`, `pytest==8.3.2`, `fastapi==0.112.0`

### Steps to Reproduce
1. Check out the repository and install dependencies in a clean virtual environment (`pip install -r requirements.txt`).
2. Run the unit test covering malformed hash verification:
   ```bash
   pytest tests/unit/test_security.py -k test_verify_password_malformed_hash -v
   ```
3. Alternatively, test the function interactively in Python:
   ```python
   from core.security import verify_password
   verify_password("testpassword", "invalid_malformed_hash_string")
   ```

### Observed Behavior
- The unit test currently fails with an XFAIL expectation:
  ```
  tests/unit/test_security.py::test_verify_password_malformed_hash XFAIL [100%]
  Reason: H-05: UnknownHashError raised on malformed stored hash
  ```
- Executing `verify_password` raises an unhandled exception rather than returning `False`:
  ```
  passlib.exc.UnknownHashError: hash could not be identified
  ```

### Expected Behavior
- `verify_password("testpassword", "invalid_malformed_hash_string")` should fail closed and return `False` without raising an unhandled `UnknownHashError`.
```

---

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

`Run 1: agreement: 20/20 scored items  (bar: 18/20: PASS)`

**Package analysis**

Package `pkg-20` (`ghostty-org/ghostty#13604`):
- Rubric decision: `reject`
- Gold label: `reject`
- Reasoning: `pkg-20` presents an otherwise complete reproduction report: the environment is fully specified, the steps are followable, and the terminal trace confirms the exact behavior. However, the repository facts explicitly state that Ghostty's contribution guidelines require disclosing all AI assistance in issue comments and pull requests. Because the candidate comments omitted this mandatory disclosure, check `ai-policy-compliant` failed. Under the boolean verdict rule, this required check failure gates the verdict, producing a final verdict of `reject`.

**Check rationale**

Quoted check from `rubric.md`:
`| `behavior-faithful` | Output artifacts compared against the issue context description | The artifact demonstrates the exact error message, failure mode, or exit code described in the issue (or an honest cannot-reproduce attempt). Mismatched syntax errors, unbound variable compile failures, or unacknowledged legacy version behaviors fail. *(Rationale: Misattributing unrelated errors creates false positives and wastes triage time.)* | required |`

Reasoning: In bug reproduction, contributors often encounter non-zero exit codes caused by CLI argument mistakes or syntax errors rather than the reported defect (such as `pkg-02` running prefix range syntax instead of end-offset syntax, or `pkg-08` introducing an unbound variable error). A naive check that passes any failing command output generates false accepts on misdiagnosed bugs. The check requires the artifact to match the specific error mode, traceback, or exit code of the target issue, ensuring reports represent faithful reproductions.

**Trade-offs**

Requiring exact behavioral alignment between the artifact and the issue description means that if a bug produces a platform-specific variation (such as a different OS error string or wrapper exception), the check may grade it as `fail` unless the contributor explicitly documents the platform difference. We accept this trade-off because false positives from misattributed user error waste maintainer time, whereas genuine platform discrepancies can be readily clarified by explaining the environment delta in the report.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
