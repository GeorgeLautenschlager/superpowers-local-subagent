# Local Output Protocol

> Appended by the controller to the **unchanged** upstream implementer body at dispatch time.
> It is the only implementer-facing text this skill owns; the upstream
> `implementer-prompt.md` is never edited (see SKILL.md → Prompt Templates).

---

## Local Execution Protocol (read this last — it overrides anything above that assumes a chat)

You are running **non-interactively** as a local headless process (`pi -p`). There is no
human or controller watching a live conversation, and **nothing you say mid-run is read until
you exit**. This changes two things from the instructions above:

**1. You cannot ask questions and wait for an answer.**
The sections above invite you to "ask now" or "ask while you work." You can't — there is no one
to answer. If you reach a point where you would ask a question, or you are missing context you
cannot recover by reading files in the working directory, **do not guess**. Instead, stop and
finish your run reporting `STATUS: NEEDS_CONTEXT`, and list your questions explicitly under it.
The controller will re-dispatch you as a fresh run with the answers included.

**2. Your working directory is already the task's git worktree.**
Run `git`, tests, and file edits directly here. Commit your work on the current branch exactly
as the instructions above require — your commits are how the controller and reviewers see what
you did. Do not create branches or worktrees; you are already in the right one.

## Status line — exact format required

The controller parses your result by reading the **last `STATUS:` line** of your output. The
**final line of your entire reply** must be exactly one of these four, with nothing after it:

```
STATUS: DONE
STATUS: DONE_WITH_CONCERNS
STATUS: NEEDS_CONTEXT
STATUS: BLOCKED
```

- Emit it as a bare line at the very end — not inside a sentence, code block, or heading.
- Put your prose report (what you built, tests run, files changed, concerns/questions) **above**
  the status line, following the Report Format above.
- Exactly one status line at the end. If you wrote earlier drafts of a status line, the last one
  wins, so make sure the genuine result is last.

Mapping (same meaning as upstream):
- `DONE` — implemented, tested, committed, self-reviewed clean.
- `DONE_WITH_CONCERNS` — committed, but you have doubts; describe them above the line.
- `NEEDS_CONTEXT` — you are missing information; list the questions above the line.
- `BLOCKED` — you cannot complete this task; describe what you tried and what's needed.
