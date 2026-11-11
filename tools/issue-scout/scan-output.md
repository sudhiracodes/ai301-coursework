# Scan Output: 2026-10-28

## Summary

The scout scan ran 3 queries derived from the student fit profile across GitHub:
- **Query 1 (`test`)**: `gh search issues "test" --language python --label "good first issue" --state open --limit 100` (from: *"I prefer well-bounded bug fixes, test fixture repairs, parser edge case handling..."*)
- **Query 2 (`fastapi`)**: `gh search issues "fastapi" --language python --label "good first issue" --state open --limit 100` (from: *"I have strong experience with Python, FastAPI backend services..."*)
- **Query 3 (`rag`)**: `gh search issues "rag" --language python --label "good first issue" --state open --limit 100` (from: *"I have strong experience with Python, FastAPI backend services, pytest testing suites, and RAG/LLM application development."*)

Across the 3 queries, **297 unique open issues** were surfaced.
Applying the ranking filters (drop assigned, drop >= 3 comments, drop inactive > 60 days, order by fewest comments then recency) yielded **167 survivors**.
The top 5 survivors were evaluated against `rubric.md`.

All 5 candidates received an **`accept`** verdict (passing all required checks: `not-archived`, `repo-liveness`, `ai-policy-permitted`, `unclaimed`, `bounded-newcomer-scope`).

### Ranked Accepted Candidates (in Fit Order)

1. **`https://github.com/iamvisheshsrivastava/AirKube/issues/25`**
   - **Fit reason**: Directly targets FastAPI backend endpoints (`/model/{name}/details` and `/deployment/{id}/status`) in a LangGraph/LLM architecture needing pytest coverage in `tests/test_inference.py`. Perfect alignment with Python, FastAPI, pytest, and LLM skills.
   - **Surfaced by**: Query 2 (`fastapi`)
   - **Ranking signals**: 0 comments, 0 assignees, updated within 24 hours.

2. **`https://github.com/iamvisheshsrivastava/spec2program/issues/32`**
   - **Fit reason**: Localized FastAPI bug fix in `backend/main.py` adding version reporting to `/api/health` liveness probe for an LLM-assisted code generator. Fits preference for bounded FastAPI backend bug fixes.
   - **Surfaced by**: Query 2 (`fastapi`)
   - **Ranking signals**: 0 comments, 0 assignees, updated within 24 hours.

3. **`https://github.com/iamvisheshsrivastava/geospatial/issues/39`**
   - **Fit reason**: Adds pytest test coverage for FastAPI endpoint `/change-detect` in `tests/test_api.py`. Fits preference for pytest testing suites and API endpoint test fixture repairs.
   - **Surfaced by**: Query 1 (`test`)
   - **Ranking signals**: 0 comments, 0 assignees, updated within 24 hours.

4. **`https://github.com/X-4xu/fang-ssh/issues/6`**
   - **Fit reason**: Synthetic pytest test suite expansion for IPv6 parser edge cases and CIDR notation. Matches profile focus on parser edge case handling and pytest.
   - **Surfaced by**: Query 1 (`test`)
   - **Ranking signals**: 0 comments, 0 assignees, updated within 24 hours.

5. **`https://github.com/connabyss/bite/issues/17`**
   - **Fit reason**: CI matrix test suite runner for macOS path handling and BSD tar edge cases. Aligns with pytest testing suites, though CI-focused rather than pure backend code.
   - **Surfaced by**: Query 1 (`test`)
   - **Ranking signals**: 0 comments, 0 assignees, updated within 24 hours.

```json
[
  {
    "item": "https://github.com/iamvisheshsrivastava/AirKube/issues/25",
    "checks": [
      {
        "name": "not-archived",
        "grade": "pass",
        "evidence": "Repository is active (isArchived: false)."
      },
      {
        "name": "repo-liveness",
        "grade": "pass",
        "evidence": "Repository default branch pushed within the last 24 hours (2026-10-28T19:34:10Z)."
      },
      {
        "name": "ai-policy-permitted",
        "grade": "pass",
        "evidence": "CONTRIBUTING.md is silent on AI restrictions; no ban on AI-assisted contributions."
      },
      {
        "name": "unclaimed",
        "grade": "pass",
        "evidence": "0 assignees, 0 open linked PRs, and 0 comments on the issue thread."
      },
      {
        "name": "bounded-newcomer-scope",
        "grade": "pass",
        "evidence": "Self-contained task adding pytest test fixtures in tests/test_inference.py for two specific endpoints."
      },
      {
        "name": "reproduction-or-spec-present",
        "grade": "pass",
        "evidence": "Spec explicitly lists source file ml/inference.py, endpoints, and target test file tests/test_inference.py."
      },
      {
        "name": "maintainer-endorsed",
        "grade": "pass",
        "evidence": "Created by repository owner with 'good first issue' label."
      }
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/iamvisheshsrivastava/spec2program/issues/32",
    "checks": [
      {
        "name": "not-archived",
        "grade": "pass",
        "evidence": "Repository is active (isArchived: false)."
      },
      {
        "name": "repo-liveness",
        "grade": "pass",
        "evidence": "Repository pushed within the last 24 hours (2026-10-28T19:34:18Z)."
      },
      {
        "name": "ai-policy-permitted",
        "grade": "pass",
        "evidence": "CONTRIBUTING.md contains no AI bans; offline mock LLM mode supported for local testing."
      },
      {
        "name": "unclaimed",
        "grade": "pass",
        "evidence": "0 assignees, 0 open linked PRs, and 0 comments on the thread."
      },
      {
        "name": "bounded-newcomer-scope",
        "grade": "pass",
        "evidence": "Bounded single-file enhancement to backend/main.py adding app version field to /api/health probe."
      },
      {
        "name": "reproduction-or-spec-present",
        "grade": "pass",
        "evidence": "Issue body quotes the current health endpoint implementation and defines expected output."
      },
      {
        "name": "maintainer-endorsed",
        "grade": "pass",
        "evidence": "Authored by repository owner with 'good first issue' label."
      }
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/iamvisheshsrivastava/geospatial/issues/39",
    "checks": [
      {
        "name": "not-archived",
        "grade": "pass",
        "evidence": "Repository is active (isArchived: false)."
      },
      {
        "name": "repo-liveness",
        "grade": "pass",
        "evidence": "Repository default branch pushed within the last 24 hours (2026-10-28T19:33:56Z)."
      },
      {
        "name": "ai-policy-permitted",
        "grade": "pass",
        "evidence": "CONTRIBUTING.md has no AI bans; requires clear docstrings and focused PR tests."
      },
      {
        "name": "unclaimed",
        "grade": "pass",
        "evidence": "0 assignees, 0 open linked PRs, and 0 comments on thread."
      },
      {
        "name": "bounded-newcomer-scope",
        "grade": "pass",
        "evidence": "Task is scoped strictly to adding unit tests for POST /change-detect in tests/test_api.py."
      },
      {
        "name": "reproduction-or-spec-present",
        "grade": "pass",
        "evidence": "Issue details current coverage gaps and points to existing endpoint implementation."
      },
      {
        "name": "maintainer-endorsed",
        "grade": "pass",
        "evidence": "Created by repo owner with 'good first issue' label."
      }
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/X-4xu/fang-ssh/issues/6",
    "checks": [
      {
        "name": "not-archived",
        "grade": "pass",
        "evidence": "Repository is active (isArchived: false)."
      },
      {
        "name": "repo-liveness",
        "grade": "pass",
        "evidence": "Repository pushed within the last 24 hours (2026-10-28T22:43:36Z)."
      },
      {
        "name": "ai-policy-permitted",
        "grade": "pass",
        "evidence": "CONTRIBUTING.md enforces fail-safe parsing and deterministic logic with no AI bans."
      },
      {
        "name": "unclaimed",
        "grade": "pass",
        "evidence": "0 assignees, 0 open PRs, and 0 comments."
      },
      {
        "name": "bounded-newcomer-scope",
        "grade": "pass",
        "evidence": "Bounded task adding synthetic unit test cases for IPv6 parser edge cases."
      },
      {
        "name": "reproduction-or-spec-present",
        "grade": "pass",
        "evidence": "Explicitly enumerates test categories: compressed IPv6, IPv4-mapped IPv6, and link-local scope IDs."
      },
      {
        "name": "maintainer-endorsed",
        "grade": "pass",
        "evidence": "Authored by owner X-4xu with 'good first issue' label."
      }
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/connabyss/bite/issues/17",
    "checks": [
      {
        "name": "not-archived",
        "grade": "pass",
        "evidence": "Repository is active (isArchived: false)."
      },
      {
        "name": "repo-liveness",
        "grade": "pass",
        "evidence": "Repository pushed within the last 24 hours (2026-10-29T02:51:01Z)."
      },
      {
        "name": "ai-policy-permitted",
        "grade": "pass",
        "evidence": "CONTRIBUTING.md explicitly welcomes AI-assisted PRs with disclosure."
      },
      {
        "name": "unclaimed",
        "grade": "pass",
        "evidence": "0 assignees, 0 linked PRs, and 0 comments."
      },
      {
        "name": "bounded-newcomer-scope",
        "grade": "pass",
        "evidence": "Bounded CI configuration addition to .github/workflows/tests.yml with targeted macOS path adjustments."
      },
      {
        "name": "reproduction-or-spec-present",
        "grade": "pass",
        "evidence": "Points to workflow file and specifies known macOS differences (/private/var, BSD tar)."
      },
      {
        "name": "maintainer-endorsed",
        "grade": "pass",
        "evidence": "Authored by owner connabyss with 'good first issue' label."
      }
    ],
    "verdict": "accept"
  }
]
```

---

# Direct Candidate Grading: Issue Tracker Pick

## Summary: https://github.com/open-telemetry/opentelemetry.io/issues/9427
- **Title**: `[pt] Localize content/en/docs/collector pages`
- **Repo**: `open-telemetry/opentelemetry.io`
- **Mode**: direct
- **Checks**:
  - `not-archived`: pass (Repo is active: isArchived: false)
  - `repo-liveness`: pass (Recent default-branch commits pushed within the last 24 hours)
  - `ai-policy-permitted`: pass (CONTRIBUTING.md and localization guidelines contain no prohibition against AI-assisted drafting provided human review, cspell, and glossary standards are adhered to)
  - `unclaimed`: pass (0 assignees on the umbrella issue itself; claims are coordinated per comment thread for individual pages)
  - `bounded-newcomer-scope`: fail (The issue is an explicit umbrella tracking checklist with dozens of sub-pages (`- [x]`, `- [ ]`), violating the rubric rule that umbrella tracking lists fail)
  - `reproduction-or-spec-present`: pass (Clear reference to original English markdown paths (`content/en/docs/collector/...`) and Portuguese translation guide)
  - `maintainer-endorsed`: pass (Authored by member with `good first issue`, `help wanted`, and `triage:accepted:needs-pr` labels)
- **Final Verdict**: `reject` (Failed required check: `bounded-newcomer-scope` due to umbrella tracking list)
- **Fit to Profile**: Low fit. Translating Hugo documentation into Portuguese does not leverage Python, FastAPI, pytest testing suites, or RAG/LLM development.

```json
{
  "item": "https://github.com/open-telemetry/opentelemetry.io/issues/9427",
  "checks": [
    {
      "name": "not-archived",
      "grade": "pass",
      "evidence": "Repository open-telemetry/opentelemetry.io is active (isArchived: false)."
    },
    {
      "name": "repo-liveness",
      "grade": "pass",
      "evidence": "Repository default-branch commits and pushes active within the last 24 hours."
    },
    {
      "name": "ai-policy-permitted",
      "grade": "pass",
      "evidence": "CONTRIBUTING.md and localization guidelines do not ban AI assistance; requires human review and adherence to terminology glossary."
    },
    {
      "name": "unclaimed",
      "grade": "pass",
      "evidence": "0 assignees on the umbrella issue itself; contributors claim individual sub-pages in comments."
    },
    {
      "name": "bounded-newcomer-scope",
      "grade": "fail",
      "evidence": "Explicit umbrella tracking issue containing a checklist of dozens of sub-pages across multiple directories."
    },
    {
      "name": "reproduction-or-spec-present",
      "grade": "pass",
      "evidence": "Detailed guide and glossary linked in body, with explicit checklist of target file paths."
    },
    {
      "name": "maintainer-endorsed",
      "grade": "pass",
      "evidence": "Authored by organization member with 'good first issue' and 'help wanted' labels."
    }
  ],
  "verdict": "reject"
}
```

---

## Summary: https://github.com/vllm-project/production-stack/issues/531
- **Title**: `feature: Refactor the CI workflow`
- **Repo**: `vllm-project/production-stack`
- **Mode**: direct
- **Checks**:
  - `not-archived`: pass (Repo is active: isArchived: false)
  - `repo-liveness`: pass (Repository active with recent pushes)
  - `ai-policy-permitted`: pass (Standard vLLM project policies; no blanket AI ban)
  - `unclaimed`: fail (Active contributor ighutake-debug has merged PR #1024 and currently has open linked PR #1085 actively addressing the tasks)
  - `bounded-newcomer-scope`: fail (Codebase-wide CI pipeline refactoring involving Helm charts, Minikube, GPU self-hosted runners, and mock servers; previous attempt PR #924 failed due to oversized scope)
  - `reproduction-or-spec-present`: pass (Outlines target pipelines and refactoring goals)
  - `maintainer-endorsed`: pass (Labeled 'good first issue' and 'help wanted' by maintainers)
- **Final Verdict**: `reject` (Failed required checks: `unclaimed` and `bounded-newcomer-scope`)
- **Fit to Profile**: Low fit. Infrastructure / CI / Helm / Minikube configuration rather than bounded Python application code, FastAPI endpoints, or pytest suite fixes.

```json
{
  "item": "https://github.com/vllm-project/production-stack/issues/531",
  "checks": [
    {
      "name": "not-archived",
      "grade": "pass",
      "evidence": "Repository vllm-project/production-stack is active (isArchived: false)."
    },
    {
      "name": "repo-liveness",
      "grade": "pass",
      "evidence": "Repository default branch pushed recently (active maintainer activity)."
    },
    {
      "name": "ai-policy-permitted",
      "grade": "pass",
      "evidence": "No explicit AI contribution bans found in repository guidelines."
    },
    {
      "name": "unclaimed",
      "grade": "fail",
      "evidence": "Active linked open PR #1085 by contributor ighutake-debug currently in progress."
    },
    {
      "name": "bounded-newcomer-scope",
      "grade": "fail",
      "evidence": "Multi-pipeline architectural CI refactor across Helm and GitHub Actions with historical oversized PR failures."
    },
    {
      "name": "reproduction-or-spec-present",
      "grade": "pass",
      "evidence": "Issue body outlines 4 distinct pipeline refactoring goals and target workflow files."
    },
    {
      "name": "maintainer-endorsed",
      "grade": "pass",
      "evidence": "Labeled 'good first issue' and 'help wanted' by project maintainers."
    }
  ],
  "verdict": "reject"
}
```


