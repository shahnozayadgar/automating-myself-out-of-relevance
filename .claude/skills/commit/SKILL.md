---
name: commit
description: Commit staged or unstaged changes to git with a well-formatted commit message following this project's conventions
---

# Commit Changes Skill

You are helping commit changes in the neoclaw-demo project. Follow these steps carefully:

## Process

1. **Check status**: Run `git status` to see what files have changed
2. **Review changes**: Run `git diff` to see the actual changes (both staged and unstaged)
3. **Stage files**: Add relevant files with `git add <files>` if not already staged
4. **Analyze changes**: Review all changes and determine:
   - Type of change: feature, enhancement, fix, refactor, ui, docs, config, etc.
   - Scope: which part of the app is affected (chat, plugins, ui, data, hooks, app)
   - Impact: what the changes accomplish

5. **Create commit message**: Write a clear, descriptive commit message following this format:

```
<type>(<scope>): <short summary>

<optional detailed description if the change is complex>

```

### Commit Message Guidelines

**Type prefixes:**
- `feat`: New feature or plugin flow
- `ui`: UI/styling changes, design system updates
- `fix`: Bug fixes
- `refactor`: Code restructuring without changing behavior
- `perf`: Performance improvements
- `docs`: Documentation only
- `config`: Configuration files (eslint, vite, package.json)
- `data`: Mock data or script updates
- `chore`: Maintenance tasks

**Scope examples:**
- `chat`: Chat components (message bubbles, input, typing indicator)
- `plugins`: Plugin components (ExcelQA, FormFill, SlideGen, Transcription)
- `ui`: Reusable UI components (buttons, cards, modals)
- `data`: Mock data and conversation scripts
- `hooks`: Custom React hooks
- `app`: App-level changes
- `*`: Multiple areas affected

**Summary format:**
- Start with lowercase
- No period at the end
- Imperative mood ("add feature" not "adds feature" or "added feature")
- Max 72 characters
- Be specific about what changed and why

### Examples

```
feat(plugins): add excel Q&A plugin with mock data flow

feat(chat): implement typing animation with step reveal

ui(components): add primary and secondary button variants

fix(plugins): correct slide generation progress animation

refactor(hooks): extract typing logic into useTypingAnimation

data(excel): add mock spreadsheet Q&A conversation script
```

6. **Commit**: Execute the commit using a heredoc for proper formatting:

```bash
git commit -m "$(cat <<'EOF'
<your commit message here>
EOF
)"
```

7. **Verify**: Run `git log -1` to confirm the commit was created correctly

8. **Show commit details and ask about push**: After the commit is successful, present the commit details clearly before asking about push:
   - Display the commit message from `git log -1 --pretty=format:"%s%n%b"`
   - List the files that were committed using `git show --name-status --pretty="" HEAD`
   - Use the AskUserQuestion tool to ask if the user wants to push to remote:
     - Question: "Would you like to push this commit to the remote repository?"
     - Options:
       - "Yes, push now" - Execute `git push`
       - "No, keep it local" - Skip push and finish

9. **Push (if confirmed)**: If user confirms, execute `git push` and show the result

## Important Notes

- Do NOT commit if there are no changes
- ALWAYS ask for confirmation before pushing using AskUserQuestion tool
- Do NOT skip linting - run `npm run lint` first if making code changes
- Do NOT commit files with secrets or sensitive data
- Ensure commit messages accurately reflect the changes made
- Keep the summary line concise but descriptive
- After pushing, confirm the push was successful by showing the output

## Project Context

This is a frontend demo app simulating an AI chatbot with plugin capabilities. The codebase uses:
- React 19 with Vite 8
- Plain JSX (no TypeScript)
- Tailwind CSS 4
- Mock data flows (no real backend)
- Component structure: chat/, plugins/, ui/, data/, hooks/

All commits should reflect the demo/prototype nature of the project.
