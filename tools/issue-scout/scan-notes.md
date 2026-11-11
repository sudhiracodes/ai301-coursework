# Scan Notes

## 2026-10-28: Initial Live Scan & Strategy Evaluation

- **What ran**: Executed initial 3-query scan across GitHub (`test`, `fastapi`, `rag` with `--language python --label "good first issue" --state open --limit 100`).
- **Surface & Yield**: 297 unique candidates surfaced. The drop filters (assignees present, commentsCount >= 3, updatedAt > 60 days) filtered the set to 167 viable survivors.
- **Top 5 Survivor Evaluation**:
  - `iamvisheshsrivastava/AirKube#25`: Accepted. High fit — FastAPI endpoint test coverage with mock fixtures in LangGraph LLM architecture.
  - `iamvisheshsrivastava/spec2program#32`: Accepted. High fit — localized FastAPI health probe update reporting app version.
  - `iamvisheshsrivastava/geospatial#39`: Accepted. High fit — adding missing unit tests for `POST /change-detect`.
  - `X-4xu/fang-ssh#6`: Accepted. Moderate-high fit — synthetic unit tests for IPv6 parser edge cases.
  - `connabyss/bite#17`: Accepted. Moderate fit — CI workflow macOS matrix runner addition.
- **Observations & Calibrations**:
  - *Maintainer clustering*: 3 of the top 5 came from `iamvisheshsrivastava`, who systematically files clean, beginner-friendly issues. This is a common pattern in active open source orgs.
  - *DevOps / CI vs Application code*: `connabyss/bite#17` involves `.github/workflows/tests.yml` rather than pure Python backend code. The rubric correctly judged it as bounded, but in future search iterations, adding a negative keyword or ranking down `area/ci` / `devops` labels could bias results even further toward application-level code.
  - *Rubric integrity*: All 5 required checks (`not-archived`, `repo-liveness`, `ai-policy-permitted`, `unclaimed`, `bounded-newcomer-scope`) evaluated cleanly against live GitHub API endpoints. AI policy checks confirmed none of the repos banned AI assistance (and `connabyss/bite` explicitly welcomed it).
  - *Outcome*: Nothing broke in the rubric execution; search queries and ranking filters performed accurately.

## 2026-10-28: Direct Grading of Tracker Pick (open-telemetry/opentelemetry.io#9427)

- **Candidate**: `https://github.com/open-telemetry/opentelemetry.io/issues/9427` (`[pt] Localize content/en/docs/collector pages`).
- **Scout Verdict**: `reject`.
- **Failure Cause**: Failed required check `bounded-newcomer-scope`. The issue is an explicit multi-page umbrella tracking checklist with dozens of sub-items rather than a single self-contained, bounded task.
- **Fit Evaluation**: As Brazilian Portuguese documentation localization, it does not match the student's Python / FastAPI / pytest profile. However, per course guidelines: if the student still chooses to take a specific sub-page from this umbrella issue, they can do so by explaining their reasoning honestly in their contribution story.

## 2026-10-28: Direct Grading of Tracker Pick (vllm-project/production-stack#531)

- **Candidate**: `https://github.com/vllm-project/production-stack/issues/531` (`feature: Refactor the CI workflow`).
- **Scout Verdict**: `reject`.
- **Failure Causes**:
  1. Failed required check `unclaimed`: Contributor `ighutake-debug` already merged PR #1024 for part 1 and currently has an active open PR (#1085) for part 2.
  2. Failed required check `bounded-newcomer-scope`: Multi-pipeline Helm chart and GitHub Actions runner architectural refactoring. An earlier comprehensive PR (#924) failed and was closed due to oversized complexity.
- **Fit Evaluation**: Low fit. Primarily Kubernetes, Helm, and GitHub Actions CI runner infrastructure rather than Python application logic or FastAPI backend test fixtures.


