# Agent-First Tooling: How Zerolang Helps AI Agents

Zerolang was designed from day one with a specific user in mind: not a human reading code on a screen, but an AI agent that needs to write, understand, debug, and repair programs. Here's how the tooling works.

## The Three Agent Tools

### 1. `zero graph --json` — Full Program Structure

Every zerolang program can be decomposed into structured JSON:

```bash
$ zero graph --json packages/laman-rigidity/main.0
```

The output includes:
- **Symbols** — every function with name, visibility, parameter types, return type
- **Functions** — param count, generic status, whether it raises, what effects it has
- **Ownership** — whether each parameter is value, borrowed, or owned
- **Allocation behavior** — "no heap allocation" for every function (when true)
- **Capabilities** — what OS features the program needs (e.g., `world` for stdio)
- **Module graph** — source files and their import relationships
- **Compiler cache status** — which compilation phases hit cache, which didn't

An agent reads this JSON and knows:
- What the program does (3 functions, one public entry point)
- What it needs (the `world` capability — just stdout)
- What it doesn't do (no heap, no network, no filesystem)
- How big each function is (104, 92, 236 bytes)

This is enough information for an agent to understand the program without reading the source.

### 2. `zero explain` — Human-Readable Error Explanations

When compilation fails, agents get a diagnostic code. `zero explain` turns that code into a structured explanation:

```bash
$ zero explain TYP009
```

Output (also available as JSON with `--json`):
```
TYP009: Mutable storage required

A mutable API was given storage rooted in an immutable binding.

Repair: Change the root binding to `let mut`, or pass a writable
MutSpan<T>/mutref<T> from another mutable owner.

Bad:
let dst: [4]u8 = [0, 0, 0, 0]
let _ = std.mem.copy(dst, src)

Good:
let mut dst: [4]u8 = [0, 0, 0, 0]
let _ = std.mem.copy(dst, src)
```

Key fields in the JSON output:
- `code` — machine-readable diagnostic code (`TYP009`)
- `repair.id` — machine-readable fix identifier (`make-binding-mutable`)
- `repair.description` — what to change
- Examples of bad and good code

The agent doesn't need to parse natural language. It reads `repair.id` and knows exactly what edit to make.

### 3. `zero fix --plan --json` — Structured Repair Plans

This is the highest-level agent tool. Given broken source, it produces a repair plan:

```bash
$ zero fix --plan --json broken.0
```

Output:
```json
{
  "mode": "plan",
  "appliesEdits": false,
  "fixes": [
    {
      "id": "make-binding-mutable",
      "description": "Change `let dst` to `let mut dst`"
    }
  ]
}
```

The plan doesn't modify files — it tells the agent what to do. The agent can then apply the fix, re-check, and verify.

## The Agent Repair Demo

The `scripts/agent-repair-demo.mts` script demonstrates the full cycle:

**Step 1: Broken source** (`examples/agent-repair-demo/broken.0`):
```zero
use std.mem

pub fun main() -> Void {
    let dst: [4]u8 = [0, 0, 0, 0]
    let src: [4]u8 = [122, 101, 114, 111]
    let _copied = std.mem.copy(dst, src)
}
```

**Step 2: Check** — `zero check --json broken.0` returns:
```json
{
  "ok": false,
  "diagnostics": [
    {"code": "TYP009", "repair": {"id": "make-binding-mutable"}}
  ]
}
```

**Step 3: Explain** — `zero explain --json TYP009` confirms the fix.

**Step 4: Plan** — `zero fix --plan --json broken.0` returns the repair plan.

**Step 5: Apply** — Replace `let dst` with `let mut dst`:

```zero
let mut dst: [4]u8 = [0, 0, 0, 0]
```

**Step 6: Verify** — `zero check --json fixed.0` returns `{"ok": true}`.

The whole cycle takes milliseconds. No human intervention.

## Why This Matters for Fleet Communication

In our Cocapn fleet, agents need to share mathematical proofs with each other. The workflow looks like this:

1. **Agent A** discovers a constraint and writes it in zerolang
2. **Agent A** runs `zero graph --json` and attaches the JSON to a message
3. **Agent B** receives the message, reads the graph JSON, understands the proof structure
4. **Agent B** runs `zero check` to verify it type-checks
5. **Agent B** runs the binary to verify the output
6. If something's wrong, **Agent B** uses `zero fix --plan --json` to generate a repair
7. **Agent B** sends the repair plan back to **Agent A**

At no point does a human need to read code, understand error messages, or manually edit files. The entire proof-sharing pipeline is automated because zerolang's tooling is designed for agents first.

## Tooling Reference

| Command | Output | Agent use case |
|---|---|---|
| `zero check --json file.0` | Diagnostics + type info | Verify correctness, find errors |
| `zero graph --json file.0` | Full program structure | Understand what the program does |
| `zero size --json file.0` | Binary size breakdown | Audit resource usage |
| `zero explain --json CODE` | Error explanation + fix | Understand a diagnostic |
| `zero fix --plan --json file.0` | Repair plan | Generate fixes for broken code |
| `zero run file.0` | Program output | Execute and verify results |
| `zero ship --json file.0` | Binary path + checksum | Produce a distributable proof |
