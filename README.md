# GitShift

**Route routine Git operations to Claude Haiku automatically — keep your main model focused on high-value reasoning.**

GitShift is a Claude skill that offloads repetitive Git workflows like **commit**, **pull**, **push**, **status**, and **diff** to a low-cost **Haiku subagent** instead of consuming expensive tokens on your primary model.

Your main session stays on its original model (**Opus, Sonnet, Fable, or any future model**) while GitShift temporarily handles mechanical repository tasks through a cheaper subagent.

No manual switching.
No context loss.
No workflow interruption.

---

## Why GitShift?

Most Git operations are operational, not cognitive.

Using premium models for:

* Writing commit messages
* Pulling updates
* Pushing changes
* Checking diffs
* Reviewing status
* Reading logs

...is inefficient.

GitShift solves this by routing these lightweight operations to **Haiku**, reducing costs while keeping your premium model reserved for:

* architecture decisions
* deep debugging
* refactors
* system design
* complex reasoning

---

## How It Works

```text
Main Session (Opus / Sonnet / Fable)
                │
                │ User requests:
                │ "commit these changes"
                ▼
       GitShift detects Git intent
                │
                ▼
    Spawn Haiku subagent (cheap model)
                │
                ├─ Analyze diff
                ├─ Generate commit message
                ├─ Run git operation
                │
                ▼
      Return result to main session
                │
                ▼
 Original model continues uninterrupted
```

Important:
GitShift **does not switch your main model**.
It creates a temporary **Haiku subagent** for Git tasks, which means:

✅ Your context remains intact
✅ Your original effort settings remain unchanged
✅ No need to switch back manually

---

## Installation

### Option 1 — Plugin Marketplace (Recommended)

Inside Claude:
**Settings → Plugins → Add → Add marketplace**

Add:

```text
ZoroSec/GitShift
```

Install:

```text
gitshift
```

Fastest setup. No downloads required.

---

### Option 2 — Direct Git Repository Install

From the plugin marketplace, choose **Install from Git Repository** and use:

```text
https://github.com/ZoroSec/GitShift
```

Install and enable.

---

### Option 3 — Standalone Skill

Download [`gitshift.skill`](https://github.com/ZoroSec/GitShift/raw/master/gitshift.skill), open it, and click **Save Skill**.

(If your workspace allows custom skill creation.)

---

### Option 4 — Manual Install

```bash
git clone https://github.com/ZoroSec/GitShift.git
cp -r GitShift/plugins/gitshift/skills/gitshift ~/.claude/skills/gitshift
```

---

## What It Handles

GitShift automatically routes:

### Commit Operations

* `git commit`
* auto-stage changes
* generate commit messages from diffs
* conventional commits support

Examples:

```bash
commit my changes
commit with conventional format
commit these bug fixes
```

---

### Sync Operations

* `git pull`
* `git fetch`
* `git push`

Examples:

```bash
pull latest from main
push this to github
fetch latest updates
```

---

### Read-Only Inspections

* `git status`
* `git diff`
* `git log`

Examples:

```bash
what changed?
show my git status
show recent commits
```

---

## What It Avoids

GitShift intentionally avoids high-risk operations.
These stay on your main model:

❌ Merge conflict resolution
❌ Interactive rebases
❌ Force pushes
❌ History rewriting
❌ Cherry-picks
❌ Code refactors
❌ File modifications
❌ Branch strategy decisions

Reason: these require stronger reasoning and user oversight.

---

## Token Savings

> **Illustrative only.** The figures below use example per-model token prices to
> show the *shape* of the savings. Check current model pricing for exact
> numbers — the point is that routing mechanical Git work to Haiku is far
> cheaper than running it on a premium model.

GitShift can substantially reduce Git-operation costs.

Example commit task:

* Diff input → 2,500 tokens
* Commit output → 300 tokens

### On a premium model (example rates)

```text
Input:  2500 × $10 / 1M = $0.025
Output:  300 × $50 / 1M = $0.015
Total:                    $0.040
```

### On Haiku (example rates)

```text
Input:  2500 × $1 / 1M = $0.0025
Output:  300 × $5 / 1M = $0.0015
Total:                   $0.004
```

### Savings

```text
~$0.036 saved per Git action
```

| Usage                  | Savings     |
| ---------------------- | ----------- |
| 50 commits/day         | ~$1.8/day   |
| 500 commits/month      | ~$18/month  |
| Heavy GitOps workflows | Much higher |

The bigger win:
GitShift prevents overthinking.
A max-effort model can consume **5k–40k+ tokens** for a simple commit due to excessive reasoning.
Haiku keeps it lightweight.

---

## Usage

Use natural language. GitShift detects intent automatically.

Examples:

```text
commit my changes
push this to github
pull latest from origin
show git diff
what changed?
check git status
write a commit message for this diff
```

No special syntax required.

---

## Technical Details

GitShift uses Claude's **Agent tool** to spawn a subagent pinned to:

```text
model: "haiku"
```

The subagent receives:

* repo path
* git operation
* diff context
* commit style
* branch info

It executes the task and returns structured results. The main model remains untouched.

Implementation details: see [`plugins/gitshift/skills/gitshift/SKILL.md`](https://github.com/ZoroSec/GitShift/blob/master/plugins/gitshift/skills/gitshift/SKILL.md).

---

## Safety Principles

GitShift is designed around:

* Minimal-risk operations
* Read-first behavior
* No destructive Git commands
* No automatic force-pushes
* No hidden file edits
* User-first confirmations where needed

---

## Roadmap

Planned:

* Branch-aware commit suggestions
* PR title generation
* PR description generation
* Semantic version tagging
* Release notes generation
* Smart changelog summaries
* Commit linting
* Repo health checks

---

## Contributing

Contributions are welcome. Ideas:

* Better commit style templates
* More Git intent detection
* Improved subagent prompts
* Safer operation guards

Open an issue or submit a PR.

---

## License

MIT License. See [LICENSE](https://github.com/ZoroSec/GitShift/blob/master/LICENSE).
