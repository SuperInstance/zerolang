# Zerolang Concept Packages

Self-contained mathematical proofs compiled to native executables. Each package is one file (`main.0`) that proves a theorem when run.

## Packages

| Package | Concept | Size | Teaches | Run |
|---|---|---|---|---|
| [laman-rigidity](../packages/laman-rigidity/) | Laman's theorem (1970) | 614 B | 12 neighbors guarantees fleet rigidity: 6n ≥ 2n-2 | `zero run packages/laman-rigidity/main.0` |
| [eisenstein-snap](../packages/eisenstein-snap/) | Eisenstein integers | 715 B | Hexagonal lattice norm: a² + ab + b² | `zero run packages/eisenstein-snap/main.0` |
| [information-bound](../packages/information-bound/) | Shannon entropy | 784 B | log₂(48) ≈ 5.585 bits, 69.8% of u8 capacity | `zero run packages/information-bound/main.0` |
| [constraint-solve](../packages/constraint-solve/) | Triangle inequality | 989 B | Bar-joint constraint satisfaction: a+b > c | `zero run packages/constraint-solve/main.0` |
| [pythagorean48](../packages/pythagorean48/) | Pythagorean triples | 1,068 B | 48 exact unit vectors from integer triples | `zero run packages/pythagorean48/main.0` |
| [fleet-edges](../packages/fleet-edges/) | Graph edge counting | 1,438 B | Edge enumeration for fleet rigidity verification | `zero run packages/fleet-edges/main.0` |
| [holonomy-consensus](../packages/holonomy-consensus/) | Holonomy on cycles | 1,452 B | Ring consensus via averaging, zero holonomy = convergence | `zero run packages/holonomy-consensus/main.0` |
| [spline-interpolation](../packages/spline-interpolation/) | Linear interpolation | 1,470 B | Piecewise linear interpolation with fixed-point math | `zero run packages/spline-interpolation/main.0` |
| [triplet-loss](../packages/triplet-loss/) | Metric learning | 2,265 B | Triplet mining loss with Newton's method sqrt | `zero run packages/triplet-loss/main.0` |
| [deadband-filter](../packages/deadband-filter/) | Signal processing | 3,238 B | Noise rejection: suppress changes below threshold | `zero run packages/deadband-filter/main.0` |

## Descriptions

**laman-rigidity** — Proves that any fleet with 12 neighbors per agent satisfies Laman's rigidity condition (2n-2 edges minimum) for all fleet sizes n ≥ 1. Loops from n=3 to n=100, checking `6n ≥ 2n-2` at each step. Python equivalent: `fleet_agent.fleet_math.check_rigidity`.

**eisenstein-snap** — Computes the Eisenstein norm |a + bω|² = a² + ab + b² for lattice points (3,1) and (2,2), confirming they produce integer norms on the hexagonal lattice. Python equivalent: `eisenstein_embed.eisenstein_norm`.

**information-bound** — Brackets log₂(48) between 2⁵=32 and 2⁶=64, establishing that the 48 Pythagorean directions encode 5.585 bits — 69.8% information efficiency within a u8. Related to: `pythagorean48`.

**constraint-solve** — Verifies the triangle inequality (a+b > c) for bar-joint frameworks. Tests a 3-4-5 triangle (satisfiable), a 1-2-3 triangle (degenerate), and a 2-3-4 triangle (satisfiable). The simplest possible constraint system.

**pythagorean48** — Enumerates Pythagorean triples (a² + b² = c²) with c ≤ 25 and counts unique directions. Each coprime pair produces 8 directions via sign/axis combinations, totaling 48 exact unit vectors.

**fleet-edges** — Computes edge counts (n×k/2) and Laman thresholds (2n-2) for fleet sizes n = 2, 5, 10, 50, 100 with 12 neighbors. Prints verification for each. Companion to `laman-rigidity`.

**holonomy-consensus** — Simulates 5 agents in a ring topology averaging with neighbors. After convergence, all agents hold the same value — zero holonomy means the transformation cycle returns to identity. Python equivalent: consensus algorithms in `fleet_agent`.

**spline-interpolation** — Given 4 control points at x=0,1,2,3, interpolates at x=0.5, 1.5, and 2.5 using piecewise linear interpolation. Uses fixed-point ×1000 for decimal precision.

**triplet-loss** — Computes triplet mining loss (anchor-positive-negative) using squared Euclidean distances. Demonstrates both easy triplets (loss = 0) and hard triplets (loss > 0) using Newton's method for integer square root.

**deadband-filter** — Processes 10 signal values through a deadband filter: suppress changes below 0.5 (scaled ×1000 = 500), pass everything else. Demonstrates noise rejection with 5 suppressed and 3 passed signals.

## Cross-References to Python/Rust

| Zerolang package | Python package | Rust crate |
|---|---|---|
| laman-rigidity | `fleet_agent.fleet_math.check_rigidity` | `fleet-math::rigidity` |
| eisenstein-snap | `eisenstein_embed.eisenstein_norm` | `eisenstein::norm` |
| fleet-edges | `fleet_agent.fleet_math.edge_count` | `fleet-math::edges` |
| pythagorean48 | `fleet_agent.fleet_math.pythagorean_directions` | `fleet-math::py48` |
| holonomy-consensus | `fleet_agent.consensus.ring_average` | `fleet-consensus::holonomy` |
| information-bound | `fleet_agent.fleet_math.info_bound` | `fleet-math::shannon` |
| constraint-solve | `fleet_agent.constraints.triangle_ineq` | `fleet-constraints::bar_joint` |
| spline-interpolation | `fleet_agent.math.spline` | `fleet-math::spline` |
| triplet-loss | `fleet_agent.ml.triplet_loss` | `fleet-ml::triplet` |
| deadband-filter | `fleet_agent.signal.deadband` | `fleet-signal::deadband` |

## Running All Packages

```bash
# Run all proofs
for pkg in packages/*/main.0; do
    echo "--- $(basename $(dirname $pkg)) ---"
    zero run "$pkg"
done

# Ship all binaries
for pkg in packages/*/main.0; do
    name=$(basename $(dirname $pkg))
    zero ship "$pkg" --out "dist/$name"
done
```
