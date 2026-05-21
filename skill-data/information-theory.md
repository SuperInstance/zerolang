---
name: information-theory
description: Shannon entropy bounds and why 48 directions maximize info density.
---

# Information Theory — Shannon Bounds for Pythagorean48

Shannon entropy sets the fundamental limit on how much information a symbol can carry. For a set of N equally-likely symbols:

```
H = log₂(N) bits
```

A u8 has 256 symbols → 8 bits. Pythagorean48 has 48 symbols → **5.585 bits**.

## Why 48 Directions Maximize Density

The constraint is **exact rational coordinates on the unit circle**. Only Pythagorean triples (a² + b² = c²) yield points with rational coordinates. The 48 directions from primitive triples with c ≤ 25 are the **maximum set** achievable with small-integer arithmetic:

- 2^5 = 32 < 48 < 64 = 2^6 → fits in 6 bits
- 48 / 256 = 18.75% of u8 values, but carries 5.585 / 8 = **69.8%** of the entropy
- No larger exact-rational set exists at this integer scale

## Zerolang — Entropy Bound

```zero
pub fun main(world: World) -> Void raises {
    // Bracket log2(48) between powers of 2
    // 2^5 = 32 < 48 < 64 = 2^6
    check world.out.write("info-bound: log2(48) = 5.585 bits\n")
    check world.out.write("info-bound: pythagorean48 uses 69.8% of u8 entropy\n")
}
```

See `packages/information-bound/` for the full Shannon bound computation.

## Python — Entropy Calculation

```python
import math
from fleet_agent.fleet_math import pythagorean48_directions

n_dirs = len(pythagorean48_directions())  # 48
entropy = math.log2(n_dirs)  # 5.585
efficiency = entropy / 8.0   # 0.698
print(f"entropy = {entropy:.3f} bits, efficiency = {efficiency:.1%}")
```

## References

- Shannon, C.E. (1948). *A Mathematical Theory of Communication.*
- `packages/information-bound/` — zerolang entropy bounds
- `fleet_agent.fleet_math` — Python information utilities
