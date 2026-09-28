# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `environment-recorded` | "Environment:" line or subsection in the repro report | Explicitly records the OS and runtime/tool version or git commit tested, and explicitly acknowledges any version delta from the issue. Omitting the environment or silent unacknowledged version skew fails. *(Rationale: Maintainers cannot validate a bug report without knowing the environment, especially for platform- or version-dependent failures.)* | required |
| `steps-followable` | Reproduction steps and commands in the repro report | Step-by-step commands and inputs are complete and executable by a stranger from a clean state. Steps that rely on private unshared repos, missing local configs, or omitted mandatory flags fail. *(Rationale: Repros with missing prerequisites cannot be re-run by outside collaborators.)* | required |
| `artifact-present` | Terminal excerpts, output logs, or test traces in the repro report | The report includes concrete output artifacts (command outputs, crash traces, exit codes, or measurements). Reports with zero artifacts, pure "+1 / me-too" commentary, or blank outputs fail. *(Rationale: Verifiable proof requires observable artifacts, not bare assertions.)* | required |
| `behavior-faithful` | Output artifacts compared against the issue context description | The artifact demonstrates the exact error message, failure mode, or exit code described in the issue (or an honest cannot-reproduce attempt). Mismatched syntax errors, unbound variable compile failures, or unacknowledged legacy version behaviors fail. *(Rationale: Misattributing unrelated errors creates false positives and wastes triage time.)* | required |
| `honest-narration` | Report summary and narrative claims read against the output artifacts | Narrative claims strictly reflect what the evidence demonstrates. Confident claims of reproduction or root cause backed by no evidence or contradictory outputs fail; honest, evidenced cannot-reproduce reports with clear hypotheses pass. *(Rationale: Overconfident unsupported claims mislead triagers and pollute issue discussions.)* | required |
| `claim-intent-specific` | Candidate claim comment | The claim comment identifies the specific issue/scope and states concrete next steps (promising investigation/repro), rather than generic "assign me" boilerplate, +1 comments, or overpromising guaranteed fix deadlines. *(Rationale: Meaningful claims prevent issue tracker coordination breakdown and maintain realistic project expectations.)* | required |
| `ai-policy-compliant` | Repository AI policy line under Repo facts compared against the package comments | If the repository's stated policy explicitly mandates AI-use disclosure for contributor comments/PRs, the package includes the required disclosure statement. Silence or permissive policies with no disclosure requirement pass. *(Rationale: Violating explicit disclosure mandates breaches community trust and triggers comment deletion.)* | required |
| `control-run-present` | Repro report commands and output artifacts | Includes a baseline control run or working counter-example that contrasts working versus failing behavior. *(Rationale: Control runs help isolate the failure mechanism, though not every bug strictly requires a control run.)* | preferred |

## Verdict rule

A reproduction package receives an `accept` verdict if and only if every `required` check passes. If any `required` check receives a grade of `fail` or `unclear` (subject to the claim-only draft rule in `SKILL.md` where repro-only checks are marked not yet applicable), the verdict is `reject`.

Checks weighted as `preferred` do not affect the `accept` or `reject` verdict; they are reported to evaluate reproduction quality and provide feedback.
