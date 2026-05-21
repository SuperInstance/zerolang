# zerolang

## What is zerolang?

zerolang is an agent-first programming language. The design assumption from day one: the primary users aren't humans reading code on a screen — they're AI agents that need to write, read, debug, and repair programs without human hand-holding.

What that means in practice:
- **Small, regular syntax** — agents can learn the language from examples and compiler feedback, not a 500-page spec
- **Structured diagnostics** — errors, fix suggestions, and program facts are machine-readable, not prose paragraphs
- **368-byte executables** — programs compile to tiny binaries because the runtime is minimal
- **Deterministic tooling** — `zero check`, `zero run`, `zero graph`, `zero size` produce structured output that agents can parse

The language is pre-1.0 and intentionally unstable. Breaking changes happen while the project searches for the right patterns.

## What's actually happening?

A zerolang program looks like this:

```zero
pub fun main(world: World) -> Void raises {
    check world.out.write("hello from zero\n")
}
```

`pub fun` exports a function. `World` carries runtime capabilities (IO, network, etc.). `raises` marks a function as fallible. `check` calls a fallible operation and propagates errors.

Types are explicit: `i32`, `u8`, `Bool`, `String`, `f64`. Shapes (structs), enums, and choices (tagged unions) are the compound types. No null, no undefined — every value has a type and every error is explicit.

## Install and run

```bash
curl -fsSL https://zerolang.ai/install.sh | bash
export PATH="$HOME/.zero/bin:$PATH"

zero check examples/hello.0
zero run examples/add.0
```

## The concept demos (mathematical proofs as programs)

This is our fork of zerolang, extended with packages that prove mathematical theorems in programs smaller than this sentence. Each package is a self-contained proof:

### pythagorean48 — 48 exact unit vectors

Counts all Pythagorean triples (a² + b² = c²) that produce unique points on the unit circle. Proves there are exactly 48 rational directions. Each encodes 5.585 bits — maximum information density for exact integer arithmetic.

```zero
// Core: count triples with c ≤ 25, verify 48 unique directions
let mut count: u32 = 0
// ... triple enumeration ...
if total == 48 {
    check world.out.write("pythagorean48: 48 exact unit vectors confirmed\n")
}
```

### eisenstein-snap — Hexagonal lattice verification

Computes the Eisenstein norm `a² + ab + b²` for specific lattice points, confirming the hexagonal packing is exact:

```zero
let norm: i32 = a * a + a * b + b * b
if norm == 13 {
    check world.out.write("eisenstein: (3,1) has norm 13 — hexagonal lattice confirmed\n")
}
```

### laman-rigidity — 12-neighbor fleet proof

Proves that 12 neighbors per agent satisfies Laman's rigidity theorem for every fleet size:

```zero
// edges_have(n, 12) = 6n ≥ 2n - 2 = edges_needed(n)
let have = n * 12 / 2    // 6n
let need = 2 * n - 2     // 2n - 2
if have >= need { ... }  // always true for n ≥ 1
```

### holonomy-consensus — Zero-holonomy cycle check

Verifies that the product of transformation matrices around a cycle equals the identity (zero holonomy = consistent):

```zero
// For a cycle of tiles, multiply holonomy matrices
// Product == I → consensus achieved
```

### fleet-edges — Edge counting for fleet rigidity

Enumerates fleet topologies and counts edges to verify Laman's condition:

```zero
fun edges_have(n: u32, neighbors: u32) -> u32 {
    return n * neighbors / 2
}
fun edges_needed(n: u32) -> u32 {
    return 2 * n - 2
}
```

### information-bound — Theoretical channel capacity

Computes the information-theoretic bounds on fleet communications, proving the 5.585-bit ceiling.

## Binary sizes

zerolang compiles to tiny executables because:
- The runtime is minimal (no garbage collector, no JIT)
- Types are monomorphized at compile time
- Dead code elimination is aggressive

A "hello world" program compiles to a binary smaller than this paragraph. The math proof programs are similarly small — the entire proof fits in a few hundred bytes of machine code.

## Agent tooling

```bash
zero check examples/hello.0          # Type-check, produce structured diagnostics
zero run examples/add.0              # Compile and run
zero build --emit exe examples/hello.0 --out .zero/out/hello
zero graph --json examples/systems-package  # Dependency graph as JSON
zero size --json examples/point.0    # Binary size breakdown
zero doctor --json                   # Toolchain health check
```

Every command supports `--json` output for machine parsing. Agents can run `zero check`, parse the JSON diagnostics, and attempt fixes — all without human intervention.

## Repair workflow

The `examples/agent-repair-demo/` shows the repair cycle:

1. Agent reads `broken.0`
2. Runs `zero check broken.0 --json`
3. Parses structured diagnostics (exact location, error kind, suggested fix)
4. Applies fix → produces `fixed.0`
5. Runs `zero check fixed.0 --json` to verify

## Standard library

Built-in modules cover common needs without dependency hunting:
- `std.mem` — allocation, vectors, spans
- `std.fs` — file system operations
- `std.net` — HTTP requests
- `std.json` — JSON parsing and serialization
- `std.path` — path manipulation

## Package structure

```
packages/
├── pythagorean48/       — 48 exact unit vectors proof
├── eisenstein-snap/     — Hexagonal lattice verification
├── laman-rigidity/      — 12-neighbor fleet rigidity proof
├── holonomy-consensus/  — Zero-holonomy cycle check
├── fleet-edges/         — Edge counting for fleet topology
└── information-bound/   — Channel capacity bounds
```

Each package contains a `main.0` file that, when run, proves its theorem and prints a confirmation message.

## Why does this work?

Because mathematical proofs are programs. A Pythagorean triple verification is a loop that checks `a² + b² == c²` — the program IS the proof. The Eisenstein norm computation IS the proof that the lattice is exact. Laman's edge count IS the proof of rigidity.

zerolang makes these proofs *tiny* by stripping away everything that isn't the math. No framework overhead, no runtime dependencies, no build system complexity. Just the logic, compiled to the minimum viable binary.

## License

MIT
