# Kimi CLI Hooks

> Part of [`hooks/`](../README.md) — see also [`src/hooks/`](../../src/hooks/README.md) for installation code

## Specifics

- Uses the `rtk hook kimi` Rust binary (no `jq` dependency)
- Kimi CLI's PreToolUse hook is **deny-only** — it cannot rewrite the command
  via `updatedInput`. RTK responds with `permissionDecision: "deny"` and a
  suggestion in `permissionDecisionReason`. Kimi feeds the reason back to the
  LLM, which retries with the suggested `rtk <cmd>` form.
- Configuration lives in `~/.kimi/config.toml` as a `[[hooks]]` block
  (kimi-cli is global-config-based).
- Awareness is handled via AGENTS.md — run `rtk init --agent kimi` to install.

## Install

```bash
rtk init --agent kimi
```

## Test

```bash
echo '{"tool_name":"Shell","tool_input":{"command":"git status"}}' | rtk hook kimi
# {"hookSpecificOutput":{"hookEventName":"PreToolUse","permissionDecision":"deny","permissionDecisionReason":"Use `rtk git status` instead — RTK saves 60-90% tokens"}}
```