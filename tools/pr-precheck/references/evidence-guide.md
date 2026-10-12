# Evidence guide: where evidence lives in a PR package

This guide maps each evidence family required by the `pr-precheck` rubric to its exact location in eval packages and live GitHub repositories, defining observable standards for what constitutes passing evidence.

The four evidence families below correspond directly to the eval harness's failure categories:
- Plan fidelity = `silent-drift`
- Test evidence = `not-tested`
- Diff quality = `unreviewable`
- Standards and comms = `standards-wall`
A PR that fails none of these categories is a `clear-accept`.

---

## Plan fidelity (harness category: silent-drift)

### Where it lives
- **Eval mode**: The `Plan context` block (specifically the in-scope vs. out-of-scope declarations, files named, and deviation notes), compared side-by-side against the candidate PR's unified diff and the PR description's claims about fidelity.
- **Live mode**: `plan.md` in the repository root (including its `## Scope` and `## Deviations` sections), compared side-by-side against the branch's unified diff (`git diff main...HEAD`) and the candidate PR description in `pr_draft.md`.

### What good looks like
- Every file modified in the diff falls strictly within the plan's stated scope or is explicitly accounted for in a written deviation note.
- The diff does not introduce unrequested feature additions, new configuration options/flags (e.g., adding `--time-format` or `resolve_symlinks` when unrequested), broad function rewrites (e.g., rewriting `truncate()`), or touch unrelated files (e.g., rewriting `print/property.js`).
- The PR description does not claim fidelity that the diff contradicts (e.g., claiming "implements plan exactly" when extra options are added, or claiming documentation was updated when the diff contains only code warnings and no docs).
- Honest shortfalls or deferred scope that are explicitly documented in deviation notes or the description (e.g., deferring untestable Windows paths or `%VAR%` expansion with clear technical reasons) pass this check.

---

## Test evidence (harness category: not-tested)

### Where it lives
- **Eval mode**: The candidate PR's `Test evidence` section (and description testing sections), compared against the `Plan context`'s test plan and the original issue's reproduction steps.
- **Live mode**: `test_evidence.md` (and the Testing section of `pr_draft.md`), compared against `plan.md`'s `## Test Plan` and the original reproduction commands.

### What good looks like
- The test evidence provides concrete, observable before-and-after proof re-running the specific failure scenario and reproduction steps identified in the plan (e.g., re-running the exact command that previously failed or produced an incorrect exit code, showing the post-fix passing result).
- Testing only an unaffected control path (e.g., running single-file commands when the bug was multi-file, or testing GET header casing when the bug was POST Content-Type) does not satisfy this check.
- Vague qualitative assertions without raw terminal or test outputs (such as "tested locally, colors work now", "verified working on my machine for a full day", or "tests pass") fail this check.
- The repository's standard test checks (e.g., unit test suite, linter, typechecker) are executed and reported honestly with their actual outputs.

---

## Diff quality (harness category: unreviewable)

### Where it lives
- **Eval mode**: The candidate PR's unified diff and commit list.
- **Live mode**: The unified diff produced by `git diff main...HEAD` and the branch commit history (`git log main..HEAD`).

### What good looks like
- The diff is clean, cohesive, and easily reviewable: only the necessary functional changes and accompanying test updates are present.
- The diff contains no debugging debris (e.g. temporary `eprintln!`, `print()`, or console logs left in critical paths), no dead or unused functions (e.g. `_unused_buffer_probe` or `count_open_fds` behind `allow(dead_code)`), and no commented-out experimental blocks.
- The diff does not include unneeded whitespace/re-indentation churn, duplicate re-printed lines, or unrelated import-sorting restructures.
- Commits are meaningful rather than strings of unstructured work-in-progress debris ("wip", "misc cleanups while debugging").

---

## Standards and comms (harness category: standards-wall)

### Where it lives
- **Eval mode**: The candidate PR description and title, read against the `Repo facts` block (PR template requirements, checklist asks, contributing guidelines, and stated AI policy) and issue thread highlights.
- **Live mode**: `pr_draft.md` read against `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, the repository's AI policy, and the live GitHub issue thread.

### What good looks like
- The PR description uses and completes all required sections and checklists specified by the repository's PR template (e.g., completing mandatory checklist checkboxes for changelog, tests, and formatting rather than omitting them).
- The PR description satisfies all explicit repository contribution policies, including providing mandatory AI-use disclosure statements in the author's own words when required by project policy (e.g., Ghostty's policy requiring disclosure of AI assistance).
- The PR respects any explicit maintainer guidance or instructions present on the issue thread.
