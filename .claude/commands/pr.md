---
name: pr
description: Create a pull request against a user-chosen base branch with a concise description and list of changed files
argument-hint: "[base-branch]"
allowed-tools: Bash(git *) Bash(gh *)
---

Create a pull request for the current branch.

## Step 1: Determine the base branch

If `$ARGUMENTS` is provided, use it as the base branch.

Otherwise, ask the user which branch they want to merge into using AskUserQuestion. Show them the available remote branches first by running `git branch -r --sort=-committerdate | head -10` so they can see recent options. Default suggestion should be `main`.

## Step 2: Gather context

Run these commands in parallel to understand what's changed:

- `git rev-parse --abbrev-ref HEAD` — current branch name
- `git status` — uncommitted changes (warn the user if any exist)
- `git log <base>..HEAD --oneline` — commits in this branch
- `git diff <base>...HEAD --stat` — files changed with line counts
- `git diff <base>...HEAD --name-only` — just file paths

## Step 3: Push if needed

Check if the current branch has an upstream. If not, push with `git push -u origin <branch>`.

## Step 4: Draft the PR

Write:
- **Title:** short (under 70 chars), imperative mood, follows project convention (`feat:`, `fix:`, `refactor:`, etc. — match recent commit style)
- **Body** with two sections:
  1. **Summary** — 2-4 bullet points describing what changed and why. Focus on the "why", not line-by-line "what". Derive this from the commits and diff, not just the latest commit.
  2. **Files changed** — bulleted list of all changed file paths grouped logically (e.g., by feature or layer)

Do NOT add a test plan section, Co-Authored-By line, or "Generated with Claude Code" footer.

## Step 5: Create the PR

Run `gh pr create --base <base-branch> --title "..." --body "..."` using a HEREDOC for the body to preserve formatting:

```
gh pr create --base <base> --title "<title>" --body "$(cat <<'EOF'
## Summary
- <point 1>
- <point 2>

## Files changed
- `path/to/file1.ts`
- `path/to/file2.tsx`
EOF
)"
```

## Step 6: Return the PR URL

Print the URL returned by `gh pr create` so the user can open it.

## Safety notes

- Never force-push
- Never use `--no-verify` to skip hooks
- If there are uncommitted changes, warn the user and ask whether to include them or stop
- If the branch has no commits ahead of base, abort with a clear message
