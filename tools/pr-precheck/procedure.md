# Procedure: how this tool grades a PR package

This procedure defines the operational execution steps followed by the `pr-precheck` tool to grade a candidate PR package.

---

## Read order

Execute the reading steps in the following strict chronological order before evaluating any checks:

1. **Read Repository Context and Policies**:
   - In eval mode, read the `Repo facts` block to extract repository-specific constraints, required PR template sections/checklists, contribution guidelines, and mandatory AI-use disclosure requirements.
   - In live mode, read `scope.md` to confirm the PR targets the scoped repository (`codepath/pathreview-ai301-fa26-s1`) and review `voice-guide.md`.
2. **Read the Candidate Plan and Deviation Notes**:
   - Read the plan's stated scope, boundary, and in-scope vs. out-of-scope declarations (`plan.md` in live mode or the `Plan context` block in eval mode).
   - Note any explicit deviation notes or deferred scope recorded by the author.
   - Read the plan's test plan: note the exact commands and expected observable before-and-after failure/resolution criteria.
3. **Read the Candidate Diff and Commits**:
   - Read the unified diff and commit list.
   - List every file touched by the diff, each functional change made, and any potential debris or churn hunks.
4. **Read the Test Evidence**:
   - Read the recorded test commands and outputs (`test_evidence.md` in live mode or the `Test evidence` block in eval mode).
   - Compare the executed tests against the reproduction steps and test plan noted in Step 2.
5. **Read the Candidate PR Title and Description**:
   - Read the candidate title and description (`pr_draft.md` in live mode).
   - Check that all template sections and checklists are populated, fidelity claims match the diff, and required disclosures are present.

---

## Evidence gathering

Gather factual evidence for each rubric check using the mappings in `references/evidence-guide.md`:

1. **Gather Plan Fidelity Evidence (Side-by-Side Pairing 1)**:
   - Pair the list of files and functional changes in the diff against the plan's scope and deviation notes.
   - Record whether any file or feature in the diff is absent from the plan without a written deviation note.
   - Pair the PR description's claims about what was implemented against the actual diff changes.
   - Record whether the description claims changes that are absent from the diff (e.g. docs promised but omitted) or claims "exact implementation" when unrequested options/rewrites were added.
2. **Gather Test Evidence (Side-by-Side Pairing 2)**:
   - Pair the executed test commands and outputs against the plan's test plan and the original failure reproduction.
   - Record whether the failing path was re-run with observable before-and-after proof, or whether only an unaffected control path / vague qualitative statement was provided.
3. **Gather Diff Quality Evidence**:
   - Inspect all diff hunks for debugging statements (e.g. `eprintln!`, `console.log`, temporary prints), dead/unused functions, commented-out obsolete code, duplicate lines, or formatting/import churn.
   - Record any offending hunks or confirm the diff is clean.
4. **Gather Standards and Comms Evidence**:
   - Compare the candidate PR description and title against the repository's PR template and policies from Step 1.
   - Record whether all required template checklists were preserved and completed, and whether mandatory AI disclosures were included.

---

## Check execution

Grade each check sequentially against the gathered evidence:

1. **Execute `plan-fidelity`**:
   - If every diff change falls within the plan or its documented deviations, and description fidelity claims accurately reflect the diff: grade `pass`.
   - If the diff introduces unannounced scope (e.g. new options, unrequested rewrites, extra files), omits promised changes without a note, or the description claims changes not in the diff: grade `fail`.
2. **Execute `test-evidence`**:
   - If the test evidence shows observable before-and-after proof re-running the specific failure scenario and repo test checks: grade `pass`.
   - If the reproduction failure scenario was never re-run (e.g. only unaffected controls executed) or evidence relies on vague qualitative assertions: grade `fail`.
3. **Execute `diff-quality`**:
   - If the diff is clean, focused, and free of debug logs, dead code, commented-out blocks, and formatting churn: grade `pass`.
   - If the diff contains leftover debug prints, dead functions behind compiler allowances, commented-out experiments, or mechanical churn: grade `fail`.
4. **Execute `standards-compliance`**:
   - If the PR description fills all required template sections/checklists and includes mandatory AI disclosures per repository policy: grade `pass`.
   - If required template checklists are omitted or mandatory AI disclosures are missing: grade `fail`.

---

## Verdict assembly

Assemble the final evaluation output following these deterministic rules:

1. **Evaluate Required Checks**:
   - If all `required` checks evaluate to `pass`: the final verdict is `accept`.
   - If any `required` check evaluates to `fail` or `unclear`: the final verdict is `reject`.
2. **Format Human-Readable Summary**:
   - Output a concise summary listing each check name, its assigned grade (`pass`, `fail`, or `unclear`), and a one-line evidence statement citing the deciding fact, hunk, or quote.
   - If multiple required checks fail, quote the first failing required check in rubric order as the primary deciding failure.
   - In live mode, report any broken rules from `voice-guide.md`.
3. **Format Machine-Readable Output**:
   - Emit the final result as a single fenced JSON block at the very end of the response:
   ```json
   {
     "item": "<PR URL or bundle id>",
     "checks": [
       {"name": "<check name>", "grade": "pass|fail|unclear", "evidence": "<one-line deciding evidence>"}
     ],
     "verdict": "accept|reject"
   }
   ```
