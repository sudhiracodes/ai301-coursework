---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

This tool answers exactly one question about exactly one PR package: **is this pull request ready to submit?** A PR package consists of a candidate pull request (draft title, description, commits, diff, and test evidence) evaluated side-by-side against the plan it claims to implement (`plan.md` including any deviation notes) and the issue that plan addresses. The tool never answers a different question, never evaluates multiple packages simultaneously, and never decides based on subjective impressions: it strictly executes the operating procedure in `procedure.md` to evaluate the checks defined in `rubric.md` against evidence located via `references/evidence-guide.md`.

## Inputs and modes

The tool operates in two distinct modes:

- **Live mode**: Grades the student's own draft PR before submission to GitHub.
  - Inputs:
    - Candidate plan: `plan.md` in the working copy, including any recorded `## Deviations`. (For house-chain students, the house plan and house repro pack).
    - Branch diff: The full unified diff of all committed branch changes compared to the default branch, produced by `git diff main...HEAD` in the working repository.
    - Draft PR title and description: `pr_draft.md`.
    - Test evidence: `test_evidence.md`, containing captured terminal and test suite execution logs.
    - Issue context: The target issue thread on GitHub.
    - Repository standards: The repository's PR template (`.github/PULL_REQUEST_TEMPLATE.md`), contribution guidelines (`docs/CONTRIBUTING.md`), and stated AI policy.
- **Eval mode**: Grades a frozen package bundle (`eval/packages/<item_id>.md`).
  - The package bundle text is the entire universe of evidence.
  - Nothing external is fetched or read, and `scope.md` and `voice-guide.md` are completely ignored.
  - Every check is evaluated fully according to the rubric's verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before examining any other artifact. `scope.md` defines the repository where the student's pull request must target and sets the local room rules.
- If the `Repo:` line in `scope.md` still contains an unfilled placeholder (such as `<ORG>/<PATH-REVIEW-REPO>`), halt immediately without grading and inform the user to configure `scope.md` with their section's Path Review repository. Never guess or infer a scope.
- Refuse to grade any PR package whose target repository falls outside the repository specified in `scope.md`.
- In eval mode, ignore `scope.md` completely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` and compare the outgoing PR title and description against the student's writing rules.
- Check that the title and description follow clear, humble, evidence-backed phrasing and include required AI disclosure and honest scope boundaries.
- Report any violated voice rules in the human-readable summary, citing the specific rule.
- The voice guide never alters the rubric's binary verdict on its own unless a rubric check explicitly evaluates voice compliance.
- In eval mode, ignore `voice-guide.md` completely.

## Component reads

The tool integrates its components through a strict separation of concerns:
- `rubric.md` defines the evaluation checks table and the binary verdict rule.
- `references/evidence-guide.md` maps where each evidence family lives and defines what passing evidence looks like.
- `procedure.md` specifies the step-by-step operating procedure and must be executed as written.
- If `procedure.md` is silent on any required step, report the procedure gap in the summary rather than improvising.
- If `rubric.md` contains no checks or `procedure.md` contains no operating steps, refuse to grade and report that the tool cannot operate without author-defined rubric and procedure content.

## Verdict and output

The verdict space is binary: `accept` (ready to submit) or `reject` (hold). There is no intermediate verdict. Emit a short readable per-check summary, followed immediately by the required fenced JSON block as the very last content in the response, with nothing following it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

Every evaluation must adhere to the following contract rules:
- **Evidence first**: Never assign a grade without citing the specific code hunk, terminal output, or text quote that decided it. "Looks fine" is not evidence.
- **Grade the artifact, not the polish**: A terse PR with decisive evidence and a clean diff is ready; a polished, lengthy description that hides silent drift, unrun tests, or diff debris is not.
- **The rubric decides, not the run**: If a check satisfies its stated pass condition, it passes even if subjective intuition disagrees; improvements belong in the rubric.
- **The procedure decides how**: Execute the procedural steps in order, reporting any omissions instead of inventing workarounds.
- **Unclear defaults to fail**: If an outcome or claim cannot be verified from the package evidence, or if the verdict rule is silent on unclear grades, treat `unclear` as `fail`. A PR whose claims cannot be substantiated is not ready to submit.
