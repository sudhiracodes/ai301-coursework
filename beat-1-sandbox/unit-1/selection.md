# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72

**Verdict output**

```
Grading candidate issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72

- not-archived: pass (Repository codepath/pathreview-ai301-fa26-s1 is active and not archived)
- repo-liveness: pass (Repository shows recent default-branch commits and activity in September 2026)
- ai-policy-permitted: pass (Contributing docs contain no prohibitions against AI-assisted contributions)
- unclaimed: pass (No assignee, no open PRs; Path Review classroom house rule permits claiming)
- bounded-newcomer-scope: pass (Bounded exception handling bug in core/security.py with explicit behavior)
- reproduction-or-spec-present: pass (Names core/security.py and tests/unit/test_security.py with covering xfail test)
- maintainer-endorsed: pass (Filed by collaborator Aburke225 and labeled 'good first issue' / 'tier-1')

Final verdict: accept

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72",
  "checks": [
    {
      "name": "not-archived",
      "grade": "pass",
      "evidence": "Repository codepath/pathreview-ai301-fa26-s1 is active and not archived"
    },
    {
      "name": "repo-liveness",
      "grade": "pass",
      "evidence": "Repository shows recent default-branch commits and activity in September 2026"
    },
    {
      "name": "ai-policy-permitted",
      "grade": "pass",
      "evidence": "Contributing documentation contains no prohibitions against AI-assisted contributions"
    },
    {
      "name": "unclaimed",
      "grade": "pass",
      "evidence": "No assignee, no open PRs; Path Review classroom house rule permits claiming"
    },
    {
      "name": "bounded-newcomer-scope",
      "grade": "pass",
      "evidence": "Bounded exception handling bug in core/security.py with explicit behavior"
    },
    {
      "name": "reproduction-or-spec-present",
      "grade": "pass",
      "evidence": "Names core/security.py and tests/unit/test_security.py with covering xfail test"
    },
    {
      "name": "maintainer-endorsed",
      "grade": "pass",
      "evidence": "Filed by collaborator Aburke225 and labeled 'good first issue' / 'tier-1'"
    }
  ],
  "verdict": "accept"
}
```
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

`Run 1: agreement: 20/20 scored items  (bar: 18/20: PASS)`

**Issue analysis**

Issue `issue-12` (`bookwyrm-social/bookwyrm#1133`):
- Rubric decision: `reject`
- Gold label: `reject`
- Reasoning: `issue-12` describes a clean, well-bounded bug report on an active, living repository with no assignees or conflicting pull requests. However, examining the repository facts reveals that the contribution policy explicitly bans AI-generated code and documentation. Under check `ai-policy-permitted`, an outright ban on AI assistance results in a `fail` grade. Because `ai-policy-permitted` is weighted as `required`, any failure gates the verdict rule, causing the rubric to output `reject`. This prevents students in an AI-assisted workflow from submitting pull requests that violate project governance.

**Check rationale**

Quoted check from `rubric.md`:
`| unclaimed | "this issue: assignees:", "linked PRs:", and Comments section under Repo facts (eval mode) or issue sidebar and thread on github.com (live mode) | No assignee assigned to the issue, NO open linked pull requests actively in progress, and no active claim comment from another contributor within the last 30 days without an explicit maintainer invitation for new contributors (stale claims older than 90 days with maintainer inviting takers pass; in live mode, Path Review house rule applies). | required |`

Reasoning: On open source repositories, issues frequently accumulate historical comments from users saying "Can I take this?" months or years prior, where no pull request was ever submitted or where the maintainer later reopened the issue to new contributors (such as `issue-09`). A naive check that treats any mention of "working on this" as a hard claim causes false rejections on abandoned issues. The check is formulated with concrete thresholds (30-day active window, checking for open linked PRs, and checking for maintainer reopening) so it reliably catches active claims (`issue-03`, `issue-08`, `issue-13`, `issue-18`) while accepting valid stale ones (`issue-09`).

**Trade-offs**

The 30-day recency window on claim comments accepts the risk that a contributor may be working offline for over 30 days without having submitted a draft PR or progress comment. In that rare case, another contributor might start working in parallel. We accept this trade-off because stale comment squatting without follow-through is far more frequent in open source than silent multi-month development, and requiring visible signals (assigned status, open PRs, or fresh comments) prevents good issues from being indefinitely blocked.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. **The issue's fit to your interests and to the time available:**
   Issue #72 directly aligns with my experience and interest in Python backend engineering and robust security practices. The issue involves fixing `verify_password` in `core/security.py` so that passlib's `UnknownHashError` fails closed by returning `False` instead of crashing unhandled on malformed hashes. With an estimated effort of 1–2 hours, it fits my available schedule for Unit 2 and provides a focused, high-impact fix with a clear testing footprint.

2. **What the verdict identified correctly, and what you weighed that the rubric could not:**
   The verdict correctly verified that the repository is active, the issue is unassigned with no conflicting PRs, the scope is localized to a single module, and it has maintainer endorsement as a `tier-1` / `good first issue`. What I weighed beyond the rubric was the clarity of the acceptance criteria: the issue already has a dedicated unit test in `tests/unit/test_security.py` marked `@pytest.mark.xfail` referencing manifest id H-05. This makes reproduction and verification completely unambiguous compared to open-ended feature suggestions.

3. **The anticipated difficulty in claiming it:**
   Claiming the issue will be straightforward. Under the Path Review classroom house rules, multiple students can work on shared issues without friction or blocking claims. The primary implementation task is catching `passlib.exc.UnknownHashError` (and malformed hash format exceptions) inside `verify_password`, verifying that invalid hashes cleanly evaluate to `False`, removing the `@pytest.mark.xfail` marker in `tests/unit/test_security.py`, and confirming that all unit and lint tests pass.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
