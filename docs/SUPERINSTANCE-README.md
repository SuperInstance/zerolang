# SuperInstance

*A working library of mathematical techniques for coordination, constraint, and emergence — implemented across 10 languages.*

## What's Here

This organization contains ~130 repositories spanning a single interconnected idea: **you can build reliable multi-agent systems from algebraic geometry instead of heuristics.**

The code proves it. The math is real. The tests pass.

## Where to Start

### You want to understand the core idea

Read [`constraint-theory-ecosystem`](https://github.com/SuperInstance/constraint-theory-ecosystem). It contains 10 chapters walking through the theory from first principles, with Python implementations you can run immediately.

```bash
pip install constraint-theory-ecosystem
python3 -c "from constraint_ecosystem import *; print('installed')"
```

### You want to see the math made physical

Read [`zerolang`](https://github.com/SuperInstance/zerolang) — our fork of the zerolang agent-first language. We've written programs that prove our core theorems as sub-KB native executables:

```bash
# Build the compiler (one step)
cd zerolang && make -C native/zero-c

# Prove Laman's rigidity theorem for fleet coordination
bin/zero run packages/laman-rigidity/main.0
# Output: "laman rigidity: all fleet sizes 3-100 are rigid with 12 neighbors"

# Verify Eisenstein integer norms on the hexagonal lattice
bin/zero run packages/eisenstein-snap/main.0
# Output: "eisenstein: (3,1) has norm 13 — hexagonal lattice confirmed"
```

These compile to native ELF binaries under 1.5KB. No runtime. No libc. Pure math, machine code.

### You want to use it in your code

Each concept has packages in multiple languages:

| Concept | Python | Rust | C | CUDA |
|---------|--------|------|---|------|
| Eisenstein embeddings | [`eisenstein-embed`](https://github.com/SuperInstance/eisenstein-embed) | [`eisenstein`](https://github.com/SuperInstance/eisenstein) | in forgemaster | in ai-forest |
| Constraint theory | [`constraint-theory-ecosystem`](https://github.com/SuperInstance/constraint-theory-ecosystem) | [`constraint-theory-rust-python`](https://github.com/SuperInstance/constraint-theory-rust-python) | [`flux-engine-c`](https://github.com/SuperInstance/flux-engine-c) | in forgemaster |
| Fleet coordination | [`fleet-agent`](https://github.com/SuperInstance/fleet-agent) | [`fleet-resonance`](https://github.com/SuperInstance/fleet-resonance) | [`warp-room`](https://github.com/SuperInstance/warp-room) | in marine-gpu-edge |
| Holonomy consensus | [`fleet-health-monitor`](https://github.com/SuperInstance/fleet-health-monitor) | [`holonomy-consensus`](https://github.com/SuperInstance/holonomy-consensus) | — | — |
| Spline interpolation | [`tensor-spline`](https://github.com/SuperInstance/tensor-spline) | — | — | — |

## The Core Ideas, Explained Without Hype

### Laman Rigidity (Why 12 Neighbors?)

A network of agents in 2D is "rigid" — meaning its shape can't deform — if it has exactly `2n - 2` independent constraints for `n` agents. This is Laman's theorem from 1970.

If each agent connects to 12 neighbors, the network has `6n` edges (undirected). Since `6n ≥ 2n - 2` for all `n ≥ 1`, the network is **always rigid**. That's why our fleet coordination uses 12 neighbors — not 8, not 16. Twelve is the threshold.

You can verify this yourself:

```bash
cd zerolang && bin/zero run packages/laman-rigidity/main.0
```

### Eisenstein Integers (Why Hexagonal?)

Eisenstein integers are complex numbers of the form `a + bω` where `ω = e^(2πi/3)`. They form a hexagonal lattice — the densest possible 2D packing (proven by Thue in 1910).

The Eisenstein norm is `|a + bω|² = a² + ab + b²`. This is the distance metric we use for embeddings because hexagonal packing gives maximum resolution per bit.

```python
from eisenstein_embed import eisenstein_norm
print(eisenstein_norm(3, 1))  # 13
print(eisenstein_norm(2, 2))  # 12
```

### Pythagorean48 (5.585 Bits Per Bit)

There are exactly 48 unit vectors on the unit circle with rational coordinates — the 3-4-5 triangle has 12 rotations times 4 sign combinations. `log₂(48) = 5.585`, meaning each direction carries 5.585 bits of information. An 8-bit integer only stores 8 bits. So we're using 69.8% of the theoretical maximum.

```python
from fleet_agent.fleet_math import encode_pythagorean48, BITS_PER_VECTOR
print(BITS_PER_VECTOR)  # 5.585
```

### H1 Cohomology (Emergence Without ML)

Cohomology is a tool from algebraic topology that detects "holes" in data structures. The first cohomology group H¹ measures loops that can't be contracted — patterns that persist. Instead of training a neural network to detect emergence, we compute H¹ algebraically. It's a 127-line function that replaces an ML pipeline.

### Holonomy Consensus (Agreement Without Voting)

Holonomy measures how much a vector "twists" when parallel-transported around a loop. Zero holonomy means the loop is flat — everyone agrees. Instead of running a voting protocol (Paxos, Raft), agents converge by averaging with neighbors. When holonomy hits zero, consensus is reached. No leader election. No quorum.

## The Tooling

### Testing

We test aggressively. Current status: **3,134 tests across 33 Python packages, 0 failures**. Plus Rust, C, CUDA, Go, Zig, and Fortran tests.

```bash
# Run the full Python test suite
cd constraint-theory-ecosystem && python3 -m pytest tests/ -q
# 786 passed, 0 failed
```

### Languages We Use

| Language | Repos | Why |
|----------|-------|-----|
| Python | 33 packages | Research, prototyping, PyPI distribution |
| Rust | 18 crates | Type-safe systems code, crates.io |
| C | 10 projects | Embedded targets, minimal runtime |
| CUDA | 6 projects | GPU constraint evaluation at 62B/s |
| Go | 2 projects | Floor agent in ai-forest |
| Zig | 2 projects | Deadband filtering, Penrose tiling |
| Fortran | 1 project | Sediment transport physics |
| Zerolang | 10 programs | Educational proofs, sub-KB executables |

### Build Status

From our most recent sweep (2026-05-21):

**Python:** 33 packages, 3,134 tests passing
**Rust:** 13/18 build, 3 blocked on missing deps, 2 have test failures
**C:** 6/6 build with passing tests (flux-engine-c: 64/64, flux-fortran: 15/15, flux-fracture-c: 47/47)
**CUDA:** 4/6 build cleanly (2 fixed — just missing `#include <stdint.h>`)
**Go:** 2/2 build
**Zig:** 2/2 build

## The Story

This started as constraint theory research — the mathematics of when a system of equations has a unique solution. Along the way it turned into a fleet coordination framework, then an emergence detection system, then a GPU optimization pipeline. The connecting thread is always the same: **algebraic structure beats heuristic tuning.**

The repos are organized by concept, not by language. The same Eisenstein norm exists in Python, Rust, C, CUDA, and zerolang. Pick whichever language fits your use case.

## Contributing

PRs welcome. The bar is:

1. Tests must pass
2. New concepts need at least one worked example
3. Code should teach, not just work

See [`CONTRIBUTING.md`](https://github.com/SuperInstance/sunset-ecosystem/blob/main/CONTRIBUTING.md) for details.

## License

MIT across the board unless otherwise noted.
