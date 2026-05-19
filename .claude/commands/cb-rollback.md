Rollback the castlebridgedemoportfolio site by reverting the latest commit on main.

## Environment notes
- `gh` CLI is available in this environment — prefer it over raw `curl` for GitHub operations.

## Steps

1. Get the latest commit on `main`:
   ```bash
   gh api repos/sharoncastillo690-wq/castlebridgedemoportfolio/commits/main
   ```
   Capture the `sha` and `commit.message` from the response.

2. Show the user the latest commit (sha + message) and ask:
   "This is the latest commit. Should I revert it? (yes/no)"

3. Wait for user confirmation.
   - If **no**: stop here and let the user know no changes were made.
   - If **yes**: continue to step 4.

4. Create a revert commit locally and push it to main:
   ```bash
   git fetch origin main
   git checkout main
   git pull origin main
   git revert --no-edit <SHA>
   git push origin main
   ```
   Use the SHA captured in step 1.

5. Confirm to the user that the revert commit was pushed and show the new commit hash.
