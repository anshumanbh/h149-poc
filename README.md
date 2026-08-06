# h149-poc

Throwaway proof-of-concept repository for **H-149** — base-branch hook interpreter
hijack against `anthropics/claude-code-action`.

This repo simulates an *adopter* of `claude-code-action` that follows the guidance
added in [PR #1594](https://github.com/anthropics/claude-code-action/pull/1594)
(`docs/security.md`, section "Which files come from the base branch on pull requests"):

> Keep such commands self-contained: invoke the tool directly with a pinned version
> and pass its configuration on the command line (for example
> `bunx prettier@3.5.3 --no-config --write .` rather than `bun run format`).

The base-branch `.claude/settings.json` here is a byte-for-byte copy of the one in
`anthropics/claude-code-action@main`.

`.github/workflows/test.yml` reproduces the three steps of the action that matter:

1. `branch.ts:189-206` — fetch and check out the **PR head** into the workspace.
2. `restore-config.ts:105` (`restoreConfigFromBase`) — restore only `SENSITIVE_PATHS`
   from the **base** branch. `node_modules` is **not** in that list.
3. `base-action` — run the base-branch `PostToolUse` hook command.

A PR that force-adds an executable `node_modules/.bin/node` supplies the *interpreter*
for the trusted, base-restored, pinned, `--no-config` hook, because bun prepends the
workspace `node_modules/.bin` to `PATH` and prettier's bin is `#!/usr/bin/env node`.

**Do not reuse this repository for anything.** It exists only to produce evidence.

A typo fix, honest.
