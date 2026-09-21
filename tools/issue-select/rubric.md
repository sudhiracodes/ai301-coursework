# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `not-archived` | `archived:` status on the repo line under Repo facts (eval mode) or GitHub repository header (live mode) | The repository is active and not archived (`archived: false`). An archived repository fails immediately. | required |
| `repo-liveness` | "last 5 default-branch commits", "last push to any branch", and "maintainer first-response sample" under Repo facts (eval mode) or commit/push timestamps on github.com (live mode) | At least one human commit or push to any branch within the last 180 days relative to capture date (or today in live mode), OR maintainer response activity within the last 180 days. Repositories with no commits or maintainer activity for over 180 days fail. | required |
| `ai-policy-permitted` | "contribution policy" line under Repo facts (eval mode) or `CONTRIBUTING.md`, `AI_POLICY.md` on github.com (live mode) | The repository does NOT explicitly ban AI-assisted or AI-generated contributions. Policies that are silent or that impose conditions (e.g. disclosure, human verification, testing) pass. | required |
| `unclaimed` | "this issue: assignees:", "linked PRs:", and Comments section under Repo facts (eval mode) or issue sidebar and thread on github.com (live mode) | No assignee assigned to the issue, NO open linked pull requests actively in progress, and no active claim comment from another contributor within the last 30 days without an explicit maintainer invitation for new contributors (stale claims older than 90 days with maintainer inviting takers pass; in live mode, Path Review house rule applies). | required |
| `bounded-newcomer-scope` | Issue title, body text, and comment thread | The issue is a single, self-contained, bounded task (bug fix, documentation change, or localized feature) with clear direction. Explicit umbrella tracking lists, megaissues, codebase-wide refactors, or open-ended feature wishes with unresolved design debate fail. | required |
| `reproduction-or-spec-present` | Issue body, test cases, and comments section | The issue provides explicit reproduction steps, failing test fixtures, error traces, or concrete pointers to relevant files/functions. | preferred |
| `maintainer-endorsed` | Author `author_association` badge (Owner, Member, Collaborator) or labels under Repo facts / issue header | The issue was authored by a maintainer/collaborator OR carries a beginner-friendly label (e.g., `good first issue`, `good-first-issue`, `tier-1`, `help wanted`). | preferred |

## Verdict rule

An issue receives an `accept` verdict if and only if it passes all `required` checks. If any `required` check receives a grade of `fail` or `unclear`, the final verdict is `reject`.

Checks weighted as `preferred` do not affect the `accept` or `reject` verdict; they are reported and used solely to evaluate fit and rank candidate issues that have been accepted.
