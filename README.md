# SuperInstance / zerolang

We forked [zerolang](https://github.com/vercel-labs/zerolang) to use it as a teaching tool. The programs in this repo prove mathematical theorems as native executables smaller than this paragraph.

## What's in Here

### `packages/` — Concept Demos

Each directory contains a zerolang program that demonstrates one mathematical concept. They compile to standalone ELF binaries — no interpreter, no runtime, no libc.

| Package | What It Shows | Binary Size |
|---------|--------------|-------------|
| `laman-rigidity` | Fleet of agents stays rigid with 12 neighbors | 614 bytes |
| `eisenstein-snap` | Hexagonal lattice distance (a²+ab+b²) | 715 bytes |
| `pythagorean48` | 48 exact unit vectors on the unit circle | 1,068 bytes |
| `holonomy-consensus` | 5 agents converge without voting | 1,452 bytes |
| `fleet-edges` | Edge count always exceeds Laman threshold | 1,438 bytes |
| `information-bound` | log₂(48) = 5.585 bits of information per byte | 784 bytes |

### Run Them

```bash
# Build the compiler
make -C native/zero-c

# Check a program (verify it type-checks)
bin/zero check packages/laman-rigidity/main.0

# Run it
bin/zero run packages/laman-rigidity/main.0
# Output: laman rigidity: all fleet sizes 3-100 are rigid with 12 neighbors

# Build a native executable
bin/zero build --emit exe --target linux-musl-x64 packages/laman-rigidity/main.0 --out /tmp/proof
/tmp/proof
# Output: laman rigidity: all fleet sizes 3-100 are rigid with 12 neighbors

# Check the size
ls -la /tmp/proof
# -rwxr-xr-x 1 user user 614 May 21 10:00 /tmp/proof
```

614 bytes. That's the theorem, the proof loop, the output string, the ELF headers, and the syscall to write it. Nothing else.

### Understand Them

Zerolang's tooling shows you exactly what a program does:

```bash
# See every function, type, and effect as structured JSON
bin/zero graph --json packages/laman-rigidity/main.0

# Get an explanation of any diagnostic
bin/zero explain TYP009

# Get a machine-readable repair plan
bin/zero fix --plan --json packages/laman-rigidity/main.0

# Check the exact binary size breakdown
bin/zero size --json packages/laman-rigidity/main.0
```

## The Concepts, Briefly

### Laman Rigidity

A structure in 2D is rigid if it has exactly `2n - 2` independent bars for `n` joints. This is Laman's theorem (1970). If each joint connects to 12 neighbors, you get `6n` bars, which always exceeds `2n - 2`. That's why our fleet coordination uses 12 neighbors — it's the threshold where rigidity is guaranteed.

### Eisenstein Integers

Complex numbers of the form `a + bω` where `ω = e^(2πi/3)` form a hexagonal lattice. The hexagonal lattice is the densest possible 2D packing (Thue, 1910). The distance metric is `a² + ab + b²`, and it's what we use for embedding data because hexagonal resolution beats rectangular.

### Pythagorean48

The 3-4-5 right triangle generates 12 rotations on the unit circle, times 4 sign combinations, giving 48 exact unit vectors with rational coordinates. `log₂(48) = 5.585`, meaning each direction carries 5.585 bits. An 8-bit integer only holds 8 bits total — so we're at 69.8% of theoretical maximum information density.

### Holonomy Consensus

Holonomy measures how much a vector "twists" when you walk it around a loop on a curved surface. On a flat surface (zero holonomy), the vector comes back unchanged. In our fleet, each agent averages with its neighbors. When all values converge (zero "twist"), consensus is reached. No leader election. No quorum. No voting.

### Information Bound

Shannon's theorem says the maximum information in a symbol is `log₂(N)` bits where `N` is the number of distinct symbols. With 48 directions, that's 5.585 bits per direction. This program computes that bound by finding the power of 2 that brackets 48: `2⁵ = 32 < 48 < 64 = 2⁶`.

## The Full Ecosystem

These tiny programs are proofs of concept. The real implementations are in our [Python packages](https://pypi.org/user/superinstance/), [Rust crates](https://crates.io/users/superinstance), and [C/CUDA repos](https://github.com/orgs/SuperInstance/repositories):

| What | Python Package | Rust Crate | C/CUDA |
|------|---------------|-----------|--------|
| Eisenstein embeddings | `eisenstein-embed` | `eisenstein` | in forgemaster |
| Constraint theory | `constraint-theory-ecosystem` | `constraint-theory-rust-python` | `flux-engine-c` |
| Fleet coordination | `fleet-agent` | `fleet-resonance` | `warp-room` |
| Holonomy consensus | `fleet-health-monitor` | `holonomy-consensus` | — |
| Spline interpolation | `tensor-spline` | — | — |

3,134 tests passing across 33 Python packages. Build status and cross-language benchmarks in [`docs/`](docs/).

## The Upstream

This is a fork of [vercel-labs/zerolang](https://github.com/vercel-labs/zerolang). The upstream project is building an agent-first programming language. We're using it as a medium for mathematical proofs.

All zerolang features work as documented upstream. Our additions are in `packages/`, `skill-data/`, and `docs/`.

## License

MIT (upstream zerolang) + MIT (our additions).
