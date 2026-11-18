# h149-poc

Throwaway proof-of-concept repository for **H-149** — base-branch hook interpreter
hijack against `anthropics/claude-code-action` (HackerOne #3922068).

This repo simulates an *adopter* of `claude-code-action` that follows the guidance
in `docs/security.md`:

> Keep such commands self-contained: invoke the tool directly with a pinned version
> and pass its configuration on the command line (for example
> `bunx prettier@3.5.3 --no-config --write .` rather than `bun run format`).

The base-branch `.claude/settings.json` hook is exactly that command.

## 2026-11-18 retest

Anthropic shipped a fix in v1.0.277 (PRs #2020 / #2026 / #2088) and the floating
`v1` tag now points at v1.0.282. On pull requests whose author does **not** have
write access, the action replaces tracked `node_modules/`, `.npmrc` and
`bunfig.toml` with the base branch's versions (`restoreToolConfigFromBase` in
`src/github/operations/tool-config.ts`).

- `.github/workflows/claude.yml` runs the official `anthropics/claude-code-action@v1`
  when a maintainer comments `@claude`. The retest PR must come from an account
  without write access (fork PR).
- `.github/workflows/h149-retest.yml` runs the published restore functions from
  `v1`, `v1.0.277`, and the pre-fix control `v1.0.276`, then executes the
  base-branch hook and looks for the hijack marker.

**Do not reuse this repository for anything else.** It exists only to produce evidence.

Retest fixture typo: repository.
