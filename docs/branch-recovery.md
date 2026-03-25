# Branch Recovery: `fix-missing-data`

## What happened

The remote branch `fix-missing-data` was deleted after it was merged into `Staging` via
**Pull Request #1 – "Fix missing data"** (merged 2026-03-25).

## How the branch was restored

The last commit that belonged to `fix-missing-data` was identified from the closed PR:

| Detail | Value |
|--------|-------|
| PR | [#1 – Fix missing data](https://github.com/kiran259-droid/advanced-data-engineering-snowflake/pull/1) |
| Head commit SHA | `b7bf921b2ed42c51c7b31ace98463556063079e9` |
| Commit message | `Fix missing data` |
| Merged into | `Staging` |

The branch was recreated locally at that commit and then pushed to the remote:

```bash
# 1. Make sure you have the full history (run once per clone)
git fetch --unshallow origin

# 2. Create the branch at the last-known commit
git branch fix-missing-data b7bf921b2ed42c51c7b31ace98463556063079e9

# 3. Push the branch to GitHub
git push -u origin fix-missing-data
```

## How to repeat this process in the future

If a branch is accidentally deleted again, follow these steps:

### Option A – Restore from a closed / merged Pull Request (easiest)

1. Go to the repository on GitHub.
2. Open **Pull requests → Closed**.
3. Open the PR whose head branch was the deleted branch.
4. At the bottom of the PR page, click **Restore branch** (GitHub keeps the commit
   reachable for at least 90 days after a PR is closed).

### Option B – Recreate from a known commit SHA

If the "Restore branch" button is no longer available (or was never there), find the last
commit SHA from the PR details page or the API, then run:

```bash
# Replace <sha> with the commit SHA and <branch-name> with the branch you want to restore
git fetch --unshallow origin          # ensure the commit is available locally
git branch <branch-name> <sha>
git push -u origin <branch-name>
```

### Option C – Recover from a local clone

If a teammate still has the branch in their local clone, they can push it back:

```bash
git push origin <local-branch-name>:<branch-name>
```

## Prevention tips

- Enable **branch protection rules** on important branches to require a PR before deletion.
- Avoid ticking "Delete branch after merge" unless the branch is truly disposable.
- For long-lived feature branches, consider creating a tag at the tip before merging:
  `git tag archive/<branch-name> <sha> && git push origin archive/<branch-name>`
