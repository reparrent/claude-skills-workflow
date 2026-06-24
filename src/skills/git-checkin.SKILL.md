---
name: git-checkin
description: Commit and push all changes to the current branch with a well-crafted commit message. This skill reads the project status report to understand what was accomplished, auto-detects file changes, generates conventional-format commit messages (feat, fix, test, docs, refactor), invites user approval, and pushes to the tracked branch. Use this after project-status to lock in your work. Handles sensitive file detection and git-ignore warnings.
compatibility: Bash, Read
---

# Git Check-in Workflow

Commit and push all changes to your current branch with well-crafted conventional commit messages. This skill reads your project status report to understand what you've built, detects file changes, and guides you through a safe, thoughtful commit process.

## When to use this skill

- **After project-status:** Use the status report to inform what and how to commit
- **User says:** "commit", "push", "check in", "finalize", "save changes"
- **End of session:** Natural point to commit all work
- **Feature complete:** Ready to share code with team
- **Bug fixed:** Resolved issue ready for review

## What the skill does

1. **Read project status** — If `PROJECT_STATUS.md` exists, use it to understand what was accomplished
2. **Detect changes** — Runs `git status` to see staged, unstaged, and untracked files
3. **Warn about sensitive files** — Flags `.env`, `credentials.*`, `secrets.*`, `*.key`, etc.
4. **Suggest .gitignore additions** — Warns if sensitive files would be committed
5. **Generate commit message** — Based on status report accomplishments, create a conventional commit
6. **Get user approval** — Show the files, the message, and ask for confirmation
7. **Commit & push** — Stage files, commit with approved message, push to current branch

## Commit message format

Follow [Conventional Commits](https://www.conventionalcommits.org/) style:

```
<type>(<scope>): <subject>

<body (optional, for complex changes)>
```

**Types:**
- `feat` — New feature
- `fix` — Bug fix
- `test` — Add or update tests
- `docs` — Documentation changes
- `refactor` — Code refactor (no feature/fix)
- `perf` — Performance improvement
- `chore` — Build, deps, tooling (no code change)

**Examples:**
- `feat(auth): add JWT token validation`
- `fix(api): handle null user response gracefully`
- `test: add integration tests for payment flow`
- `docs: update README with setup instructions`

## Instructions

1. **Read project status (if available):**
   - If `PROJECT_STATUS.md` exists, read it to understand what was accomplished
   - Use accomplishments to inform commit scope and message
   - Example: If status says "Implemented JWT auth with rate limiting", the commit should reflect that scope

2. **Gather file changes:**
   ```bash
   git status --porcelain          # See all changes
   git branch --show-current        # Current branch
   git diff --stat                  # Summary of changes
   git diff (key files)             # Read diffs to understand scope
   ```

3. **Detect sensitive files:**
   - Check for `.env`, `credentials.*`, `secrets.*`, `*.pem`, `*.key`, `config/secrets.*`
   - If found, WARN the user: "⚠️ Sensitive file detected: [file]. This will be committed. Add to .gitignore? (y/n)"
   - If user says yes, create/update `.gitignore` and stage it, then skip the sensitive file

4. **Generate commit message:**
   - Use the project status to understand what was accomplished
   - Choose the right type (feat, fix, test, etc.) based on status and changes
   - Keep the subject line under 50 characters, clear and descriptive
   - If status includes multiple accomplishments, consider splitting into logical commits or writing a body explaining scope
   - Example: Status says "Implemented JWT validation, added rate limiting, wrote tests" → could be one `feat(auth)` with a body, or split into multiple commits

5. **Show user what will be committed:**
   ```
   📋 Files to commit:
   - path/to/file1.js
   - path/to/file2.py
   
   📄 Based on status: [accomplishment from PROJECT_STATUS.md]
   
   💬 Commit message:
   feat(auth): add JWT token validation
   
   ✅ Ready to commit? (y/n) [continue only on approval]
   ```

6. **Commit and push:**
   ```bash
   git add .
   git commit -m "$(approved message)"
   git push origin $(current-branch)
   ```

7. **Report success:**
   - Show the commit hash and branch
   - Confirm the push completed
   - Example: `✅ Committed to main: abc1234 - feat(auth): add JWT token validation`

## Edge cases

- **No project status available:** Infer from git diffs and files changed. Ask user: "What was the main accomplishment? (e.g., 'added auth', 'fixed bug in API')"
- **Nothing to commit:** If `git status` shows no changes, inform the user: "No changes to commit. Working tree is clean."
- **Merge conflict:** If push fails due to conflicts, advise user to pull and resolve before retrying
- **Detached HEAD:** If on a detached HEAD, warn and ask if they meant to work on a branch
- **Untracked files:** Show untracked files in the summary. If user hasn't staged them, ask: "Include these untracked files? (y/n)"
- **Large commit:** If many files or large diffs, offer to split: "This is a large commit (N files). Want to review/split before committing?"

## Pre-commit checks (informational, not blocking)

Before committing, run a quick check:
- If code files changed but no test files modified: "You modified code but didn't add/modify tests. Still commit? (y/n)"

This is advisory only—user can override.

## Safety

- **Never force-push** unless explicitly requested by user
- **Always show the user what will be committed before proceeding**
- **Never auto-commit sensitive files** — always warn and ask
- **Always confirm branch name** — no surprises when pushing
- **Respect project status** — use it to inform message quality and scope
