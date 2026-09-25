---
name: solo-commit
description: Review repository changes, stage only the work that belongs in one focused commit, and write a concise Conventional Commit message.
allow_implicit_invocation: false
---

# Solo commit

Prepare and create one focused Git commit for the current task. Infer the intended commit scope from the user's request and the repository state. Keep the commit small, reviewable, and accurately described.

## Workflow

1. Inspect repository context before changing the index: run `git status --short --branch`, inspect the diff (including untracked files), and read relevant project guidance such as `AGENTS.md`, contribution docs, and existing commit conventions. Use `git diff --` and `git diff --cached --` to understand both unstaged and staged work. Do not assume all existing changes belong to this task.
2. Identify the task-related files. Preserve the user's existing staged changes and unrelated work. Do not reset, checkout, stash, clean, or otherwise discard changes. If a file mixes task-related and unrelated edits, stage only the relevant hunks with interactive staging or an equivalent patch. If the boundary is ambiguous, leave the uncertain changes unstaged and report them.
3. Inspect candidate files for credentials, tokens, private keys, local environment files, personal data, and machine-specific configuration before staging. Never stage secrets. If a likely secret is already staged, unstage it without modifying its working-tree contents, then tell the user. Do not print secret values in the report.
4. Decide whether generated or local files belong in the commit by checking project conventions and whether the change is required to build, run, or use the feature. Keep caches, dependencies, build outputs, logs, temporary files, editor state, and local machine settings out unless the repository explicitly tracks them for a clear project reason.
5. Update `.gitignore` only when the task or repository evidence shows a useful, reusable ignore rule is missing. Add the narrowest pattern that fits the project's conventions; do not add broad patterns that could hide source, fixtures, or deliverables. Do not ignore a file merely to avoid deciding whether it belongs in the commit.
6. Stage the selected paths or hunks explicitly. Avoid `git add .` and `git add -A` when the worktree contains unrelated or unclear changes. Review `git diff --cached` and `git status --short` after staging; verify the index contains only the intended changes and no secrets.
7. Write a short Conventional Commit subject: `<type>(<optional-scope>): <imperative summary>`. Choose the type and scope from the actual change (`feat`, `fix`, `docs`, `refactor`, `test`, `build`, `ci`, `chore`, or another established project convention). Use a lowercase, concise summary, usually under 72 characters. Describe the user-visible result or purpose, not the files edited. Add a body only when it clarifies meaningful context; avoid filler and redundant detail.
8. Create the commit only when the user asked for a commit or the active workflow clearly requires one. Run the repository's relevant checks first when appropriate, then commit the reviewed index. Do not amend, rebase, force-push, or rewrite existing history unless explicitly requested.
9. Confirm the resulting commit with `git status --short --branch` and `git show --stat --oneline --summary HEAD`. Report the commit hash and subject, checks run (if any), and any changes left unstaged or excluded, without exposing secret contents.

## Commit message examples

- `feat(auth): add passkey sign-in`
- `fix: preserve query parameters on redirect`
- `docs: clarify local development setup`
- `chore(deps): update vite`

If the worktree is clean, do not create an empty commit. If the requested scope cannot be separated safely, explain what is ambiguous and ask before staging or committing that portion.
