# Daily Activity Report

Generate a summary of today's GitHub activity for user **shahnozayadgar**.

## Process

1. **Get today's date** using `date +%Y-%m-%d` and also compute the start-of-day ISO timestamp (e.g. `2026-04-16T00:00:00Z`).

2. **Fetch commits across all repos** using:
   ```bash
   gh search commits --author=shahnozayadgar --committer-date=today --json repository,sha,commit --limit 100
   ```

3. **Inspect each commit** — for every commit found, clone or navigate to the repo and run:
   ```bash
   gh api repos/{owner}/{repo}/commits/{sha} --jq '.files[].filename'
   ```
   and read the full diff with:
   ```bash
   gh api repos/{owner}/{repo}/commits/{sha} --jq '.files[] | "\(.filename) (+\(.additions)/-\(.deletions))"'
   ```
   Use the commit message, changed files, and diff stats to write a **2-sentence description** of what was done in that commit. The first sentence should state what changed. The second sentence should explain why or what it enables.

4. **Fetch PRs created or updated today** using:
   ```bash
   gh search prs --author=shahnozayadgar --updated=today --json repository,title,number,state,url,updatedAt --limit 50
   ```

5. **Fetch PR reviews done today** using:
   ```bash
   gh search prs --reviewed-by=shahnozayadgar --updated=today --json repository,title,number,url --limit 50
   ```

6. **Fetch issues created or commented on today** using:
   ```bash
   gh search issues --author=shahnozayadgar --updated=today --json repository,title,number,state,url --limit 50
   ```

7. **Group by project/repository** and for each repo:
   - List commits with 2-sentence descriptions
   - List PRs opened, merged, or updated (with status)
   - List issues touched
   - List reviews given

8. **Write the report** in this format:

```markdown
# Daily Report — {date}

## {org/repo-name}

### Commits
- `abc1234` — **commit message here**
  Changed X service to use Y instead of Z. This enables real-time streaming instead of polling for updates.

- `def5678` — **commit message here**
  Removed unused hooks and service functions for delete endpoints. These are commented out with TODOs for future re-enablement.

### Pull Requests
- #12 **PR title** (merged/open/closed) — url

### Reviews
- #15 **PR title** — reviewed

### Issues
- #8 **Issue title** (open/closed)

## {org/another-repo}
...

---

## Lessons Learned

Look across all of today's commits, PRs, issues, and reviews. Identify what was genuinely learned — not what was done, but what was *discovered* or *understood* for the first time. Write 2-4 bullet points covering:

- **Technical lessons** — new patterns, gotchas, or things that broke and why (e.g. "Learned that ESLint flags hook calls after early returns as conditional — hooks must always be called at the top of the component before any returns")
- **Architecture/design lessons** — decisions made about how to structure code, trade-offs understood (e.g. "Discovered that terminalEventType pattern simplifies SSE flows for single-entity responses by letting the promise resolve with the final event's data")
- **Process lessons** — workflow improvements, tooling discoveries, collaboration insights (e.g. "Found that commenting out planned-but-unused code with TODOs is cleaner than deleting it when the endpoint is documented but not yet needed")
- **Domain lessons** — things learned about the product, users, or business logic

Only include genuine insights. If nothing was truly learned (routine day), say "Routine day — no major new lessons." Do not fabricate lessons.

---

**Summary:** {1-2 sentence overview of the day — what was the main focus, what shipped}
```

9. **Output the report** directly as text. Do not save to a file unless the user asks.

## Notes
- If there is no activity for a section (e.g. no issues), skip that section entirely
- If there is no activity at all, say "No GitHub activity found for today."
- Commit descriptions must be exactly 2 sentences — no more, no less
- The lessons section should reflect real insights from the code changes, not generic advice
- The summary at the bottom should highlight the main themes (e.g. "Focused on SSE integration for neoclaw frontend, shipped 3 PRs")
