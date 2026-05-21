# Concept Cross-Reference Map

> Generated 2026-05-21 — comprehensive survey of 12 core mathematical concepts across all workspace repos.

---

## 1. Laman Rigidity

**Definition:** A graph is Laman-rigid in 2D when it has exactly 2n−2 edges and every subgraph on k vertices has at most 2k−3 edges. On hexagonal lattices the natural degree-6 connectivity gives 1.5× redundancy over the Laman threshold.

| Repo | Language | Key Files | Control Surface |
|------|----------|-----------|-----------------|
| `constraint-theory-math` | Python | `hex-zhc/laman_proof.py`, `hex-zhc/hex_zhc.py`, `hex-zhc/hex_graph.py` | E = 3V (hex interior); Laman ratio = 1.5× |
| `constraint-theory-math` | Python | `hex-zhc/ANALYSIS.md` | Theorem proofs and benchmarks |
| `holonomy-consensus` | Rust | `src/zhc_gl9.rs` (GL(9) extension references hex rigidity) | Zero-holonomy via Laman-sparse spanning trees |
| `constraint-theory-py` | Python | `constraint_theory/temporal.py`, `constraint_theory/eisenstein.py` | COVERING_RADIUS = 1/√3 ≈ 0.577 |
| `constraint-theory-rust-python` | Rust + Python | `src/constraint.rs`, `src/lib.rs` | Core constraint solver |
| `constraint-theory-engine-cpp-lua` | C++ + Lua | `src/constraint.cpp`, `src/solver.cpp` | Same solver in C++ with Lua bridge |
| `constraint-theory-llvm` | Rust | `src/analog_compute.rs`, `src/lib.rs` | JIT-compiled constraint checking |
| `constraint-theory-mlir` | C++ + MLIR | `lib/Dialect/FluxOps.cpp`, `lib/Conversion/FluxToLLVM/FluxToLLVM.cpp` | Compiler-level constraint dialect |
| `constraint-theory-mojo` | Mojo | `src/constraint.mojo`, `src/engine.mojo` | Mojo-native constraint engine |
| `constraint-theory-ecosystem` | C + Python + Rust | `deploy/bare-metal.c`, `deploy/minimal-linux.c`, `src/python/flux_genome.py` | Cross-language golden vectors |
| `eisenstein` | Rust | `src/lib.rs` | `no_std` Eisenstein integer arithmetic |
| `swarm-rooms` | Python | `swarm_rooms/eisenstein.py`, `swarm_rooms/network.py` | Hexagonal room assignment |

**Cross-language equivalence:** The Laman/edge-count check appears identically in Python (`constraint-theory-math/hex-zhc/`), Rust (`eisenstein/`, `holonomy-consensus/`), C++ (`constraint-theory-engine-cpp-lua/`), and MLIR (`constraint-theory-mlir/`).

---

## 2. Eisenstein Integers

**Definition:** Elements of Z[ω] where ω = e^(2πi/3) = −½ + i√3/2. They form a hexagonal lattice in the complex plane. Every point is within covering radius ρ = 1/√3 of a lattice point.

| Repo | Language | Key Files | Control Surface |
|------|----------|-----------|-----------------|
| `eisenstein` | Rust | `src/lib.rs` | `no_std`, `A2Point`, `OMEGA_RE/OMEGA_IM`, `COVERING_RADIUS` |
| `constraint-theory-py` | Python | `constraint_theory/eisenstein.py` | `A2Point`, `Dodecet`, `snap()`, `COVERING_RADIUS` |
| `constraint-theory-math` | Python | `eisenstein-triples/eisenstein_triples.py`, `eisenstein-triples/analyze.py`, `eisenstein-triples/verify_proofs.py` | Triple analysis and prime norm proofs |
| `constraint-theory-math` | Python | `eisenstein-prime-norms.md` | Prime norm theory |
| `swarm-rooms` | Python | `swarm_rooms/eisenstein.py` | Hexagonal lattice room assignment |
| `deadband-rs` | Rust | `target/package/deadband-0.1.0/src/eisenstein.rs` | Eisenstein arithmetic in deadband context |
| `eisenstein-embed` | Python | `eisenstein_embed/eisenstein_quantize.py`, `eisenstein_embed/deadband_rust.py` | Lattice-based quantization |
| `tensor-spline` | Python | `tensor_spline/spline.py`, `tensor_spline/spline_nd.py` | `EisensteinLattice` control-point layout |
| `plato-training` | Python | `plato_training/eisenstein_encoder.py` | Eisenstein-structured weight encoder |
| `dodecet-encoder` | Rust | (original Rust crate ported into `constraint-theory-py`) | 12-bit dodecet encoding |
| `constraint-theory-rust-python` | Rust | `src/constraint.rs` | Constraint solver using Eisenstein snapping |

**Cross-language equivalence:** `eisenstein/src/lib.rs` (Rust `no_std`) ↔ `constraint-theory-py/constraint_theory/eisenstein.py` (pure Python port) — identical covering radius, snapping algorithm, and dodecet encoding.

---

## 3. Pythagorean48 (48 Unit Vectors, 5.585 bits)

**Definition:** The 48 exact unit vectors representable with 16-bit integer numerators. log₂(48) = 5.585 bits — the theoretical information ceiling for fleet vector communication. Enables bit-identical reproduction across 1000+ hops.

| Repo | Language | Key Files | Control Surface |
|------|----------|-----------|-----------------|
| `holonomy-consensus` | Rust | `src/encoding.rs` | `Vector48(u8)`, 48 directions, 5.585 bits |
| `constraint-theory-math` | Python | Various proofs and analysis | log₂(48) = 5.585 theoretical ceiling |
| `dodecet-encoder` | Rust | Original 12-bit encoding crate | 12-bit subset of Pythagorean48 |

**Cross-language equivalence:** `holonomy-consensus/src/encoding.rs` (Rust) encodes the same 48 directions proven in `constraint-theory-math/`.

---

## 4. H1 Cohomology (Emergence Detection)

**Definition:** Sheaf cohomology H¹ measures emergent patterns in a network. H¹_dim = E − V + H⁰_dim, where H¹_dim > 0 means independent cycles (emergent behavior) exist. Replaces 12K-line ML detectors with 127 lines of pure math.

| Repo | Language | Key Files | Control Surface |
|------|----------|-----------|-----------------|
| `holonomy-consensus` | Rust | `src/cohomology.rs` | `EmergenceResult { h0, h1, emergence_detected }` |
| `sheaf-constraint-synthesis` | Markdown | `SYNTHESIS.md`, `README.md` | Theoretical framework |
| `constraint-theory-math` | Markdown | `proposed/CONSISTENCY-HOLONOMY-CORRESPONDENCE.md` | Correspondence theorem |
| `polyformalism-thinking` | Markdown | `research/PAPER-MATHEMATICAL-FRAMEWORK.md`, `research/PHYSICS-SYNERGY.md` | Mathematical formalism |

**Control Surface:** H¹_dim > 0 → emergence detected. 100% true positive, 0% false positive (vs 62%/38% for ML).

---

## 5. Holonomy Consensus

**Definition:** Zero-holonomy consensus replaces voting with geometric constraint satisfaction. If the holonomy matrix product around a cycle equals identity (Hol(γ) = I), the system is globally consistent. No voting required.

| Repo | Language | Key Files | Control Surface |
|------|----------|-----------|-----------------|
| `holonomy-consensus` | Rust | `src/consensus.rs` | `HolonomyMatrix`, deviation from identity |
| `holonomy-consensus` | Rust | `src/zhc_gl9.rs` | GL(9) 9×9 intent transforms |
| `holonomy-consensus` | Rust | `src/constraints.rs` | INT8-saturated bounds, `sat8()`, `HolonomyBounds` |
| `holonomy-consensus` | Rust | `src/benchmarks.rs` | 38ms vs 412ms PBFT comparison |
| `holonomy-bounded` | (exists) | — | Bounded holonomy variant |
| `pbft-rust` | Rust | — | PBFT baseline for comparison |
| `constraint-theory-math` | Markdown | `proposed/INTENT-HOLONOMY-DUALITY-ATTEMPT.md` | Intent-holonomy duality |

**Control Surface:** `||Hol(γ) − I|| < ε` → consistent; INT8 max_deviation = 10 (0.01 × 1000).

---

## 6. Constraint Theory

**Definition:** Mathematical framework for determining whether a system of constraints is rigid (exactly determined), flexible (underdetermined), or over-constrained. Core operations: rigidity matrix, infinitesimal flexibility analysis, degrees of freedom counting.

| Repo | Language | Key Files | Control Surface |
|------|----------|-----------|-----------------|
| `constraint-theory-py` | Python | `constraint_theory/{__init__,temporal,eisenstein,adaptive,plato,baton}.py` | Full Python toolkit v0.3.0 |
| `constraint-theory-rust-python` | Rust + Python | `src/constraint.rs`, `src/lib.rs`, `python/flux_constraint/` | PyO3 bridge, Rust core |
| `constraint-theory-engine-cpp-lua` | C++ + Lua | `src/constraint.cpp`, `src/solver.cpp`, `lua/flux.lua` | AVX512-optimized C++, Lua scripting |
| `constraint-theory-llvm` | Rust | `src/lib.rs`, `src/jit.rs`, `src/analog_compute.rs`, `src/emitter.rs` | LLVM JIT compilation |
| `constraint-theory-mlir` | C++ + MLIR | `lib/Dialect/FluxDialect.cpp`, `lib/Conversion/FluxToLLVM/` | MLIR compiler dialect |
| `constraint-theory-mojo` | Mojo | `src/constraint.mojo`, `src/engine.mojo` | Mojo-native implementation |
| `constraint-theory-math` | Python + Markdown | `hex-zhc/`, `eisenstein-triples/`, `galois-unification-visualizer.py` | Proofs and formal verification |
| `constraint-theory-ecosystem` | Multi | `src/python/flux_genome.py`, `deploy/bare-metal.c`, `benchmarks/` | Cross-language ecosystem |
| `constraint-demos` | — | — | Demo notebooks |
| `constraint-inference` | — | — | Inference tooling |
| `constraint-theory-core-cuda` | CUDA | — | GPU-accelerated constraint checking |
| `intent-directed-compilation` | — | — | Intent → constraint compilation |
| `intent-inference` | — | — | Intent inference from constraints |
| `sheaf-constraint-synthesis` | Markdown | `SYNTHESIS.md` | Sheaf-theoretic synthesis |

**Cross-language equivalence:** The core constraint check (`check_constraint`) is implemented identically in Python, Rust, C++, Lua, MLIR, Mojo, and CUDA — all sharing the same golden test vectors (`constraint-theory-ecosystem/tools/generate_golden_vectors.py`).

---

## 7. Flux Genome (Tick/Degrade/Incubate Protein Evolution)

**Definition:** Biological metaphor for constraint system evolution. The genome is a fixed Tensor-Penrose DNA encoding all possible constraint-checking proteins. Environmental context determines gene expression; the ribosome assembles proteins; incubation matures them through tick/degrade cycles.

| Repo | Language | Key Files | Control Surface |
|------|----------|-----------|-----------------|
| `flux-genome-py` | Python | `flux_genome/genome.py`, `flux_genome/expression.py`, `flux_genome/ribosome.py`, `flux_genome/builtins.py` | `Genome`, `ExpressionProfile`, `Incubator` |
| `constraint-theory-ecosystem` | Python | `src/python/flux_genome.py` | Genetic expression engine for Tensor-Penrose DNA |
| `flux-genome` | — | — | Original genome spec |

**Control Surface:** Gene activation = environmental context match; expression levels = protein concentration; tick/degrade = temporal maturation.

---

## 8. Nerve Adaptation (Adaptation Score, Threshold-Based Learning)

**Definition:** Bio-inspired adaptation where repeated signal exposure decreases cognitive load ("the shoe metaphor" — you stop noticing your shoes). An `AdaptationEngine` tracks all nerve fibers and manages the progression: feel every edge → dampen → muscle memory → novelty alert.

| Repo | Language | Key Files | Control Surface |
|------|----------|-----------|-----------------|
| `sunset-ecosystem` | Python | `nerve/adaptation.py`, `nerve/fiber.py` | `AdaptationEngine`, `ShoeTracker`, `ShoeState` |
| `sunset-ecosystem` | Python | `tests/test_nerve.py` | Adaptation test suite |
| `sunset-ecosystem` | Python | `swarm/chaos.py`, `swarm/swarm_runner.py` | Chaos testing with adaptation |
| `NERVE-FIBER-ARCHITECTURE.md` | Markdown | Workspace root doc | Architecture specification |

**Control Surface:** `adaptation_score` per fiber; threshold determines dampening onset; novelty alert when new signal breaks adapted pattern.

---

## 9. Tripartite Architecture (Ethos/Pathos/Logos Trinity)

**Definition:** Three-body selection pressure for agent sunset decisions. Ethos (values alignment), Pathos (emotional resonance), Logos (logical relevance). Score = product of the three; if any is zero, the agent sunsets.

| Repo | Language | Key Files | Control Surface |
|------|----------|-----------|-----------------|
| `sunset-ecosystem` | Python | `sunset/trinity_scorer.py`, `sunset/agent.py`, `sunset/sunset_documents.py` | `trinity_score(ethos, pathos, logos)` |
| `sunset-ecosystem` | Python | `ethos/__init__.py`, `pathos/trinity_connection.py` | Ethos/Pathos modules |
| `sunset-ecosystem` | Python | `experiments/distillation_demo.py` | Demo with trinity scoring |
| `experiments/` | Python | `trinity_scoring.py`, `hardware_swarm.py` | Early trinity experiments |
| `TRIPARTITE-ARCHAEOLOGY.md` | Markdown | Workspace root | Archaeological investigation of the pattern |
| `TRINITY-AS-INVESTIGATION.md` | Markdown | Workspace root | Trinity investigation notes |
| `SUNSET-DESIGN.md` | Markdown | Workspace root | Sunset ecosystem design doc |

**Control Surface:** Score = ethos × pathos × logos ∈ [0, 1]. If any factor = 0, agent sunsets (dies).

---

## 10. Spline Interpolation (Control Points, Lattice-Parameterized Weights)

**Definition:** Neural network weights parameterized by control points on an Eisenstein (hexagonal) lattice instead of independent floats. Enables massive compression (e.g., 512×512 dense = 262K params → 16 control points).

| Repo | Language | Key Files | Control Surface |
|------|----------|-----------|-----------------|
| `tensor-spline` | Python (PyTorch) | `tensor_spline/spline.py` | `EisensteinLattice`, `SplineLinear`, `inject_spline()` |
| `tensor-spline` | Python (PyTorch) | `tensor_spline/hierarchical_spline.py` | `HierarchicalSplineLinear` — coarse+fine multi-scale |
| `tensor-spline` | Python | `tensor_spline/spline_nd.py` | N-dimensional extension, pure NumPy core |
| `tensor-spline` | Python | `tensor_spline/low_rank.py` | `LowRankLinear` — alternative compression |
| `plato-training` | Python | `plato_training/eisenstein_encoder.py` | Eisenstein-structured encoder weights |
| `eisenstein-embed` | Python | `eisenstein_embed/eisenstein_quantize.py` | `SplineLinearQuantizer` — embedding compression |
| `polyformalism-thinking` | Markdown | `research/SPLINE-ANCHOR-PRINCIPLE.md`, `research/MUSIC-THEORY-SYSTEM.md` | Theoretical foundations |

**Control Surface:** Number of control points (typically 16–64) determines compression ratio. `compression_ratio = dense_params / spline_params`.

---

## 11. Triplet Mining (Anchor/Positive/Negative Embedding Training)

**Definition:** Contrastive learning technique that mines (anchor, positive, negative) triplets from data. Anchor and positive share a domain; negative comes from a different domain. Used to train embedding spaces where similar items cluster.

| Repo | Language | Key Files | Control Surface |
|------|----------|-----------|-----------------|
| `triplet-miner` | Python | `triplet_miner/triplet.py`, `triplet_miner/git_miner.py`, `triplet_miner/strategies.py`, `triplet_miner/filters.py`, `triplet_miner/exporters.py` | `Triplet` dataclass, `mine_triplets()` |
| `plato-training` | Python | `plato_training/triplet_miner.py` | Fleet git history mining |
| `plato-training` | Python | `train_eisenstein_benchmark.py`, `train_eisenstein_v2.py`, `train_eisenstein_v3.py` | Training scripts |
| `neural-plato` | Python | `experiments/learned_projections.py` | Learned projection experiments |
| `sunset-ecosystem` | Python | `nerve/fiber.py` | Triplet-like contrast in nerve fibers |

**Control Surface:** Margin: `sim(anchor, positive) − sim(anchor, negative) > margin`. Default margin = 0.1–0.5.

---

## 12. Deadband (Signal Tolerance, Noise Filtering)

**Definition:** A tolerance band around a signal value within which changes are ignored. Prevents noise from triggering unnecessary updates. Implemented as an exponential-decay funnel that tightens over time.

| Repo | Language | Key Files | Control Surface |
|------|----------|-----------|-----------------|
| `deadband-rs` | Rust | `target/package/deadband-0.1.0/src/main.rs`, `eisenstein.rs`, `lib.rs` | Rust deadband core |
| `deadband-python` | Python | `deadband_python/` | Python deadband |
| `deadband-zig` | Zig | — | Zig deadband |
| `deadband-constrained` | Rust | `vedic-lang/core/src/moolsutra/std_simant.rs` | Deadband in Vedic DSL |
| `constraint-theory-py` | Python | `constraint_theory/temporal.py` | `deadband_funnel()`, `FunnelPhase`, `TemporalAgent` |
| `eisenstein-embed` | Python | `eisenstein_embed/deadband_cache.py`, `eisenstein_embed/deadband_rust.py` | `DeadbandCache` (threshold = 0.90) |
| `signal-chain` | Markdown | `papers/DEADBAND-VOLUME-PEDAL.md`, `papers/SIGNAL-CHAIN.md` | Theory and application papers |
| `sunset-ecosystem` | Python | `nerve/fiber.py` | Deadband in nerve fibers |

**Cross-language equivalence:** Deadband appears in Rust (`deadband-rs`), Python (`deadband-python`, `constraint-theory-py`), Zig (`deadband-zig`), and Rust DSL (`deadband-constrained/vedic-lang`).

---

## Dependency Graph

```
                    ┌─────────────────────┐
                    │  Eisenstein Integers │ ← foundational lattice
                    └──────────┬──────────┘
                               │
              ┌────────────────┼────────────────┐
              │                │                │
    ┌─────────▼──────┐  ┌─────▼──────┐  ┌──────▼────────┐
    │ Laman Rigidity │  │  Pythag48  │  │ Deadband      │
    └────┬───────────┘  └─────┬──────┘  └──────┬────────┘
         │                    │                │
    ┌────▼───────────┐  ┌────▼──────────┐     │
    │ H1 Cohomology  │  │ Holonomy      │     │
    │ (Emergence)    │  │ Consensus     │     │
    └────┬───────────┘  └────┬──────────┘     │
         │                    │                │
    ┌────▼────────────────────▼──────┐         │
    │   Constraint Theory            │◄────────┘
    │   (rigidity, DoF, flexibility) │
    └────┬──────────┬────────────────┘
         │          │
    ┌────▼────┐  ┌──▼──────────────┐
    │ Spline  │  │ Flux Genome     │
    │ Interp  │  │ (protein evol.) │
    └────┬────┘  └─────────────────┘
         │
    ┌────▼──────────────┐
    │ Triplet Mining    │ ← uses spline embeddings
    └───────────────────┘
         │
    ┌────▼──────────────┐
    │ Nerve Adaptation  │ ← uses deadband + tripartite
    └────┬──────────────┘
         │
    ┌────▼──────────────┐
    │ Tripartite Arch   │ ← ethos/pathos/logos
    └───────────────────┘
```

**Key insight:** Eisenstein Integers are the bedrock — almost everything builds on hexagonal lattice arithmetic. Constraint Theory is the central hub connecting pure math to engineering.

---

## Complexity Ladder (Simplest → Most Complex)

| Rank | Concept | Why |
|------|---------|-----|
| 1 | **Deadband** | Simple tolerance band. Anyone who's used a thermostat gets it. |
| 2 | **Tripartite Architecture** | Product of three scores. Intuitive metaphor (ethos/pathos/logos). |
| 3 | **Nerve Adaptation** | "Shoe metaphor" — repeated exposure dampens response. |
| 4 | **Triplet Mining** | Standard ML technique. Anchor/positive/negative is well-documented. |
| 5 | **Pythagorean48** | 48 directions, log₂ info theory. Requires basic information theory. |
| 6 | **Flux Genome** | Biological metaphor adds complexity but core is gene expression. |
| 7 | **Spline Interpolation** | Requires understanding of lattice geometry + neural network weights. |
| 8 | **Eisenstein Integers** | Complex arithmetic, covering radius, lattice snapping. |
| 9 | **Laman Rigidity** | Graph theory + counting edges + combinatorial proofs. |
| 10 | **Constraint Theory** | Unifying framework spanning all of the above. |
| 11 | **Holonomy Consensus** | Matrix products around cycles, GL(9), geometric consistency. |
| 12 | **H1 Cohomology** | Algebraic topology. Sheaf cohomology is graduate-level mathematics. |

---

## Concept × Repo Matrix

Repos shown only if they implement 2+ concepts.

| Repo | Lam | Eis | P48 | H1 | Hol | CT | Flux | Nerve | TriP | Spl | TriM | Dead |
|------|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| `holonomy-consensus` | ● | | ● | ● | ● | | | | | | | |
| `constraint-theory-py` | ● | ● | | | | ● | | | | | | ● |
| `constraint-theory-math` | ● | ● | ● | | | ● | | | | | | |
| `constraint-theory-rust-python` | | | | | | ● | | | | | | |
| `constraint-theory-engine-cpp-lua` | | | | | | ● | | | | | | |
| `constraint-theory-llvm` | | | | | | ● | | | | | | |
| `constraint-theory-ecosystem` | | | | | | ● | ● | | | | | |
| `eisenstein` | | ● | | | | | | | | | | |
| `eisenstein-embed` | | ● | | | | | | | | ● | | ● |
| `tensor-spline` | | ● | | | | | | | | ● | | |
| `plato-training` | | ● | | | | | | | | ● | ● | |
| `triplet-miner` | | | | | | | | | | | ● | |
| `sunset-ecosystem` | | | | | | | | ● | ● | | | ● |
| `deadband-rs` | | ● | | | | | | | | | | ● |
| `deadband-python` | | | | | | | | | | | | ● |
| `signal-chain` | | | | | | | | | | | | ● |
| `swarm-rooms` | | ● | | | | | | | | | | |
| `flux-genome-py` | | | | | | | ● | | | | | |
| `constraint-theory-math` | ● | ● | ● | ● | | ● | | | | | | |

**Legend:** ● = concept implemented/referenced in repo

### Concept Clusters

- **Lattice Math Cluster:** `eisenstein` + `constraint-theory-py` + `constraint-theory-math` + `deadband-rs` + `swarm-rooms` + `tensor-spline` + `plato-training` + `eisenstein-embed`
- **Consensus Cluster:** `holonomy-consensus` + `pbft-rust` + `holonomy-bounded`
- **Constraint Theory Cluster:** 8+ `constraint-theory-*` repos (Python, Rust, C++, LLVM, MLIR, Mojo, CUDA, Math)
- **ML/AI Cluster:** `triplet-miner` + `plato-training` + `eisenstein-embed` + `tensor-spline` + `neural-plato`
- **Agent Lifecycle Cluster:** `sunset-ecosystem` + `sana-wm` + `training-throttle`
- **Systems Cluster:** `signal-chain` + `deadband-rs` + `deadband-python` + `deadband-zig` + `deadband-constrained`
