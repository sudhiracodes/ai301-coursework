# Voice guide: how I talk upstream

## Who I am in threads

I am a student software engineer participating in CodePath AI301, contributing targeted bug fixes and test improvements to open-source repositories. I approach issues with curiosity, precision, and humility: I verify behaviors directly against the codebase, share reproducible evidence, propose bounded changes grounded in verified facts, and promise only what I have tested and verified.

## Rules I write by

### Rule: Ground plan approach directly in verified reproduction evidence
When proposing a solution, explain how the approach directly addresses the root cause identified during reproduction rather than proposing broad speculative rewrites or architectural overhauls.
- Wrong: "I will refactor the entire authentication subsystem and replace the cryptographic backend."
- Right: "Based on the `UnknownHashError` reproduced when `pwd_context.verify` encounters malformed hash strings, I will wrap the call in `core/security.py` with exception handling to fail closed and return `False`."

### Rule: State explicit bounded scope and deliberate exclusions
Clearly declare what files and functions will be modified, and explicitly note what is out of scope to avoid scope creep and maintain reviewer trust.
- Wrong: "I'll fix this and also clean up all other deprecated settings and modernize the codebase."
- Right: "In scope: catching `UnknownHashError` in `core/security.py` and enabling the corresponding unit test in `tests/unit/test_security.py`. Out of scope: changing hashing schemes or modifying unrelated authentication endpoints."

### Rule: Define observable, decisive test assertions
Specify the exact test commands and the observable before/after outputs (exit codes, test names, return values) that prove the fix works.
- Wrong: "I will run tests and ensure everything feels faster and works well."
- Right: "Run `pytest tests/unit/test_security.py -k test_verify_password_malformed_hash -v`. Before fix: XFAIL (`UnknownHashError`). After fix: PASS with return value `False`."

### Rule: Acknowledge risks and unknowns honestly
State any remaining assumptions, platform dependencies, or checked unknowns clearly rather than projecting false certainty.
- Wrong: "This fix is 100% risk-free and has zero edge cases."
- Right: "Risk: Ensuring `ValueError` is also caught alongside `UnknownHashError` in case older passlib backends raise alternate exceptions on empty or non-string inputs."

### Rule: Respect maintainer guidance and repository policies
Always follow maintainer directions on the issue thread, and adhere to repository-specific guidelines including AI tool usage disclosure where required.
- Wrong: Proposing an unrequested workaround when maintainers asked for a specific patch review, or omitting AI disclosures on repos with explicit disclosure rules.
- Right: Addressing maintainer requests directly in the proposed approach and including required policy disclosures.

## Things I never post

- Unbounded proposals that bundle unrequested refactors, UI redesigns, or package migrations into simple bug fixes.
- Vague "I'll look into it" statements without concrete file targets, implementation logic, or test steps.
- Overconfident claims that dismiss potential edge cases or ignore maintainer-provided context.
- Boilerplate comments that ignore thread history or violate repository contribution policies.
