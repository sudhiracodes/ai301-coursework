# Rubric: is this plan ready to post and build from?

<!--
This rubric defines the checks and verdict rules executed by the plan-check skill.
Each check evaluates a specific dimension of a candidate plan package against
evidence mapped in references/evidence-guide.md.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `grounded-diagnosis` | Candidate plan's stated diagnosis/cause read against the package's repro evidence, stack traces, and control runs | The stated root cause directly accounts for the defect demonstrated in the reproduction evidence and does not contradict any control runs, intermediate logs, or timing traces (e.g., does not blame argument parsers when controls parse successfully, missing modules when other bundle exports work, post-read type casts when upstream data already lost values, or keybindings when timing runs without pagers reproduce the delay). *(Rationale: An incorrect or contradicted diagnosis leads to wasted implementation cycles and ineffective code changes.)* | required |
| `bounded-scope` | Candidate plan's scope declarations, in-scope vs. out-of-scope boundaries, and list of files touched | The plan isolates the minimal necessary fix for the diagnosed issue without bundling unrequested feature creep, broad architectural rewrites, dependency migrations, UI overhauls, or widespread refactors. Explicit scope deferrals with technical reasoning are permitted. *(Rationale: Unbounded scope introduces regressions, bloats diffs, and delays maintainer review.)* | required |
| `executable-approach` | Candidate plan's implementation steps, target files, functions, and architecture | The plan identifies concrete files, functions, or components to modify and details actionable, step-by-step logic that a stranger or teammate could begin building without having to guess architecture or defer primary design decisions to build time. *(Rationale: Vague or exploratory outlines cannot be reliably reviewed, scheduled, or executed by other contributors.)* | required |
| `decisive-test-plan` | Candidate plan's test plan section read against the repro evidence and verification commands | The test plan defines specific execution commands and observable post-fix success criteria (e.g., exit code 0, expected terminal string, targeted unit test pass replacing an XFAIL, or benchmark measurement) that conclusively prove the bug is resolved. *(Rationale: Without observable pass/fail criteria, regressions cannot be verified and maintainers cannot confirm resolution.)* | required |
| `stated-uncertainty` | Candidate plan's risks, unknowns, edge cases, and post-build deviations | The plan explicitly identifies plausible technical risks, edge cases, platform dependencies, or checked unknowns, or honestly notes that risks are minimal due to narrow isolation, avoiding false certainty. *(Rationale: Transparent risk assessment prevents unhandled edge cases from reaching production.)* | required |
| `thread-convention-adherent` | Candidate plan and draft plan comment read against thread highlights (maintainer directions) and repo-facts (contribution policies, AI disclosure rules) | The plan comment directly engages with and follows explicit maintainer guidance on the thread (e.g., testing maintainer-provided binaries or focusing on requested areas) and adheres to all repository rules, including mandatory AI-use disclosure statements where required by project policy. *(Rationale: Disregarding maintainer guidance or repo-wide policies breaks community trust and leads to PR rejection.)* | required |

## Verdict rule

A plan package is **accept** (ready to post and build from) if and only if every `required` check passes (`pass`). If any `required` check fails (`fail`) or is undetermined (`unclear`), the verdict is **reject** (hold). `preferred` checks provide constructive feedback but never alter the binary verdict.
