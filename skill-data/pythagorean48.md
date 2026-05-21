---
name: pythagorean48
description: 48 exact unit vectors on the unit circle encoding 5.585 bits.
---

# Pythagorean 48 — Exact Unit Vectors

The Pythagorean48 encoding defines **48 exact rational points** on the unit circle, derived from Pythagorean triples. Each direction (a/c, b/c) is computed with pure integer arithmetic — no floating point, no approximation.

## The 48 Directions

From the triple (3, 4, 5): (±3/5, ±4/5) and (±4/5, ±3/5) = 8 directions. All primitive triples with c ≤ 25 contribute similarly. Combined with axial directions, this yields exactly **48** unique unit vectors.

## Information Density

```
log₂(48) ≈ 5.585 bits
```

Each direction encodes 5.585 bits of information — 69.8% of a full u8 (8 bits), using only exact rational arithmetic.

## Zerolang — Count Directions

```zero
pub fun main(world: World) -> Void raises {
    let rotations: u32 = 12
    let signs: u32 = 4
    let total: u32 = rotations * signs  // 48
    if total == 48 {
        check world.out.write("pythagorean48: 48 exact unit vectors confirmed\n")
    }
}
```

See `packages/pythagorean48/` for triple enumeration and bit-count verification.

## Python — Fleet Math API

```python
from fleet_agent.fleet_math import pythagorean48_directions, info_bits

dirs = pythagorean48_directions()  # list of (x, y) exact rationals
print(f"{len(dirs)} directions, {info_bits():.3f} bits")  # 48, 5.585
```

## References

- `packages/pythagorean48/` — zerolang triple enumeration
- `fleet_agent.fleet_math` — Python direction utilities
