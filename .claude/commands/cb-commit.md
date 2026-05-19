Examine the current directory for changes and execute a git commit with an AI-generated summary.

## Environment notes
- `gh` CLI is available in this environment — prefer it over raw `curl` for any GitHub operations.

## Steps

1. Run the following in parallel to gather context:
   - `git status` — see what files are staged/unstaged/untracked
   - `git diff` — see unstaged changes
   - `git diff --cached` — see staged changes
   - `git log --oneline -5` — see recent commit style for this repo

2. Analyze all changes (staged and unstaged). Stage everything relevant:
   ```bash
   git add -A
   ```
   Do NOT stage files that likely contain secrets (`.env`, credentials, etc.) — warn the user if any are present.

3. Generate a concise, accurate commit message based on the diff:
   - First line: imperative mood, ≤72 chars, summarizes the "what"
   - If needed, a blank line followed by 1–3 bullet points explaining the "why" or notable details
   - Do not reference the task, issue numbers, or ephemeral context

4. Show the user the proposed commit message and the list of files being committed. Ask:
   "Proceed with this commit? (yes / edit / no)"

5. Based on the response:
   - **yes**: run the commit using a HEREDOC to preserve formatting:
     ```bash
     git commit -m "$(cat <<'EOF'
     <generated message>

     Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
     EOF
     )"
     ```
     Confirm success and show the commit hash, then run `git push` and confirm it succeeded.
   - **edit**: ask the user for their preferred message, then commit with that message (still append the Co-Authored-By trailer), then push.
   - **no**: run `git reset HEAD` to unstage and abort. Inform the user no commit was made.
