---
name: git-pushing
description: Stage, commit, and push git changes with AI-generated, pirate-style conventional commit messages (via Claude Code CLI). Use when user wants to commit and push changes, mentions pushing to remote, or asks to save and push their work. Also activates when user says "push changes", "commit and push", "push this", "push to github", or similar git workflow requests.
---

# Git Push Workflow

Stage all changes, create a conventional commit with an AI-generated pirate-style message, and push to the remote branch.

## When to Use

Automatically activate when the user:
- Explicitly asks to push changes ("push this", "commit and push")
- Mentions saving work to remote ("save to github", "push to remote")
- Completes a feature and wants to share it
- Says phrases like "let's push this up" or "commit these changes"

## Workflow

**ALWAYS use the script** - do NOT use manual git commands.

**Default: run WITHOUT a message.** The script generates the commit message with AI:

```bash
bash .claude/skills/git-pushing/scripts/smart_commit.sh
```

Only pass a message when the user explicitly provides one:
```bash
bash .claude/skills/git-pushing/scripts/smart_commit.sh "feat: add feature"
```

### AI commit messages

When no message is given, the script pipes the staged diff (first 20,000 chars) to the Claude Code CLI (`claude -p`) and uses its reply as the commit message:
- Conventional Commits format: `type(scope): description`
- Type and scope stay normal English keywords; the description is written **like a pirate**
- Single line, under 72 characters
- Example: `feat(skill): summon Claude to pen pirate commit messages, arr`

If the `claude` CLI is missing or returns nothing, the script falls back to a heuristic message based on file paths (e.g. `docs(skill): update documentation`).

### What the script handles

- Staging all changes, including untracked files
- Commit message (AI pirate message, heuristic fallback, or the provided one)
- Claude footer
- Pushing to the branch's tracking remote (falls back to `origin`), with `-u` for new branches
