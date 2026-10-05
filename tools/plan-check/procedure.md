# Procedure: how this skill grades a plan package

This procedure defines the operational execution steps followed by the `plan-check` skill to grade a candidate plan package.

---

## Read order

Execute the reading steps in the following strict chronological order before evaluating any checks:

1. **Read the Repository Context and Policies**:
   - In eval mode, read the `Repo facts` block to extract repository-specific constraints, contribution guidelines, and mandatory AI-use disclosure requirements.
   - In live mode, read `scope.md` to confirm the issue belongs to the scoped repository (`codepath/pathreview-ai301-fa26-s1`) and review `voice-guide.md`.
2. **Read the Issue Context and Maintainer Signals**:
   - Read the issue description and thread highlights (or live issue thread on GitHub). Note any explicit instructions, constraints, preferred approaches, or diagnostic findings posted by repository maintainers.
3. **Read the Reproduction Evidence**:
   - Read the reproduction commands, terminal outputs, stack traces, timing matrices, and control runs (such as runs with alternate flags or isolated subcomponents). Note the exact failure mechanism and what factors have been ruled out by control runs.
4. **Read the Candidate Plan and Plan Comment**:
   - Read the candidate `plan.md` (diagnosis, scope, approach, files touched, test plan, risks/unknowns, deviations) and draft `comment.md`.

---

## Evidence gathering

Gather factual evidence for each rubric check using the mappings in `references/evidence-guide.md`:

1. **Diagnosis and Grounding Evidence**:
   - Extract the stated root cause from the candidate plan.
   - Cross-reference with the reproduction logs and control runs to verify whether the stated mechanism is supported by or contradicted by the recorded evidence.
2. **Scope Boundaries Evidence**:
   - Extract the in-scope items, out-of-scope/deferred items, and list of files touched from the candidate plan.
   - Verify whether the proposed changes are strictly confined to fixing the reported defect or whether unrequested refactors, migrations, or feature creep are bundled.
3. **Executability Evidence**:
   - Extract the named files, classes, functions, and implementation steps from the plan.
   - Assess whether the steps provide concrete logic that can be directly executed without open-ended guesswork or deferring core design decisions to build time.
4. **Test Plan Evidence**:
   - Extract the test commands and expected post-fix outcomes from the candidate plan.
   - Compare with the original reproduction steps to verify that an observable pass/fail assertion (exit code, output string, test status) is specified.
5. **Honesty and Uncertainty Evidence**:
   - Extract the identified risks, edge cases, checked unknowns, and post-build deviation records from the candidate plan.
6. **Thread and Convention Compliance Evidence**:
   - Extract the draft plan comment and compare against maintainer thread instructions and repo AI disclosure rules.

---

## Check execution

Grade each check sequentially against the gathered evidence:

1. **Execute `grounded-diagnosis`**:
   - If the diagnosis explains the reproduction output and aligns with all control runs: grade `pass`.
   - If the diagnosis contradicts control runs, misattributes the error to an unaffected module/parser, or ignores evidence: grade `fail`.
2. **Execute `bounded-scope`**:
   - If the plan strictly bounds the fix and defers unrequested architectural overhauls with clear rationale: grade `pass`.
   - If the plan bundles unrequested subsystem rewrites, framework migrations, or UI overhauls: grade `fail`.
3. **Execute `executable-approach`**:
   - If the plan specifies concrete target files and actionable implementation steps: grade `pass`.
   - If the plan lacks concrete file paths, suggests exploratory poking, or defers key design choices: grade `fail`.
4. **Execute `decisive-test-plan`**:
   - If the test plan provides concrete commands with observable pass/fail criteria: grade `pass`.
   - If the test plan is vague (e.g. "feels fast") or merely runs a whole suite without an isolated check: grade `fail`.
5. **Execute `stated-uncertainty`**:
   - If risks, edge cases, or checked unknowns are transparently documented (or minimal risk noted): grade `pass`.
   - If obvious risks are dismissed with false certainty: grade `fail`.
6. **Execute `thread-convention-adherent`**:
   - If the comment respects maintainer thread directions and includes mandatory AI disclosures when required: grade `pass`.
   - If maintainer guidance is ignored or mandatory AI disclosure is missing: grade `fail`.

---

## Verdict assembly

Assemble the final evaluation output following these deterministic rules:

1. **Evaluate Required Checks**:
   - If all `required` checks evaluate to `pass`: the final verdict is `accept`.
   - If any `required` check evaluates to `fail` or `unclear`: the final verdict is `reject`.
2. **Format Human-Readable Summary**:
   - List each check name, its assigned grade (`pass`, `fail`, or `unclear`), and a concise one-line evidence statement citing the deciding fact or quote.
   - In live mode, include any voice-guide rule violations or deviation notes.
3. **Format Machine-Readable Output**:
   - Emit the final result as a single fenced JSON block at the very end of the response:
   ```json
   {
     "item": "<issue URL or bundle id>",
     "checks": [
       {"name": "<check name>", "grade": "pass|fail|unclear", "evidence": "<one-line deciding evidence>"}
     ],
     "verdict": "accept|reject"
   }
   ```
