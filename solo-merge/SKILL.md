---
name: solo-merge
description: Finish a Git worktree by integrating its branch, stopping worktree-owned processes, and cleaning up the worktree and branch.
allow_implicit_invocation: false
---

# Solo merge

Run only when the user explicitly invokes this skill or asks to finish and clean up the current worktree. Inspect the repository's documented integration workflow and follow it: merge into the target branch or create/update a PR as appropriate. Do not guess when the workflow is unclear.

## Workflow

1. Identify the current worktree, branch, target branch, and any associated PR. Inspect status, commits, diffs, and repository guidance. Preserve uncommitted work; do not discard it or remove a worktree containing changes unless the user explicitly asks.
2. Integrate the branch using the project's workflow. Run required checks. Push or update a PR only when the user asked for that workflow or repository guidance requires it. Never force-push or rewrite shared history unless explicitly requested.
3. Before stopping processes, establish that each process belongs to this worktree. Check process command lines and working directories, including child processes and tools such as portless, dev servers, and Playwright. Match the canonical worktree path or an identifiable worktree-specific port/session. Do not kill a process based only on its name or shared port. If ownership is uncertain, leave it running and report it.
4. Once integration succeeds and the worktree is safe to remove, stop only the confirmed worktree-owned processes. Verify they exited.
5. Remove the worktree with `git worktree remove <path>` and delete its local branch with `git branch -d <branch>`. Do not use force options. If Git refuses because there are changes, conflicts, or the branch is not safely merged, stop and explain what remains.
6. Archive or close the corresponding Codex session only if a supported session control is available and the session is clearly associated with this worktree. Otherwise report that manual cleanup may be needed.
7. Verify the final worktree list, branch state, integration result, and any remaining processes. Summarize the merge or PR, checks, cleanup completed, and anything left for the user.

Never terminate ambiguous processes, remove a worktree with uncommitted changes, or delete an unmerged branch just to complete cleanup.
