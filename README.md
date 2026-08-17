# T3 default-upstream PR association reproduction

This synthetic repository reproduces a T3 Code PR-association edge case without using any production repository or data.

The repository contains a merged pull request whose head branch is `main` and whose base is a maintenance branch. Separately, create a local feature branch that tracks `origin/main`:

```sh
git clone git@github.com:gsimone/t3-default-upstream-pr-repro.git
cd t3-default-upstream-pr-repro
git worktree add -b feature/local-worktree /tmp/t3-pr-repro-worktree origin/main
git -C /tmp/t3-pr-repro-worktree status --short --branch
```

Git records `origin/main` as the new feature branch's upstream. Before the T3 fix, PR resolution treated that upstream as the feature branch's published PR head, found the merged reverse PR for `main`, and reported it for `feature/local-worktree`. Once the thread became idle, the merged PR state caused the unrelated thread to auto-settle.

Expected behavior: an upstream default branch used as the starting point for a differently named local feature branch is a base relationship, not a PR association.
