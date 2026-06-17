# wt — Git Worktree Manager

Work on multiple PRs and branches in true parallel — each with its own environment, port, and terminal session.

---

## Get started in 3 steps

### Step 1 — Install

```bash
curl -fsSL https://raw.githubusercontent.com/KishanNanavath/wt-releases/main/install.sh | sh
```

### Step 2 — Install dependencies

```bash
brew install tmux gh
gh auth login
```

Check everything is ready:

```bash
wt doctor
```

All items should show `OK`. If anything is `MISSING`, run `wt doctor --install` and it will offer to install them for you.

### Step 3 — Initialise your project

Navigate to your project directory and run:

```bash
wt init
```

This provisions a wt-owned bare repo at `.wt/repo.git` — the foundation for all isolated worktrees. It works in three scenarios:

| Your directory    | What happens                                                   |
| ----------------- | -------------------------------------------------------------- |
| Existing git repo | Clones `--bare` into `.wt/repo.git`; your `.git` is untouched |
| Non-git directory | Seeds current files as an initial commit on `main`             |
| Empty directory   | Creates an empty initial commit on `main`                      |

Idempotent — safe to re-run.

---

## Your first worktree

```bash
wt new                   # interactive picker — choose a PR or branch
```

This does everything in one shot:

1. Fetches the branch from origin
2. Creates a git worktree in `.wt/workspaces/`
3. Copies `.env` from the project root (with a unique port assigned)
4. Detects your dev setup (nix / procfile / task / docker-compose / makefile) and auto-starts it
5. Opens a tmux session with a welcome banner in pane 1

You can also pass a branch name directly:

```bash
wt new feature/auth          # existing branch (local or remote)
wt new feature/new-idea      # doesn't exist? creates it from origin/main
```

---

## The daily workflow

```
wt new              → pick PR or branch → worktree + session created
wt attach           → jump into the tmux session
                      (ctrl+b d to detach, session keeps running)
wt status           → dashboard: branch, dirty, ahead/behind, session state
wt done             → safe teardown (warns about uncommitted/unpushed work)
```

---

## After a reboot

tmux sessions live in memory — a reboot kills them, but your worktrees survive on disk. Bring everything back at once:

```bash
wt revive-all         # recreates sessions for every worktree
wt revive-all --dry-run   # preview first
```
