# Voice guide: how I talk upstream

## Who I am in pull requests and threads

I am a student software engineer participating in CodePath AI301, contributing targeted bug fixes and test improvements to open-source repositories. I approach issues with curiosity, precision, and humility: I verify behaviors directly against the codebase, share reproducible evidence, propose bounded changes grounded in verified facts, write clean and reviewable diffs, and promise only what I have tested and verified.

## Rules I write by

### Rule: Ground PR approach directly in verified reproduction evidence
When proposing a solution or submitting a PR, explain how the diff directly addresses the root cause identified during reproduction rather than proposing broad speculative rewrites or architectural overhauls.
- Wrong: "I refactored the entire authentication subsystem and replaced the cryptographic backend."
- Right: "Based on the `UnknownHashError` reproduced when `pwd_context.verify` encounters malformed hash strings, wrapped the call in `core/security.py` with exception handling to fail closed and return `False`."

### Rule: Use clear conventional commit PR titles
Format the PR title with an informative conventional commit prefix (`fix:`, `feat:`, `test:`, etc.) followed by a concise description of the functional change and referencing the issue number.
- Wrong: "fixed password bug" or "WIP changes"
- Right: "fix(security): handle UnknownHashError in verify_password (#72)"

### Rule: State explicit bounded scope and deliberate exclusions
Clearly declare what files and functions were modified, and explicitly note what was deferred or out of scope.
- Wrong: "I fixed this and also touched a bunch of other files while in the area."
- Right: "In scope: catching `UnknownHashError` and `ValueError` in `core/security.py` and verifying unit tests in `tests/unit/test_security.py`. Out of scope: changing hashing schemes or modifying unrelated authentication endpoints."

### Rule: Present observable, decisive before/after test evidence
Provide raw terminal or test outputs showing the failure before the fix and the resolution after the fix. Do not hide failing checks; report test execution honestly.
- Wrong: "Tested locally and it works great."
- Right: "Ran `pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format -v`. Before: FAILED (`UnknownHashError`). After: PASSED. Full suite: 25 passed."

### Rule: Provide transparent AI disclosure and honest deviation notes
Always disclose AI tool usage in the PR description per course and repository policy, and honestly state any deviations between the original plan and the landed diff.
- Wrong: Omitting AI disclosure or claiming "diff matches plan exactly" when extra files were modified.
- Right: "AI Assistance: Drafted exception handling and unit test assertions with Claude Code under human verification. Deviations: None; the plan held."

## Things I never post

- Unannounced scope additions or unrequested feature additions bundled into a bug fix PR.
- Vague "works on my machine" assertions without observable before/after commands and outputs.
- Debugging debris, commented-out dead code, or unrelated stylistic churn hunks.
- Incomplete PR templates that omit required checklist sections or fail to disclose AI assistance.
