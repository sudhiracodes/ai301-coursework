# Plan: Handle UnknownHashError in verify_password

## Diagnosis

When `verify_password` in `core/security.py` is invoked with a malformed or unrecognized password hash string (such as non-bcrypt legacy strings or corrupted entries in the database), `pwd_context.verify(plain_password, hashed_password)` delegates to `passlib.context.CryptContext._identify_record`. Because the hash does not match any registered hash scheme in `CryptContext(schemes=["bcrypt"], deprecated="auto")`, passlib raises an unhandled `passlib.exc.UnknownHashError: hash could not be identified` (or `ValueError`).

As verified in the Unit 2 reproduction evidence:
```
pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format -v
```
Observed failure:
```
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
core/security.py:37: in verify_password
    return bool(pwd_context.verify(plain_password, hashed_password))
passlib.exc.UnknownHashError: hash could not be identified
```
This uncaught exception causes authentication endpoints to fail with a 500 Internal Server Error rather than gracefully failing closed and returning `False` (which translates to a standard 401 Unauthorized).

## Scope

### In Scope
- Modify `verify_password` in `core/security.py` to catch `passlib.exc.UnknownHashError` and `ValueError` during hash verification.
- Return `False` when an unrecognized or malformed hash format is encountered, failing closed securely.
- Ensure all unit tests in `tests/unit/test_security.py` execute and pass.

### Out of Scope
- Migrating or adding new cryptographic hashing schemes (e.g. Argon2, PBKDF2).
- Modifying JWT token generation, encoding, decoding, or expiration logic.
- Altering FastAPI route handlers or dependency injection in `api/`.

## Files Touched

- `core/security.py`: Add `UnknownHashError` import from `passlib.exc` and wrap `pwd_context.verify` inside `verify_password` in a `try...except (UnknownHashError, ValueError): return False` block.

## Approach

1. In `core/security.py`, import `UnknownHashError` from `passlib.exc`.
2. Refactor `verify_password(plain_password: str, hashed_password: str) -> bool`:
   ```python
   def verify_password(plain_password: str, hashed_password: str) -> bool:
       """Verify a plain password against a hashed password.

       Args:
           plain_password: Plain text password to verify
           hashed_password: Previously hashed password to compare against

       Returns:
           True if password matches, False otherwise
       """
       try:
           return bool(pwd_context.verify(plain_password, hashed_password))
       except (UnknownHashError, ValueError):
           return False
   ```
3. Run the unit test suite via `pytest tests/unit/test_security.py` to verify that `test_verify_with_wrong_hash_format` passes alongside all 24 other security tests.

## Test Plan

### Reproduction Re-run (Targeted)
```bash
pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format -v
```
- **Expected before fix**: `FAILED` with `passlib.exc.UnknownHashError: hash could not be identified`.
- **Expected after fix**: `PASSED` with exit code 0 and `1 passed`.

### Full Security Suite Run
```bash
pytest tests/unit/test_security.py -v
```
- **Expected before fix**: 1 failed, 24 passed (`test_verify_with_wrong_hash_format` failed).
- **Expected after fix**: 25 passed in `tests/unit/test_security.py`.

### Interactive Python Verification
```python
from core.security import verify_password
assert verify_password("password", "not_a_valid_bcrypt_hash") is False
assert verify_password("", "invalid_hash") is False
```

## Risks and Unknowns

- **Risk**: Catching overly broad exceptions (such as base `Exception`) could conceal unrelated programming errors.
  - *Mitigation*: Strictly catch `(UnknownHashError, ValueError)` which are the documented exception types raised by `passlib` on malformed hash inputs.
- **Risk**: Potential timing side-channels if invalid hashes return faster than bcrypt comparisons.
  - *Assessment*: Standard practice in passlib and Web APIs when a stored hash is invalid is to fail closed immediately.

## Deviations

Nothing changed; the plan held. The implementation in `core/security.py` wrapped `pwd_context.verify` in `try ... except (UnknownHashError, ValueError): return False` exactly as planned, and all 25 unit tests in `tests/unit/test_security.py` passed with exit code 0.
