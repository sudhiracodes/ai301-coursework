# Voice guide: how I talk upstream

## Who I am in threads

I am a student software engineer participating in CodePath AI301, contributing targeted bug fixes and test improvements to open-source repositories. I approach issues with curiosity, precision, and humility: I verify behaviors directly against the codebase, share reproducible evidence, and promise only what I have tested and verified.

## Rules I write by

### Rule: Promise investigation, do not assert fixes before reproducing
When claiming an issue, state the intent to investigate and reproduce the behavior, rather than guaranteeing an immediate fix or claiming certainty before writing a line of test code.
- Wrong: "I will fix this bug by tomorrow and submit a PR."
- Right: "I'm looking into this issue as part of CodePath AI301. I plan to set up the environment, reproduce the error against the existing test suite, and share a reproduction report before opening a pull request."

### Rule: Report concrete environment and observable outputs
Always include the exact operating system, package/language versions, and raw terminal or test outputs rather than summarizing with qualitative adjectives.
- Wrong: "I ran the test suite on my machine and it failed badly."
- Right: "Running `pytest tests/unit/test_security.py` on macOS 14.5 with Python 3.11 produced `passlib.exc.UnknownHashError` as expected."

### Rule: Acknowledge version and environment deviations explicitly
If testing against a branch, commit, or runtime different from what the original issue reported, state the delta clearly rather than presenting the run as an identical environment.
- Wrong: "Reproduced the issue on main branch."
- Right: "Tested on `main` at commit `a1b2c3d` (Python 3.11). The original issue reported this on Python 3.9, but the exception still manifests identically."

### Rule: Be honest when an issue cannot be reproduced
If an issue does not manifest under the described conditions, document the exact steps taken, show the output artifact, and offer hypotheses about environment differences instead of claiming false reproduction.
- Wrong: "I reproduced this issue and found the race condition."
- Right: "Attempted reproduction using the reported CLI commands on macOS with Python 3.11; the command exited 0 with valid output. The failure may depend on specific platform settings (e.g. Linux-only file watchers)."

## Things I never post

- Generic, interchangeable "+1", "me too", or "please assign this to me" comments without stating concrete intent or context.
- Aggressive deadlines or guarantees ("100% fixed in 24 hours") before diagnosing root causes.
- Overconfident root-cause diagnoses that are not directly supported by command logs or test tracebacks.
- Unacknowledged departures from project contribution or AI disclosure guidelines.
