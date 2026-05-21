---
name: eisenstein-lattice
description: Eisenstein integers, hexagonal lattice norm, and densest packing.
---

# Eisenstein Integers — Hexagonal Lattice

Eisenstein integers **z = a + bω** (where ω = e^{2πi/3}) form a hexagonal lattice in the complex plane — the **densest possible** 2D packing (Thue's theorem, 1892).

## Norm Formula

```
|a + bω|² = a² + ab + b²
```

Every Eisenstein integer has a unique factorization. The norm is always a non-negative integer, and the lattice tiles the plane with equilateral triangles.

## Densest Packing (Thue's Theorem)

No arrangement of equal non-overlapping circles in the plane has density greater than π / (2√3) ≈ 0.9069. The hexagonal lattice achieves this bound.

## Zerolang — Compute Eisenstein Norm

```zero
fun eisenstein_norm(a: i32, b: i32) -> i32 {
    return a * a + a * b + b * b
}

pub fun main(world: World) -> Void raises {
    let n = eisenstein_norm(3, 1)  // 13
    if n == 13 {
        check world.out.write("eisenstein norm verified: 3² + 3×1 + 1² = 13\n")
    }
}
```

See `packages/eisenstein-snap/` for the full lattice verification.

## Python — Eisenstein Embedding

```python
from eisenstein_embed import eisenstein_norm, nearest_lattice_point

norm = eisenstein_norm(3, 1)  # 13
point = nearest_lattice_point(0.7 + 0.3j)  # snap to hexagonal grid
```

## References

- Thue, A. (1892). *Om nogle geometrisk-taltheoretiske Theoremer.*
- `packages/eisenstein-snap/` — zerolang lattice verification
- `eisenstein_embed` — Python hexagonal grid utilities
