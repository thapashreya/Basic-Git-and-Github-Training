# Lab 2 — Undoing Changes

[← Back to index](README.md)

Git has four different "undo" commands and picking the wrong one is how people lose work. Use this table to choose:

| Your situation | Command | What survives |
|---|---|---|
| Edited a file, not committed yet, want the old version back | `git restore <file>` | Your edits are **gone** |
| Bad commit already pushed to GitHub / shared with teammates | `git revert HEAD` | Everything — history is kept, a new commit undoes it |
| Committed too early, want to fix the message or add a file | `git reset --soft HEAD~1` | Your changes, still staged |
| Commit is garbage and nobody else has it | `git reset --hard HEAD~1` | Nothing — commit **and** changes are gone |

> **The one rule:** `revert` is safe on shared branches, `reset` is not. `reset` rewrites history — if you've already pushed, your teammates' repos break. Reset only commits that live on your machine alone.

## Step 8 — Discard Unsaved Changes (restore)

> **Use it when:** you were experimenting in a file, it made things worse, and you never committed it. You want the file back exactly as it was at the last commit.
> **Careful:** those edits are not stored anywhere — once restored, they cannot be recovered. If you might want them later, use `git stash` (Step 22) instead.

In VSCode — open `hello.txt`, add a bad line:

```
Hello Git
I am learning Git
Git is a version control system
this is a mistake
```

Save — `Ctrl + S`

Terminal — discard that change:

```bash
git restore hello.txt
```

Now open `hello.txt` in VSCode — the bad line is gone ✅

## Step 9 — Undo Last Commit (revert)

> **Use it when:** the bad commit is **already pushed** to GitHub, or on a branch other people use. `revert` doesn't delete anything — it adds a new commit that reverses the old one, so nobody's history breaks.
> **Real example:** you pushed a change that broke the login page. Production is down. `git revert HEAD && git push` puts it back within seconds, and the record of what happened stays in the log.

In VSCode — open `hello.txt`, add a line:

```
Hello Git
I am learning Git
Git is a version control system
this line should not have been committed
```

Save — `Ctrl + S`

Terminal:

```bash
git add .
git commit -m "bad commit"
git log --oneline         # see bad commit at top
git revert HEAD           # creates a new undo commit
git log --oneline         # bad commit still there but neutralized
```

## Step 10 — Reset Soft (reset --soft)

> **Use it when:** the commit itself was premature but the work is fine — you forgot a file, typo'd the message, or want to combine two commits into one. `--soft` deletes the commit and leaves your changes staged, ready to re-commit properly.
> **Real example:** you commit as `"fix"`, then realise you left out `config.js`. `git reset --soft HEAD~1`, add the missing file, commit again with a proper message.
> **Only if:** you have not pushed that commit yet.

In VSCode — open `hello.txt`, add a line:

```
Hello Git
I am learning Git
Git is a version control system
accidental line
```

Save — `Ctrl + S`

Terminal:

```bash
git add .
git commit -m "accidental commit"
git log --oneline             # see it at top

git reset --soft HEAD~1       # commit gone, changes still staged
git status                    # file still green (staged)
git log --oneline             # commit is gone ✅
```

## Step 11 — Reset Hard (reset --hard)

> **Use it when:** you want the commit **and** the work in it completely gone — a failed experiment, debug code you committed by mistake, a branch you want back to a clean state.
> **This is the destructive one.** It deletes your files' changes with no undo. Before running it, ask two questions: *Have I pushed this?* (if yes → use `revert`) and *Do I want any of this work?* (if maybe → use `git stash` or `reset --soft`).

In VSCode — open `hello.txt`, add a line:

```
Hello Git
I am learning Git
Git is a version control system
delete everything including this
```

Save — `Ctrl + S`

Terminal:

```bash
git add .
git commit -m "commit to destroy"
git log --oneline

git reset --hard HEAD~1       # commit gone + file changes gone
git log --oneline             # commit removed ✅
```

Open `hello.txt` in VSCode — the line is completely gone ✅

---
Next: [Lab 3 — GitHub Basics](Lab3-GitHub-Basics.md)


shreya 