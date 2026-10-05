# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

sudhiracodes

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-2375102941

```markdown
## Plan: Handle UnknownHashError in verify_password

### Diagnosis
`verify_password` in `core/security.py` delegates password verification directly to `pwd_context.verify(plain_password, hashed_password)`. When passed a malformed or unrecognized hash string, `passlib.context.CryptContext._identify_record` fails to identify any matching hash algorithm scheme and raises an uncaught `passlib.exc.UnknownHashError` (or `ValueError`). In an authentication flow, this results in unhandled 500 errors instead of securely failing closed and returning `False`.

### Scope
- **In scope**: Catch `UnknownHashError` and `ValueError` inside `verify_password` in `core/security.py` and return `False`.
- **Out of scope**: Migrating password hashing schemes, modifying token encoding/decoding, or altering API authentication routes in `api/`.

### Approach & Files Touched
- **Target File**: `core/security.py`
- Import `UnknownHashError` from `passlib.exc`.
- Wrap `pwd_context.verify(plain_password, hashed_password)` inside `verify_password` with `try...except (UnknownHashError, ValueError): return False`.

### Test Plan
- Re-run unit test: `pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format -v` (expected: status transitions from FAILED with `UnknownHashError` to PASSED with return value `False`).
- Re-run complete security test suite: `pytest tests/unit/test_security.py -v` (expected: all 25 unit tests pass cleanly).

### Risks and Unknowns
- Exception catching is restricted strictly to `(UnknownHashError, ValueError)` to avoid catching unexpected base runtime exceptions.
```

---

## Your branch

**Branch**

fix/72-unknown-hash-error

**Evidence**

Before fix:
```bash
pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format -v
```
Output:
```
============================= test session starts ==============================
platform darwin -- Python 3.13.5, pytest-9.1.1, pluggy-1.6.0
rootdir: /Users/sudhirachegu/Desktop/codepath-ai110/pathreview
configfile: pyproject.toml
plugins: benchmark-5.2.3, cov-7.1.0, asyncio-1.4.0, anyio-4.14.2, pytest_httpserver-1.1.5, hypothesis-6.156.6
asyncio: mode=Mode.STRICT, debug=False, asyncio_default_fixture_loop_scope=None, asyncio_default_test_loop_scope=function
collected 25 items / 24 deselected / 1 selected

tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format FAILED [100%]

=================================== FAILURES ===================================
_______________ TestSecurity.test_verify_with_wrong_hash_format ________________

self = <tests.unit.test_security.TestSecurity object at 0x104fc6fd0>

    def test_verify_with_wrong_hash_format(self):
        """Test verify_password with non-bcrypt hash."""
        wrong_hash = "not_a_valid_bcrypt_hash"
    
        # Should handle gracefully, return False
>       result = verify_password("password", wrong_hash)

tests/unit/test_security.py:230: 
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 
core/security.py:37: in verify_password
    return bool(pwd_context.verify(plain_password, hashed_password))
.venv/lib/python3.13/site-packages/passlib/context.py:2343: in verify
    record = self._get_or_identify_record(hash, scheme, category)
.venv/lib/python3.13/site-packages/passlib/context.py:2031: in _get_or_identify_record
    return self._identify_record(hash, category)
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ 

self = <passlib.context._CryptConfig object at 0x104f8c6e0>
hash = 'not_a_valid_bcrypt_hash', category = None, required = True

    def identify_record(self, hash, category, required=True):
        if not isinstance(hash, unicode_or_bytes_types):
            raise ExpectedStringError(hash, "hash")
        for record in self._get_record_list(category):
            if record.identify(hash):
                return record
        if not required:
            return None
        elif not self.schemes:
            raise KeyError("no crypt algorithms supported")
        else:
>           raise exc.UnknownHashError("hash could not be identified")
E           passlib.exc.UnknownHashError: hash could not be identified

.venv/lib/python3.13/site-packages/passlib/context.py:1132: UnknownHashError
=========================== short test summary info ============================
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
======================== 1 failed, 24 deselected in 0.15s =======================
```

After fix:
```bash
pytest tests/unit/test_security.py -v
```
Output:
```
============================= test session starts ==============================
platform darwin -- Python 3.13.5, pytest-9.1.1, pluggy-1.6.0
rootdir: /Users/sudhirachegu/Desktop/codepath-ai110/pathreview
configfile: pyproject.toml
plugins: benchmark-5.2.3, cov-7.1.0, asyncio-1.4.0, anyio-4.14.2, pytest_httpserver-1.1.5, hypothesis-6.156.6
asyncio: mode=Mode.STRICT, debug=False, asyncio_default_fixture_loop_scope=None, asyncio_default_test_loop_scope=function
collected 25 items

tests/unit/test_security.py::TestSecurity::test_hash_password_returns_bcrypt_hash PASSED [  4%]
tests/unit/test_security.py::TestSecurity::test_verify_password_correct PASSED [  8%]
tests/unit/test_security.py::TestSecurity::test_verify_password_incorrect PASSED [ 12%]
tests/unit/test_security.py::TestSecurity::test_verify_password_case_sensitive PASSED [ 16%]
tests/unit/test_security.py::TestSecurity::test_hash_same_password_different_hash PASSED [ 20%]
tests/unit/test_security.py::TestSecurity::test_create_access_token_returns_string PASSED [ 24%]
tests/unit/test_security.py::TestSecurity::test_create_access_token_is_jwt PASSED [ 28%]
tests/unit/test_security.py::TestSecurity::test_decode_access_token_valid PASSED [ 32%]
tests/unit/test_security.py::TestSecurity::test_decode_access_token_invalid_token PASSED [ 36%]
tests/unit/test_security.py::TestSecurity::test_decode_access_token_malformed PASSED [ 40%]
tests/unit/test_security.py::TestSecurity::test_decode_access_token_empty_string PASSED [ 44%]
tests/unit/test_security.py::TestSecurity::test_roundtrip_token_with_data PASSED [ 48%]
tests/unit/test_security.py::TestSecurity::test_create_access_token_with_custom_expiry PASSED [ 52%]
tests/unit/test_security.py::TestSecurity::test_access_token_includes_expiration PASSED [ 56%]
tests/unit/test_security.py::TestSecurity::test_password_hash_different_for_different_passwords PASSED [ 60%]
tests/unit/test_security.py::TestSecurity::test_verify_password_with_empty_strings PASSED [ 64%]
tests/unit/test_security.py::TestSecurity::test_verify_password_with_special_characters PASSED [ 68%]
tests/unit/test_security.py::TestSecurity::test_token_with_empty_data PASSED [ 72%]
tests/unit/test_security.py::TestSecurity::test_token_with_special_characters_in_data PASSED [ 76%]
tests/unit/test_security.py::TestSecurity::test_token_with_unicode_data PASSED [ 80%]
tests/unit/test_security.py::TestSecurity::test_hash_password_long_input PASSED [ 84%]
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format PASSED [ 88%]
tests/unit/test_security.py::TestSecurity::test_token_tampering_detection PASSED [ 92%]
tests/unit/test_security.py::TestSecurity::test_create_token_consistency PASSED [ 96%]
tests/unit/test_security.py::TestSecurity::test_password_with_whitespace PASSED [100%]

============================== 25 passed in 4.74s ==============================
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

`Run 1: agreement: 20/20 scored items  (bar: 18/20: PASS)`

**Package analysis**

Package `pkg-06` (`kubernetes/minikube#21408`):
- Rubric decision: `reject`
- Gold label: `reject`
- Reasoning: `pkg-06` addresses an empty-tar save failure by proposing a sprawling five-front campaign: image preload regeneration, a major containerd version upgrade, cross-runtime abstraction redesign, and a new CI matrix configuration. The bounded, minimal fix (surfacing the empty tar error gracefully) was completely subordinated within an extensive redesign. Check `bounded-scope` graded this package as `fail`. Because `bounded-scope` is a required check under the binary verdict rule, the overall verdict evaluated to `reject`.

**Check rationale**

Quoted check from `rubric.md`:
`| `grounded-diagnosis` | Candidate plan's stated diagnosis/cause read against the package's repro evidence, stack traces, and control runs | The stated root cause directly accounts for the defect demonstrated in the reproduction evidence and does not contradict any control runs, intermediate logs, or timing traces (e.g., does not blame argument parsers when controls parse successfully, missing modules when other bundle exports work, post-read type casts when upstream data already lost values, or keybindings when timing runs without pagers reproduce the delay). *(Rationale: An incorrect or contradicted diagnosis leads to wasted implementation cycles and ineffective code changes.)* | required |`

Reasoning: In software planning, contributors often propose diagnoses based on intuition or surface symptoms that directly contradict control runs (such as `pkg-01` blaming the argument tokenizer when flagless controls succeed, `pkg-07` blaming missing modules when bundle exports operate normally, `pkg-11` blaming collect operators when top-level controls pass, or `calib-03` blaming keybindings when pagerless timing traces isolate syntax highlighting). The check requires explicitly verifying the diagnosis against all control evidence in the package, rejecting plans whose proposed mechanisms are disproven by control runs.

**Trade-offs**

Requiring strict consistency with all control runs means that if a multi-factor bug has both primary and secondary causes, a plan focusing solely on the secondary trigger will be graded `fail` unless it reconciles the control data. We accept this trade-off because proposing fixes for disproven mechanisms wastes engineering bandwidth and causes build failures, whereas genuine multi-factor issues can be addressed by explicitly articulating how both factors interact with the control runs.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
