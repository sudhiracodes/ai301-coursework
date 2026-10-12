# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/118

**Branch**

fix/72-unknown-hash-error

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

`Run 1: agreement: 20/20 scored items  (bar: 18/20: PASS)`

**Package analysis**

Package `pkg-03` (`starship/starship#7648`):
- Rubric decision: `reject`
- Gold label: `reject`
- Reasoning: In `pkg-03`, while the core one-line fallback fix was implemented, the diff also included an unrequested `truncate()` function rewrite and introduced a new `resolve_symlinks` configuration option. Meanwhile, the PR description claimed it "implements the posted plan exactly" even though the plan explicitly scoped out any configuration change. Check `plan-fidelity` graded this package as `fail` due to unannounced scope and conflicting fidelity claims. Under the binary verdict rule, this required check failure resulted in an overall verdict of `reject`.

**Check rationale**

Quoted check from `rubric.md`:
`| `plan-fidelity` | Candidate diff's modified files and implementation changes compared side-by-side against the candidate plan's scope, boundary, and deviation notes, read alongside the PR description's claims | Every modified file and functional change in the diff falls within the plan's stated scope or is accounted for in an explicit deviation note, and the PR description accurately represents the diff without claiming unexecuted changes (e.g., claiming documentation updates that are missing in the diff) or silently introducing unrequested features, configuration options, broad function rewrites, or untouched file modifications. *(Rationale: Silent scope drift or false fidelity claims mislead maintainers and introduce unreviewed functional surface area.)* | required |`

Reasoning: In pull request reviews, silent drift occurs in two distinct directions: adding unannounced scope beyond what was approved in planning (such as `pkg-03` adding a new config option and rewriting `truncate()`, `pkg-06` bundling theme settings and renaming passes, or `pkg-09` adding an unannounced `--time-format` flag), or claiming more in the PR description than the diff actually delivers (such as `pkg-17` claiming documentation updates that are missing from the diff). The check was specifically designed to require side-by-side pairing between the diff and the plan, and between the diff and the description claims, catching both unannounced additions and unfulfilled claims while explicitly permitting documented deviations.

**Trade-offs**

Enforcing strict plan fidelity across both diff files and description claims means that minor opportunistic improvements (such as drive-by typo fixes or slight helper refactors) will fail the check unless the author records a brief deviation note in the plan or description. We accept this trade-off because unannounced scope drift is a major source of unexpected regressions and review friction in open-source projects, whereas documenting legitimate deviations takes negligible effort and keeps reviewers fully informed.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
