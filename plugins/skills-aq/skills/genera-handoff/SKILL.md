---
name: genera-handoff
description: Use when the user asks to generate a handoff of the current session so another agent with zero context can pick it up - produces a task-specific document plus a prompt to paste into the next session
argument-hint: "focus or task for the next session (optional)"
disable-model-invocation: true
---

# Generate a session handoff

Two deliverables, always both: (1) a document on disk that another agent with zero prior context can
read and resume the task from without asking anyone anything, and (2) a short prompt, ready to copy and
paste, that kicks off that next session.

## Language of the handoff document

Write the handoff document itself in whichever language the current session has actually been
conducted in with the user, inferred from the conversation. Do not default to any fixed language and do
not ask - just match what's already there. These skill instructions stay in English regardless of the
document's language.

## Before writing

1. If the user passed an argument when invoking the skill, treat it as the focus of the next session:
   what that session should do first, and tailor the document to that.
2. Load the `docs-que-sobreviven` skill (Skill tool) before drafting. This document is exactly the kind
   of artifact that skill audits, and its rules (no session deixis, provenance for every claim, pending
   items with an experiment, absolute pointers) apply in full here. Don't copy them, invoke the skill.
3. Detect the active repo (`git rev-parse --show-toplevel` in the current working directory):
   - If there is a repo: the document goes in `<repo>/docs/handoffs/YYYY-MM-DD-<task-slug>.md`, with
     today's date in absolute form (`2026-08-05`, never "today").
   - If there is NO repo (a loose session in a directory without git): use the current session's
     scratchpad, and say so explicitly in the chat - that path is ephemeral and no other session will
     see it unless someone moves it.
4. Don't commit the file on your own. It's delivered written; the commit is whoever asked for it's call.

## Document contents (task-specific, not a generic template)

No empty "just in case" sections. Only what this specific task needs to be resumed, at minimum:

1. **Title + absolute date.**
2. **Hard rules that apply**, if any is critical to avoid breaking something on resume (which account to
   push with, correct repo/branch, read-only mode, etc.) - only if relevant, above everything else.
3. **What was asked**, in the requester's own words, not paraphrased.
4. **Actionable current state**: what's done, what's missing, and the concrete next action (the next
   command, file, or decision point, not "keep exploring").
5. **Necessary context**: repos, absolute paths, branches, commands already run and their result,
   relevant tools/MCPs for this task.
6. **Decisions made and their rationale**, one line each, without narrating how they were reached.
7. **Pending items**, each with its experiment or next step, estimated cost if applicable, and status
   (`not run`, `partial`).
8. **Absolute pointers**: files (`file.py:123`), commits, PRs, branches, relevant memories
   (`[[memory-name]]` where applicable).
9. **Suggested skills** for the next session, with the reason for each.
10. Redact any secret, token, or credential before saving.

Don't duplicate content that already lives in another artifact (spec, plan, PR, commit, issue): point to
it by path or URL, don't copy it.

## When done writing

Run the document through the `docs-que-sobreviven` smoke test: can someone who was never in this session
understand what the document answers and execute the first useful action without asking anyone anything?
If the answer is no, keep editing before handing it off.

## Delivering

In the chat (not just the file), deliver two things:

1. The absolute path of the saved document.
2. A ready-to-paste prompt block for the next session. That prompt:
   - Is self-contained: it doesn't assume the next session saw this conversation.
   - Explicitly states the handoff's absolute path and asks to read it first.
   - Summarizes the task in 1-2 lines, so whoever pastes it knows what they're pasting without opening
     the file.
   - Doesn't repeat the document's full contents: the document is the source, the prompt is the pointer.

Reference shape for that block (adapt to the case, don't copy literally):

```
Resume this task by first reading <absolute path to the handoff>. One-line context: <what's being
done>. That document has the state, the pending items, and the next steps - don't ask for context
that's already there.
```
