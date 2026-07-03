---
name: gitshift
description: >-
  GitShift routes routine git and GitHub chores — committing, pulling, pushing,
  and status/diff inspection — to a cheap Haiku subagent instead of running them
  on the main (expensive) model. Use this skill WHENEVER the user asks to commit,
  stage and commit, pull, fetch, push, sync with the remote, or check
  status/diff/log, even if they don't mention models, cost, or "Haiku." The
  point is to keep an Opus/Sonnet session from paying premium rates for
  mechanical git work. Do NOT use it for resolving merge conflicts by hand,
  interactive rebases, rewriting history, or anything that needs real judgment
  about code — those stay on the main model.
---

# GitShift

## Why this skill exists

Git commit/pull/push/status are mechanical, low-reasoning tasks. Running them on
a top-tier model (Opus, Sonnet) wastes money — a `git commit` doesn't need a
frontier model to write a one-line message. Haiku is the cheapest model
available and handles these chores perfectly well. GitShift *shifts* that work
down to Haiku and hands control straight back.

A skill can't change the model running *this* session. But it can hand the work
to a **subagent** and pin that subagent to Haiku via the Agent tool's `model`
parameter. Your main session stays on whatever model you're using; the git chore
runs cheaply in the background and reports back. That's the whole idea: shift
down, then continue.

Because only the *subagent* runs on Haiku, there is nothing to "switch back."
The moment the subagent returns, control is already back on the user's original
model — same model, same effort/settings — with no manual step and no chance of
getting stranded on the cheap model. Only ever set `model: "haiku"` on the
delegated subagent; never change the main session's model.

## Make the shift visible

The model badge in the user's interface will NOT change — it always shows the
main session's model, and that never moves. Since the Haiku work happens inside
a subagent, the user has no other way to see that the shift occurred. So make it
explicit in the chat, every time:

1. **Before delegating**, post a short line like:
   `Shifting this <commit/pull/push/status> to a Haiku subagent to save cost…`
2. **After the subagent returns**, confirm with a line like:
   `Done on Haiku — back on your <model> now.` followed by the result.

This is the only signal the user gets, so don't skip it. Keep it to one line on
each side — the goal is confirmation, not noise. If you know the user's current
model name, use it in the "back on your …" line; if not, just say "back on your
main model."

## When to shift the work to Haiku

Delegate to a Haiku subagent when the user wants to:

- **Commit** — stage changes and create a commit (write the message from the diff).
- **Pull / fetch** — pull or fetch + merge from the remote.
- **Push** — push commits to the remote.
- **Inspect** — read-only `git status`, `git diff`, `git log`, branch listing.

Keep these on the **main** model (do NOT delegate): resolving merge conflicts,
interactive rebase, history rewriting (`filter-branch`, force-push cleanups),
or anything where a wrong git call could lose work. If Haiku hits a conflict or
anything ambiguous mid-task, it should stop and report back rather than guess.

## How to delegate

Spawn one subagent with the Agent tool, setting `model: "haiku"`. Give it a
tight, self-contained brief so it doesn't need to ask follow-ups. Include the
repo path, exactly which git operation to run, and any message or branch the
user specified.

Example brief for a commit:

```
Repo: <absolute path to the repo>
Task: Stage all changes and commit them.
- Run `git -C <repo> status` and `git -C <repo> diff --staged` (after staging) to see what changed.
- Stage with `git -C <repo> add -A` unless the user named specific files.
- Write a concise commit message: <style — e.g. Conventional Commits, or "match the repo's recent history" if unspecified>.
- Commit, then report: the message used, files committed, and the resulting commit hash.
- If anything is ambiguous or a conflict/error appears, STOP and report it — do not force or guess.
```

Adapt the same shape for pull/push/status. For pull/push, tell Haiku which
remote and branch (default to the current branch's upstream), and to report 