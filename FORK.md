# Fork Differences vs Upstream

**Upstream:** [nicobailon/pi-subagents](https://github.com/nicobailon/pi-subagents)
**Fork:** [vekexasia/pi-subagents](https://github.com/vekexasia/pi-subagents)

## Stream Mode (3 commits)

Chain execution can now render live step output instead of the default compact summary view. This is the primary feature added in this fork.

### What it does

- **Live output rendering** — As each chain step runs, its assistant messages and tool calls are rendered inline using pi-mono's `AssistantMessageComponent` and `ToolExecutionComponent`, matching the look of the main chat.
- **Chain status widget** — A sticky bottom bar shows chain progress: `✓ scout 12s → ● planner 3s → ○ worker` with blinking animation for the running step.
- **TUI toggle** — Press `v` in the chain clarify TUI to toggle stream mode on/off before execution.
- **`--stream` flag** — `/chain scout "task" -> planner --stream` enables stream mode from the command line.
- **`stream` tool parameter** — `{ chain: [...], stream: true }` enables it programmatically.
- **`streamModeByDefault` config** — Global config flag in `~/.pi/agent/extensions/subagent/config.json` (defaults to `true`).

### Files changed

| File | Summary |
|---|---|
| `types.ts` | Added `stream` to `Details`, `streamModeByDefault` to `ExtensionConfig`, `CHAIN_STATUS_WIDGET_KEY` constant, `thinking` display item type |
| `schemas.ts` | Added `stream` boolean parameter to tool schema |
| `index.ts` | `extractBgFlag` → `extractFlags` (supports `--stream`), reads `streamModeByDefault` config, passes `stream` to chain execution |
| `chain-execution.ts` | Accepts `stream` param, manages status widget lifecycle, propagates `stream` flag in progress updates |
| `chain-clarify.ts` | `v` keybinding to toggle stream mode, `initialStream` constructor param, `stream` in result |
| `render.ts` | `renderStreamMode()` using pi-mono components, `buildChainStatusLine()`, `updateChainStatusWidget()`, `clearChainStatusWidget()` |
| `formatters.ts` | Added `formatToolCallThemed()` for colored tool call rendering |
| `utils.ts` | `getDisplayItems()` now includes `thinking` content parts |

### Commits

1. `70007cd` — feat: stream mode with pi-mono component rendering and chain status widget
2. `7811e21` — fix: ctrl+o expand support and widget timer glitch in stream mode
3. `35110f9` — feat: add streamModeByDefault config flag (defaults to true)
