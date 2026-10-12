# Rubric: is this pull request ready to submit?

<!--
This rubric defines the checks and verdict rules executed by the pr-precheck tool.
Each check evaluates a specific dimension of a candidate PR package against
evidence mapped in references/evidence-guide.md.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `plan-fidelity` | Candidate diff's modified files and implementation changes compared side-by-side against the candidate plan's scope, boundary, and deviation notes, read alongside the PR description's claims | Every modified file and functional change in the diff falls within the plan's stated scope or is accounted for in an explicit deviation note, and the PR description accurately represents the diff without claiming unexecuted changes (e.g., claiming documentation updates that are missing in the diff) or silently introducing unrequested features, configuration options, broad function rewrites, or untouched file modifications. *(Rationale: Silent scope drift or false fidelity claims mislead maintainers and introduce unreviewed functional surface area.)* | required |
| `test-evidence` | Candidate PR's test evidence section and testing description compared against the plan's test plan and the original issue's reproduction steps | The test evidence provides concrete, observable before-and-after proof re-running the specific failure scenario and reproduction steps identified in the plan (rather than only testing an unaffected control path or asserting vague qualitative success like "tested locally" or "works on my machine"), demonstrating that the defect was directly reproduced and resolved. *(Rationale: Unsubstantiated test claims leave bugs unverified and waste maintainer triage time.)* | required |
| `diff-quality` | Unified diff hunks, commits, and codebase modifications across the branch | The diff is clean, focused, and reviewable: free of debugging debris (e.g. temporary logging or print statements in execution loops), dead or unused code (e.g. probe functions or dead imports), commented-out obsolete experiments, and unrelated formatting, whitespace, or import churn. *(Rationale: Review debris and mechanical churn distract reviewers and introduce maintenance burdens into production code.)* | required |
| `standards-compliance` | Candidate PR description, title, checklist items, and disclosures read against the repo-facts block (PR template requirements, contribution guidelines, stated AI policy) and thread highlights | The PR description completes all required sections and checklists mandated by the repository's PR template (not omitting required checklist sections) and fulfills all stated project policies, including mandatory AI-use disclosure statements in the author's own words where required by repo policy. *(Rationale: Ignoring repository checklists and disclosure policies violates community guidelines and results in PR rejection.)* | required |

## Verdict rule

A pull request package is **accept** (ready to submit) if and only if every `required` check passes (`pass`). If any `required` check fails (`fail`) or is undetermined (`unclear`), the verdict is **reject** (hold). `preferred` checks provide constructive feedback but never alter the binary verdict.
