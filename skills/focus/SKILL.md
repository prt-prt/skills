---
name: focus
description: Complete a clearly scoped request with the smallest sufficient action, tool use, verification, and response. Use only when the user explicitly invokes this skill.
disable-model-invocation: true
---

# Focus

Do the core ask and stop.

- Identify the smallest concrete outcome that satisfies the request.
- Prefer direct action over planning, broad exploration, delegation, or commentary.
- Read only files needed to locate and make the change. Do not survey adjacent code without evidence that it matters.
- Do not invoke optional review, audit, research, verification, or documentation skills unless the user asks for them.
- Make the narrowest viable edit. Preserve unrelated behavior and changes.
- Run the smallest check that provides reasonable confidence for the change. Skip redundant, broad, or unrelated checks. For a trivial low-risk edit, inspection of the diff can be enough.
- Stop when the requested outcome works. Do not add cleanup, refactors, enhancements, or speculative fixes.
- Keep progress updates rare and short. Give the result, the check performed, and any real limitation in the final response.

Explicit user instructions, repository rules, permissions, and safety requirements still apply. Do not skip investigation, validation, security, accessibility, or error handling that the task or risk level requires. If the task is ambiguous in a way that could materially change the result, ask one concise question instead of exploring every interpretation.
