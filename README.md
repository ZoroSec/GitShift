# GitShift

A Claude skill that **shifts** routine git chores — **commit, pull, push, and status/diff** — onto a cheap **Haiku** subagent instead of running them on your main (expensive) model.

## Why

`git commit` and `git pull` are mechanical tasks. They don't need a frontier model like Opus or Sonnet to do well. GitShift keeps your premium session from paying premium rates for one-line commit messages and routine syncs, by handing that work to Haiku (the cheapest model) and returning control to your original model the moment it's done.

There's no manual model switching: only the *subagent* runs on Haiku. Your main session never leaves your chosen model, so there's nothing to "switch back" — it's automatic and there's no risk of getting stranded on the cheap model.

## What it does

When you ask to:

- **Commit** — stages changes and writes a message from the diff
- **Pull / fetch** — syncs from the remote
- **Push** — pushes commits upstream
- **Inspect** — read-only `git status`, `git diff`, `git log`

...GitShift delegates the work to a Haiku subagent, then reports the result.

It deliberately **stays out of the way** for anything needing real judgment — merge-conflict resolution, interactive rebases, history rewriting, or actual code editing. Those remain on your main model.

## Install

**As a plugin marketplace (recommended):** in Claude, go to **Settings → Plugins → Add → Add marketplace** and enter:

```
ZoroSec/GitShift
```

Then install the **gitshift** plugin from the marketplace list.

**As a standalone skill:** download [`gitshift.skill`](https://github.com/ZoroSec/GitShift/raw/main/gitshift.skill) and open it — click **Save skill** to install (if your organization allows skill creation).

**Manual:** clone the repo and copy the skill folder into your skills directory:

```bash
git clone https://github.com/ZoroSec/GitShift.git
cp -r GitShift/plugins/gitshift/skills/gitshift ~/.claude/skills/gitshift
```

## Usage

Just talk normally. Any of these will trigger it:

- "commit my changes"
- "push this up to github"
- "pull the latest from main"
- "what's changed in this repo?"

## How it works

A skill can't change the model running your current session. Instead, GitShift spawns a subagent via the Agent tool with `model: "haiku"`, gives it a tight brief (repo path, operation, commit style), and relays what comes back. See [`gitshift/SKILL.md`](./gitshift/SKILL.md) for the full instructions.

## License

MIT — see [LICENSE](./LICENSE).
