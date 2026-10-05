# Evidence guide: where evidence lives in a plan package

This guide maps each evidence family required by the `plan-check` rubric to its exact location in eval packages and live GitHub repositories, defining observable standards for what constitutes passing evidence.

---

## Diagnosis and grounding

### Where it lives
- **Eval mode**: The `Repro evidence` section (including terminal logs, stack traces, step-by-step reproduction runs, timing matrices, and control runs without flags or with isolated components) compared against the candidate plan's `Diagnosis` or `Root Cause` section.
- **Live mode**: The student's posted week-2 reproduction comment on the issue thread (or house repro pack), containing environment details, reproduction commands, terminal outputs, and stack traces, compared against `plan.md`'s `## Diagnosis` section.

### What good looks like
- The plan identifies the specific defect mechanism that directly accounts for the error message, traceback, or unexpected behavior documented in the reproduction evidence.
- The diagnosis does not contradict any control runs, timing matrices, or intermediate logs present in the reproduction evidence (e.g., it does not blame argument tokenizers when control runs prove argument parsing succeeds, does not blame missing bundled modules when control runs demonstrate other modules in the same bundle execute normally, does not blame post-processing type casts when input data tables already lack the data before casting, and does not blame terminal keybindings when timing runs without pagers reproduce the full delay).

---

## Scope

### Where it lives
- **Eval mode**: The candidate plan's `Scope`, `In Scope`, and `Out of Scope` / `Not in Scope` sections, read alongside the list of files touched and proposed architectural changes.
- **Live mode**: The `## Scope` section of `plan.md` and the draft comment in `comment.md`, detailing what will and will not be modified.

### What good looks like
- The plan defines a strictly bounded change focused on fixing the diagnosed issue without bundling unrequested feature additions, wide-ranging refactors, dependency migrations (e.g., migrating HTTP clients or state machines), UI redesigns, or unrelated CI/build harness modifications.
- If the issue uncovers wider architectural questions, the plan explicitly marks broader refactors or untestable platform variants as out-of-scope/deferred with clear technical reasoning, while delivering a self-contained fix for the target defect.

---

## Executability

### Where it lives
- **Eval mode**: The candidate plan's `Approach`, `Implementation Steps`, and `Files Touched` sections.
- **Live mode**: The `## Approach` and `## Files Touched` sections of `plan.md`, read against the codebase repository structure.

### What good looks like
- The plan names concrete files, functions, methods, or modules to be added or modified.
- The implementation steps describe concrete logic (e.g., specific error handling, condition checks, clamping calculations, or constant wiring) in enough detail that a stranger or teammate can immediately begin implementation without having to invent the architecture, guess target locations, or defer fundamental design decisions to build time.
- Plans that merely suggest open-ended exploration ("poke around the editor code", "profile and optimize without named files", "investigate input stack layers") fail executability.

---

## Test plan

### Where it lives
- **Eval mode**: The candidate plan's `Test Plan` section, read in comparison with the commands and artifacts in the package's `Repro evidence` section.
- **Live mode**: The `## Test Plan` section of `plan.md`, specifying verification commands, reproduction replays, and expected outputs.

### What good looks like
- The test plan specifies concrete commands (e.g., targeted pytest invocations, CLI reproduction commands with specific flags, or scripted regression test cases).
- The test plan specifies an observable post-fix outcome (e.g., exit code 0 instead of failure, specific output text, newly passing unit test assertion replacing an XFAIL, or measurable latency reduction) that directly proves the defect is resolved.
- Generic test instructions that lack observable outcome assertions (such as "run the whole test suite and see if it feels fast" or "ensure nothing else breaks") without an isolated target check fail this requirement.

---

## Honesty

### Where it lives
- **Eval mode**: The candidate plan's `Risks`, `Unknowns`, `Edge Cases`, and `Trade-offs` sections.
- **Live mode**: The `## Risks and Unknowns` section of `plan.md` and the post-build `## Deviations` section.

### What good looks like
- The plan explicitly identifies plausible edge cases, checked unknowns (e.g., verifying tool-specific option flags, secondary error types, or platform-specific behaviors), or honestly states that risks are minimal due to narrow isolation.
- The plan does not project false certainty on unverified assumptions. Post-build deviations honestly record what changed and why ("Nothing changed; the plan held" is valid when true).

---

## Comms

### Where it lives
- **Eval mode**: The candidate plan comment read against the `Issue context` (especially maintainer comments and thread highlights) and the `Repo facts` block (stating contribution guidelines, required templates, and AI disclosure rules).
- **Live mode**: The draft `comment.md` read against recent GitHub issue comments, maintainer directions, and repository contribution / AI-use policies.

### What good looks like
- The plan comment directly engages and respects maintainer guidance present in the thread (e.g., testing maintainer-provided patched binaries or focusing on isolated file paths rather than pushing unwanted workarounds).
- The plan comment complies with repository contribution policies, including mandatory AI-use disclosure statements when required by repository rules (e.g., repositories with explicit AI assistance policies).
