# AGENTS.md — plugin-tmux

Standalone plugin repo for the tmux terminal and agent runtime
(`terminal:tmux`, `agent-runtime:tmux`). The plugin is a Go module at
`candy/plugin-tmux/` (module path
`github.com/opencharly/plugin-tmux/candy/plugin-tmux`); the root `charly.yml`
only declares `discover: candy` so the repo is a project and its candy is
scanned.

Canonical files:

- `candy/plugin-tmux/charly.yml` — the `plugin-tmux:` candy entity (`plugin:`
  block, `plan:` checks).
- `candy/plugin-tmux/provider.go` / `plugin.go` — the terminal + agent-runtime
  providers.
- `candy/plugin-tmux/terminal.go` — the tmux control-mode channel driver.
- `candy/plugin-tmux/schema/tmux.cue` — the self-contained input schema.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-internals:plugin` — the plugin authoring reference: the `plugin:`
  block, the unified Provider model, the CUE-generated `Provider.Channel`
  contract, placement. Load before touching the provider or schema.
- `/charly-automation:tmux` — the typed terminal and terminal-agent session
  surface this provider serves.
- `/charly-automation:agent` — the agent control plane that drives terminals.
- `/charly-internals:git-workflow` — before any git/PR action.

## Build / validate / test

- `go build ./...` in `candy/plugin-tmux/` — compile the plugin module.
- `go test ./...` in `candy/plugin-tmux/` — the plugin's Go tests
  (`terminal_test.go`, `no_timed_poll_test.go`).
- `charly box validate` at the repo root — the structural check (the candy +
  `plugin:` block, CUE schema).
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate.
- The `plan:` checks run `charly agent runtime status tmux --class …` against a
  live deployment.

## Modify this repo

- Edit the `plugin-tmux:` candy entity, the Go source, and `schema/tmux.cue`
  **together** — the schema is the single source for the `params/` struct, so a
  field change not mirrored in the schema desyncs the generated types.
- The provider is placement-neutral (local / gRPC / SSH / nested); do not
  describe it as tied to one transport.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
