# Evidence guide: where proof lives in a reproduction package

The skill uses this guide as its map: for every kind of proof a rubric check names, this file says WHERE to find it in a package (in both eval bundles and live mode) and WHAT GOOD LOOKS LIKE when you do.

---

## Environment

### Where it lives
- **In an eval bundle**: Under the `Reproduction report` section, look for an `Environment:` line or dedicated `## Environment` subsection specifying operating system (e.g. macOS, Ubuntu, Windows), runtime/compiler versions (e.g. Python 3.11, Node 20, Rust 1.78), package versions, or git commit hashes. Compare these against the `Issue context` and the `Repo facts` target versions.
- **In live mode**: In the student's draft repro comment or report. Compare against the live repository's default branch environment, issue description, and CI matrix.

### What good looks like
- The environment record explicitly names the OS platform and exact version or git commit tested.
- If the reproduction environment differs from the version in the issue (e.g. reproducing on `main` vs a release tag), the deviation is explicitly stated and acknowledged rather than hidden.
- Silent omissions of critical environment parameters (e.g. omitting the OS driver on a platform-specific issue, or testing an old unsupported version without acknowledging it) fail.

---

## Steps

### Where it lives
- **In an eval bundle**: Under the `Reproduction report` section, look for the step-by-step instructions or CLI command snippets (e.g. `Steps to reproduce`, input files, command invocation).
- **In live mode**: In the draft repro comment or standalone reproduction script.

### What good looks like
- The steps provide concrete, standalone commands and inputs that a stranger can execute starting from a clean checkout without missing prerequisites.
- All configuration files, CLI arguments, or sample inputs needed to trigger the behavior are fully provided inline.
- Steps that reference private monorepos, unshared local files, or omit mandatory flags required to reach the trigger state fail.

---

## Behavior shown

### Where it lives
- **In an eval bundle**: Under the `Reproduction report` section, look for raw terminal output excerpts, test failure tracebacks, error messages, and exit codes. Compare this directly against the `Issue context` description of the bug.
- **In live mode**: In the draft report's code blocks, terminal logs, or test run outputs compared against the GitHub issue body and comment history.

### What good looks like
- The output artifact demonstrates the exact failure mode, crash traceback, error message, or exit code described in the issue.
- The artifact does not substitute an unrelated error (e.g. an argument validation syntax error or missing variable compilation error) for the reported runtime crash or logic bug.
- When applicable, showing a control run (e.g. contrasting the failing invocation with a working baseline) clearly isolates the trigger.

---

## Honesty

### Where it lives
- **In an eval bundle**: Read the narrative text and summary conclusions in the `Reproduction report` against the actual output artifacts shown in the same report.
- **In live mode**: Compare the draft commentary and claims against the verifiable command logs and test outputs.

### What good looks like
- The narrative conclusions state only what the evidence demonstrates.
- An honest "cannot reproduce" report that faithfully attempts the reported steps, includes the output artifact showing successful/different behavior, and notes environment hypotheses passes.
- Overconfident claims (e.g. "I verified this race condition" or "guaranteed reproducible") backed by zero artifacts, blank outputs, or misattributed errors fail.

---

## Comms

### Where it lives
- **In an eval bundle**: Look at the `Claim comment` text and the `Reproduction report` narrative, and compare them against the `Repo facts` (especially the `contribution policy` and `AI policy` lines).
- **In live mode**: In the student's draft claim comment and repro comment, compared against the repository's `CONTRIBUTING.md`, `AI_POLICY.md`, and issue templates.

### What good looks like
- **Claim specificity**: The claim comment specifically identifies the issue and states concrete next steps (promising investigation and a repro report), rather than using generic "assign me" boilerplate or overpromising guaranteed fix deadlines.
- **Policy adherence**: If the repository explicitly mandates disclosure of AI assistance in issue/PR comments, the comment includes the required disclosure statement. Silence or general responsibility policies without mandatory disclosure checkboxes pass without disclosure.
