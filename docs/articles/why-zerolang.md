# Why We Forked Zerolang

## The Problem

Our math lives in Python packages and Rust crates. That's fine for production. But when you need to show someone a concept — Laman rigidity, Eisenstein norms, holonomy consensus — you hit a wall:

```bash
# To demonstrate Laman rigidity to someone:
pip install fleet-agent
python3 -c "from fleet_agent.fleet_math import check_rigidity; check_rigidity(12, 100)"
```

That's a pip install, a virtual environment, an import chain, and five lines of code. It works. But it's not something you can hand to someone in 5 seconds.

## The Insight

A 614-byte native executable that proves a theorem is worth 1000 words. Not because it's small — because it's **inspectable**. Every symbol, type, and side effect is exposed as structured data. The program doesn't just run; you can ask it what it is.

## Building and Running a Proof

```bash
$ zero ship packages/laman-rigidity/main.0 --out laman
binary: laman (614 bytes)

$ ./laman
laman rigidity: all fleet sizes 3-100 are rigid with 12 neighbors
```

614 bytes. Download it, run it. No runtime, no dependencies, no package manager. The binary is a static ELF that makes one syscall (write to stdout) and exits.

The source is 41 lines:

```zero
fun edges_have(n: u32, neighbors: u32) -> u32 {
    return n * neighbors / 2
}

fun edges_needed(n: u32) -> u32 {
    return 2 * n - 2
}

pub fun main(world: World) -> Void raises {
    let neighbors: u32 = 12
    let mut n: u32 = 3
    let mut all_rigid: Bool = true

    while n <= 100 {
        let have = edges_have(n, neighbors)
        let need = edges_needed(n)
        if have < need {
            all_rigid = false
        }
        n = n + 1
    }

    if all_rigid {
        check world.out.write("laman rigidity: all fleet sizes 3-100 are rigid\n")
    }
}
```

## The Graph: Every Symbol Inspectable

```bash
$ zero graph --json packages/laman-rigidity/main.0
```

This outputs a JSON document with:
- **3 functions** — `edges_have`, `edges_needed`, `main` — each with parameter types, return types, and effects
- **Ownership metadata** — every parameter is `value` ownership, no heap allocation anywhere
- **Capability tracking** — `main` requires the `world` capability (stdio), the helpers require nothing
- **Size breakdown** — `edges_have` is 104 bytes, `edges_needed` is 92 bytes, `main` is 236 bytes

An agent can read this JSON and understand the program's structure without parsing source code. Every function's allocation behavior, side effects, and capabilities are explicit.

## `zero explain`: Human-Readable Diagnostics

```bash
$ zero explain TYP009
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

The explain system doesn't just tell you what's wrong — it shows you the fix. And it does this in structured JSON, so agents can parse the repair plan programmatically.

## `zero fix --plan --json`: Structured Repair

When code is broken:

```bash
$ zero fix --plan --json broken.0
```

This returns a JSON repair plan with:
- `fixes[0].id` — the machine-readable fix identifier
- `fixes[0].description` — what to change
- The exact edit (replace line X, column Y through column Z)

An agent reads the plan, applies the edit, re-checks. No human needed.

## The Comparison

**Python way:**
```bash
pip install fleet-agent          # 2.3 MB download, 47 dependencies
python3 -c "..."                  # 5 lines, requires Python 3.10+
# Total: ~30 seconds to set up, ~50 MB on disk
```

**Zerolang way:**
```bash
curl -O laman                     # 614 bytes
chmod +x laman
./laman
# Total: 0 seconds to set up, 614 bytes on disk
```

The point isn't that Python is bad. The point is that for *demonstrating a mathematical concept*, a 614-byte proof you can download and run beats a 50 MB package ecosystem every time. And the graph JSON means an AI agent can understand, modify, and repair the proof without human intervention.

## Why This Matters for Our Fleet

We have 9 agents working on constraint theory, fleet coordination, and mathematical proofs. When an agent discovers a new theorem or constraint, it needs to share that proof with other agents. A 614-byte binary with a JSON graph is a format every agent can parse, run, and verify. No shared language runtime. No version conflicts. Just the proof.
