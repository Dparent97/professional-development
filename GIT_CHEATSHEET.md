# Git Cheatsheet

*The 20% of git that covers 90% of what I need*

## Daily Workflow

```bash
# Start of session - get latest
git pull origin main

# Check what's changed
git status

# Stage and commit
git add .
git commit -m "feat: add user authentication"

# Push to remote
git push origin main
```

## Branching (for bigger features)

```bash
# Create and switch to new branch
git checkout -b feature/add-notifications

# Do work, commit as usual...
git add .
git commit -m "feat: add email notifications"

# Push branch to remote
git push -u origin feature/add-notifications

# When done: switch back to main, merge
git checkout main
git pull origin main
git merge feature/add-notifications
git push origin main

# Clean up branch
git branch -d feature/add-notifications
git push origin --delete feature/add-notifications
```

## Checking History

```bash
# Recent commits (short)
git log --oneline -10

# What changed in a commit
git show <commit-hash>

# What's different between branches
git diff main..feature-branch
```

## Fixing Mistakes

```bash
# Undo last commit (keep changes staged)
git reset --soft HEAD~1

# Undo last commit (keep changes unstaged)
git reset HEAD~1

# Discard all local changes (CAREFUL)
git checkout -- .

# Abort a merge that has conflicts
git merge --abort
```

## Working with Remotes

```bash
# See what's on remote
git fetch --all

# See all branches (local and remote)
git branch -a

# Get a remote branch locally
git checkout -b local-name origin/remote-name

# Prune deleted remote branches
git fetch --prune
```

---

## Commit Message Convention

```
type: short description (max 72 chars)

Types:
- feat:     New feature
- fix:      Bug fix
- docs:     Documentation only
- refactor: Code change that doesn't fix bug or add feature
- test:     Adding tests
- chore:    Maintenance (deps, config, etc)

Examples:
- feat: add SMS notification system
- fix: resolve login timeout on slow connections
- docs: update README with deployment instructions
- refactor: extract validation into separate module
- chore: update dependencies to latest versions
```

---

## Common Scenarios

### "I made changes but want to switch branches"
```bash
git stash                    # Save changes temporarily
git checkout other-branch    # Switch
# ... do stuff ...
git checkout original-branch
git stash pop                # Restore changes
```

### "I committed to wrong branch"
```bash
git log --oneline -1         # Note the commit hash
git reset --hard HEAD~1      # Remove commit from current branch
git checkout correct-branch
git cherry-pick <hash>       # Apply commit here
```

### "I need to update my branch with main"
```bash
git checkout my-branch
git merge main               # or: git rebase main
# Resolve conflicts if any
git push origin my-branch
```

### "Remote has changes I don't have"
```bash
git pull origin main         # Fetch and merge
# If conflicts, resolve them, then:
git add .
git commit -m "merge: resolve conflicts with main"
git push
```

---

## What I Always Forget

| Task | Command |
|------|---------|
| See remote URLs | `git remote -v` |
| Change remote URL | `git remote set-url origin <new-url>` |
| See what branch I'm on | `git branch` (star = current) |
| Delete local branch | `git branch -d branch-name` |
| Delete remote branch | `git push origin --delete branch-name` |
| Rename branch | `git branch -m old-name new-name` |
| See commits not yet pushed | `git log origin/main..HEAD` |

---

*When in doubt: `git status` tells you what's happening, `git log --oneline -5` tells you what happened*
