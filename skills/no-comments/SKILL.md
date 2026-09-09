---
name: no-comments
description: "Audit a requested scope for removable comments and suppressions, fix accepted findings, and offer safe encodings for real constraints. Run only when the user explicitly asks for it."
disable-model-invocation: true
---

# No comments

Find comments and suppressions that can be removed, then make the smallest safe
in-scope fix. A reviewer may help, but this skill must work without delegation.

## Scope

Use the caller's explicit files or diff. Otherwise resolve the repository root
and review its current tracked diff against the checked-out base branch, falling
back to `main`. Include staged and unstaged changes. Do not touch untracked
files unless the caller includes them. Never widen the scope to make a finding
easier to fix.

## Steps

1. If the runtime provides a reviewer subagent, pass it the resolved scope and
   inspect its report. Otherwise review the scope directly. The reviewer must
   report findings only; it must not edit application code.
2. Check every finding against the scoped diff and source. Reject scope escapes,
   edits outside the skill's task, exception-protected deletions, incorrect
   reasons, and flags aimed at intentional code. Audit scoped lint and
   TypeScript suppressions. Correctness and safety suppressions remain
   actionable. Trace ambiguous `IMPORTANT` or `do not remove` claims through
   the definition, callers, tests, and `git log -S` before deciding. Do not
   revert unrelated work or rerun a rejected report; record the disputed finding
   and why it was rejected.
3. Delete accepted dead comments and make trivial fixes directly. If a fix needs
   a new shape, write one concise sketch covering its types, boundaries, and
   names, then implement it in the same run.
4. Implement the smallest root-cause fix in scope. Remove named workarounds. If
   the root cause is out of scope, make the smallest safe in-scope change and
   report the root cause as open. Do not widen the scope.
5. Treat comments saying `do not remove`, `do not change wording`, or `talk to X
   before changing` as constraints. Keep them until the caller approves an
   encoding or has pre-approved unattended work. Offer the cheapest type,
   runtime, test, or CI check. After approval, encode the constraint and delete
   the comment. Without approval, preserve it and report the open constraint.
6. Run the smallest relevant formatter, linter, test, or type check after edits.
   If no check applies, say so.
7. Report the scope, deleted comments, preserved comments and reasons, rejected
   findings, shape sketch, fixes, checks, encodings, and open work.
