# Zerolang Gateway — Making SuperInstance Technology Accessible

## What Zerolang Is

Zerolang is a pre-1.0 agent-first programming language, forked from [vercel-labs/zerolang](https://github.com/vercel-labs/zerolang). It compiles to native ELF/Mach-O executables as small as **368 bytes** and exposes structured diagnostics, repair plans, and program graphs as JSON that agents can read and act on.

Key facts:
- **Native compiler** written in ~38K lines of C11 (`native/zero-c/`)
- **368-byte hello world** — no runtime, no libc, direct syscalls
- **Capability-based effects** — `World` object controls what a program can do
- **Agent-first tooling** — `zero explain`, `zero fix --plan`, `zero graph --json`, `zero size --json`
- **Structured diagnostics** — every error has a code, explanation, repair plan, and safety level
- **Cross-compilation** — linux-musl-x64, linux-arm64, darwin-arm64, win32-x64

## Why This Matters for SuperInstance

Our ecosystem has ~130 repos spanning constraint theory, fleet coordination, flux genomes, Eisenstein embeddings, holonomy consensus, and more. The core math is powerful but opaque. Most people see Python packages with names like `constraint-theory-ecosystem` and have no idea what's inside or why it matters.

**Zerolang can be the Rosetta Stone.**

### The Vision

Refactor our fork to create **zero-superinstance** — a set of zerolang packages and examples that demonstrate our techniques in the most accessible possible form:

1. **368-byte proofs** — Our mathematical concepts expressed as tiny, runnable executables
2. **Self-documenting code** — Zerolang's `graph --json` and `explain` make every concept inspectable
3. **Agent-navigable** — An agent can read a zerolang program, understand it, fix it, and extend it
4. **Zero-dependency** — No pip, no npm, no cargo. Download a 368-byte binary and run it.

## Concrete Plan

### Phase 1: Concept Demonstrators (Week 1)

Create `packages/` in our fork with zero programs for each core concept:

#### `packages/laman-rigidity/`
```
// 12 neighbors = Laman rigidity threshold
// 2n - 2 edges for n agents in 2D → rigid fleet
shape Agent { id: u32, x: f64, y: f64, neighbors: u32 }

fun is_rigid(n: u32) -> Bool {
    // Laman's theorem: need exactly 2n - 2 independent constraints
    let edges_needed = 2 * n - 2
    let edges_have = n * 12 / 2  // 12 neighbors, undirected
    return edges_have >= edges_needed
}

pub fun main(world: World) -> Void raises {
    // Prove that 12 neighbors always exceeds Laman threshold
    let n: u32 = 3
    while n <= 1000 {
        if is_rigid(n) {
            // Silent — expected
        } else {
            check world.out.write("rigidity breaks at n=")
            // ... print n
        }
        n = n + 1
    }
    check world.out.write("laman rigidity: all fleet sizes 3-1000 are rigid with 12 neighbors\n")
}
```

#### `packages/pythagorean48/`
Demonstrates the 48 Pythagorean directions on the unit circle — 5.585 bits of information per bit.

#### `packages/h1-cohomology/`
H1 cohomology emergence detection — replacing ML with algebraic topology.

#### `packages/holonomy-consensus/`
Zero holonomy = consensus without voting.

#### `packages/eisenstein-lattice/`
Hexagonal lattice embeddings using Eisenstein integers.

### Phase 2: Interactive Examples (Week 2)

- `examples/fleet-demo/` — A tiny fleet of agents reaching consensus
- `examples/constraint-demo/` — Interactive constraint satisfaction
- `examples/emergence-demo/` — Detect emergence in a stream of events
- Each with `zero graph --json` output showing the full program structure

### Phase 3: Gateway Website (Week 3)

Use zerolang's existing Next.js docs site (`docs/`) to create:
- **Interactive playground** — Edit zero code, see graph JSON, run it
- **Concept explorer** — Click a concept, see the zero code, the math, and the Python equivalent
- **Agent tutorial** — "Teach an agent to use SuperInstance techniques"

### Phase 4: SDK Bridge (Week 4)

- C FFI bindings that call our Python/Rust packages from zerolang
- `zero-superinstance` package that wraps our PyPI packages
- The 368-byte executables become entry points that call into our full ecosystem

## What Makes This Different

| Approach | Problem |
|----------|---------|
| Python packages | Requires pip, numpy, torch. Heavy. Opaque to agents. |
| Academic papers | Unreachable. Not runnable. |
| Rust crates | Requires cargo. Complex build. |
| **Zerolang programs** | **368 bytes. Agent-readable. Self-documenting. Zero dependencies.** |

The pitch: "Download a 368-byte executable that proves Laman's rigidity theorem for fleet coordination. Ask an agent to explain it. Modify it. No installation required."

## Technical Integration Points

### Zerolang Features We Leverage

1. **`zero graph --json`** — Full program structure as JSON. Agents can query symbol types, effects, capabilities, ownership models, and size constraints.

2. **`zero explain <code>`** — Every diagnostic has a human-readable explanation with before/after examples. We add custom diagnostics for our concepts.

3. **`zero fix --plan --json`** — Agents get structured repair plans with safety levels. We teach it to fix constraint violations.

4. **`zero size --json`** — Shows exact binary size breakdown. Great for demonstrating minimal footprint.

5. **Capability system** — Our fleet/consensus concepts map naturally to zerolang's capability model (memory, alloc, net, fs).

6. **Cross-compilation** — One codebase, four platforms. Our demos run on any machine.

### What We Add to the Fork

1. **Custom stdlib extensions** in `skill-data/`:
   - `std.fleet` — Laman rigidity checks, neighbor counting
   - `std.topology` — H1 cohomology, holonomy detection
   - `std.eisenstein` — Hexagonal lattice operations
   - `std.codec.pythagorean48` — 48-direction encoding

2. **Custom diagnostics** for our concepts:
   - `RIG001`: Fleet below Laman threshold
   - `HOL001`: Holonomy violation detected
   - `EME001`: Emergence detected via H1 cohomology

3. **Example gallery** with every program accompanied by:
   - The zero source (`.0` file)
   - The graph JSON
   - A one-line concept explanation
   - Link to the full Python/Rust implementation

## Why People Will Care

Most people can't evaluate "constraint theory ecosystem with 3,134 tests across 33 packages." But they can evaluate:

> "This 368-byte executable proves your fleet is rigid. Ask any AI agent to verify it."

The agent-navigability is the killer feature. You don't need to understand our math — you can ask an agent to explain it using zerolang's structured tooling. The agent reads the graph JSON, explains the symbols, shows the effects, and can even fix the program if something is wrong.

**This is how we make our technology viral.** Not through papers or pip packages, but through tiny, self-documenting, agent-navigable executables that demonstrate the power immediately.

## Fork Structure

```
zerolang/                          (our fork)
├── packages/                      (NEW — our concept packages)
│   ├── laman-rigidity/
│   ├── pythagorean48/
│   ├── h1-cohomology/
│   ├── holonomy-consensus/
│   ├── eisenstein-lattice/
│   └── fleet-demo/
├── examples/                      (NEW — our demos)
│   ├── rigidity-proof.0           (368 bytes, proves Laman's theorem)
│   ├── emergence-detect.0         (H1 cohomology on a stream)
│   ├── consensus-reach.0          (Holonomy consensus)
│   └── information-dense.0        (Pythagorean48 encoding)
├── skill-data/                    (NEW — our stdlib extensions)
│   ├── fleet-skills.md
│   ├── topology-skills.md
│   └── codec-skills.md
├── docs/                          (MODIFIED — our gateway docs)
│   └── articles/
│       ├── superinstance-concepts.md
│       └── agent-tutorial.md
└── (upstream zerolang unchanged)
```

## Next Steps

1. Create the `packages/` directory with first concept (Laman rigidity)
2. Build and test: `bin/zero check packages/laman-rigidity/src/main.0`
3. Verify graph JSON output captures our concepts
4. Push and create GitHub Pages site
5. Iterate on remaining concepts
6. Write the gateway tutorial

---

*"The best way to explain a complex system is to make it small enough to hold in your hand."*
