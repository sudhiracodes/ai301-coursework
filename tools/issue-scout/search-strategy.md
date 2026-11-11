# Search strategy: where your scout looks, and why

<!--
THIS IS THE PART YOU WRITE. The scout in SKILL.md executes whatever
queries and filters you define here. It ships empty on purpose: the
strategy is your judgment about where YOUR first contribution lives,
and a scan without authored queries is just someone else's defaults.

A filled strategy must contain:

1. At least one query in the Queries section. Each query is a real
   `gh search issues` invocation, and each should trace to a sentence
   of your fit profile in scope.md: if you cannot say which sentence
   produced a query, the query is probably not yours. Search breadth
   is the point at this stage: prefer generous limits and let the
   ranking filters do the narrowing.

2. At least one ranking filter in the Ranking filters section. A
   filter is a condition or an ordering over fields the search
   results already carry (last activity date, assignee presence,
   label set, comment count, repo). It must be checkable from the
   result fields alone: if deciding it would need opening the issue
   page, it belongs in your rubric, not here. Ranking is the free
   stage; keep it free. Make at least one filter an ordering, or
   state your tie-break: with drop conditions alone, the scout takes
   the top of whatever order the searches happened to return, and
   the graded candidates stop being traceable to you.

3. The top-N line, kept or deliberately changed.
-->

## Queries

# From: "I prefer well-bounded bug fixes, test fixture repairs, parser edge case handling, and documentation alignment tasks where the expected behavior and affected files are clearly specified."
gh search issues "test" --language python --label "good first issue" --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url

# From: "I have strong experience with Python, FastAPI backend services, pytest testing suites, and RAG/LLM application development."
gh search issues "fastapi" --language python --label "good first issue" --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url

# From: "I have strong experience with Python, FastAPI backend services, pytest testing suites, and RAG/LLM application development."
gh search issues "rag" --language python --label "good first issue" --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url

## Ranking filters

1. Drop any result with an assignee (an assigned issue already has someone working on it).
2. Drop any result with 3 or more comments (excess comments indicate ongoing debate or competition).
3. Drop any result not updated in the last 60 days (ensures recent maintainer attention).
4. Prefer fewer comments over more comments (0 comments > 1 comment > 2 comments).
5. Tie-break: sort by most recently updated first (`updatedAt` descending).

## How many to grade

Grade the top **5** ranked results per scan.
