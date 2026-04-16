# Daily Activity Report

Generate a summary of today's GitHub activity for user **shahnozayadgar**.

## Process

1. **Get today's date** using `date +%Y-%m-%d` and also compute the start-of-day ISO timestamp (e.g. `2026-04-16T00:00:00Z`).

2. **Fetch commits across all repos** using:
   ```bash
   gh search commits --author=shahnozayadgar --committer-date=today --json repository,sha,commit --limit 100
   ```

3. **Fetch PRs created or updated today** using:
   ```bash
   gh search prs --author=shahnozayadgar --updated=today --json repository,title,number,state,url,updatedAt --limit 50
   ```

4. **Fetch PR reviews done today** using:
   ```bash
   gh search prs --reviewed-by=shahnozayadgar --updated=today --json repository,title,number,url --limit 50
   ```

5. **Fetch issues created or commented on today** using:
   ```bash
   gh search issues --author=shahnozayadgar --updated=today --json repository,title,number,state,url --limit 50
   ```

6. **Group by project/repository** and for each repo:
   - List commits with short messages
   - List PRs opened, merged, or updated (with status)
   - List issues touched
   - List reviews given

7. **Write the report** in this format:

```markdown
# Daily Report — {date}

## {org/repo-name}
### Commits
- `abc1234` — commit message here
- `def5678` — commit message here

### Pull Requests
- #12 **PR title** (merged/open/closed) — url

### Reviews
- #15 **PR title** — reviewed

### Issues
- #8 **Issue title** (open/closed)

## {org/another-repo}
...

---
**Summary:** {1-2 sentence overview of the day — what was the main focus, what shipped}
```

8. **Output the report** directly as text. Do not save to a file unless the user asks.

## Notes
- If there is no activity for a section (e.g. no issues), skip that section entirely
- If there is no activity at all, say "No GitHub activity found for today."
- Keep commit messages concise — truncate at 80 chars if needed
- The summary at the bottom should highlight the main themes (e.g. "Focused on SSE integration for neoclaw frontend, shipped 3 PRs")
