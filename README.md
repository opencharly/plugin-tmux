# plugin-tmux

Typed terminal sessions for OpenCharly — the `tmux` terminal and agent runtime.

Each run owns an isolated tmux control-mode socket. Literal input, bracketed
paste, keys, resize, signals, ordered raw output, snapshots, detach/reattach,
exit status, and cleanup all travel through the CUE-generated
`Provider.Channel` contract. The provider is placement-neutral, so it works
locally, over gRPC/SSH, and in nested deployments.

Operators and MCP clients use `charly agent terminal`, so terminal operations
never recursively invoke `charly cmd` or `charly shell`.

## What it provides

| Capability | Surface |
|---|---|
| `terminal:tmux` | the typed tmux terminal channel (`Provider.Channel`) |
| `agent-runtime:tmux` | tmux as an agent runtime |

## How to use it

Compose the plugin candy, then drive terminals through the agent control plane:

```bash
charly agent runtime status tmux --class terminal
charly agent terminal ...
```

See `/charly-automation:tmux` for the full terminal/agent session surface.

## Layout

- `candy/plugin-tmux/` — the plugin module: `provider.go` / `plugin.go`,
  `terminal.go` (the control-mode driver), `schema/tmux.cue`,
  `params/cue_types_gen.go`, `cmd/serve/main.go`.
- `charly.yml` — the root project manifest (`discover: candy`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.

## Related

- Owning skill: `/charly-automation:tmux` — the typed terminal and
  terminal-agent session surface. This candy carries no `skill:` entity of its
  own; the gap is tracked in
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).
- `/charly-automation:agent` — the agent control plane that drives these
  terminals.
- `/charly-internals:plugin` — the plugin/provider model.
