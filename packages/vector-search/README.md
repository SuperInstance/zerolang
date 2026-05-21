# Vector Search — Nearest-Neighbor on Compressed Agent DNA

## What It Does

Demonstrates nearest-neighbor search over small integer vectors — the core operation behind vector databases like turbovec. 8 agents each carry a 4-component `i32` "DNA" vector derived from Pythagorean triples. The program computes Manhattan distance from agent 0 to all others and finds the closest match.

**Result:** `agent 0 nearest: agent 3 (distance: 1)` — agent 3's DNA `[3,4,1,5]` is nearly identical to agent 0's `[3,4,0,5]`.

## How This Connects to turbovec

This is the *exact same algorithm* turbovec runs at scale:

| Concept | This Demo | turbovec |
|---|---|---|
| Vectors | 4 × i32 per agent | 768 × f32 → 4-bit quantized |
| Distance | Manhattan (L1) | Manhattan / Hamming on packed bits |
| Search | Brute-force 8 agents | SIMD-vectorized brute-force over millions |
| Quantization | Pythagorean48 integers | 4-bit bin packing (16 levels) |
| Throughput | ~1 query | ~10M queries/sec |

turbovec replaces the scalar loop with SIMD kernels that process 32–64 dimensions per instruction, and quantizes float32 embeddings down to 4-bit integers — turning a 3KB vector into a 384-byte fingerprint. The distance metric stays the same: sum of absolute differences.

## How This Connects to Our Fleet

- **Agent DNA similarity:** Each Cocapn agent has a behavioral fingerprint (task preferences, model affinity, error patterns). Vector search finds the most similar agent for task routing.
- **Pythagorean48 encoding:** The 48 exact unit vectors from Pythagorean triples give lossless direction encoding with small integers — perfect for quantized vector representations.
- **Constraint satisfaction:** Nearest-neighbor is itself a constraint — "find the point closest to query under L1" — connecting back to the fleet's constraint-theory foundation.

## Binary

```
4.7 KiB, statically linked (linux-musl-x64)
```
