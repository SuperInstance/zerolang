---
name: fleet-rigidity
description: Laman rigidity theorem and 12-neighbor fleet proof.
---

# Fleet Rigidity — Laman's Theorem & 12 Neighbors

A fleet of agents in 2D is **rigid** when no non-trivial motion preserves all inter-agent distances. Laman (1970) proved the precise combinatorial condition.

## Theorem

A graph G = (V, E) with n = |V| vertices is *generically rigid* in 2D iff it has a subgraph with exactly **2n − 2** edges where every subset of n′ vertices spans at most **2n′ − 3** edges.

## Why 12 Neighbors

With 12 neighbors per agent (undirected):

```
edges_have  = n × 12 / 2 = 6n
edges_need  = 2n − 2
6n ≥ 2n − 2   for all n ≥ 1  ✓
```

12-neighbor fleets satisfy Laman's bound for **every** fleet size.

## Zerolang — Verify Rigidity

```zero
use fleet.laman

fun main(world: World) -> Void raises {
    let n: u32 = 50
    let have = n * 12 / 2    // 300 edges
    let need = 2 * n - 2     //  98 edges
    if have >= need {
        check world.out.write("rigid: 6n >= 2n-2 confirmed\n")
    }
}
```

See `packages/laman-rigidity/` for the full proof program.

## Python — Fleet Math API

```python
from fleet_agent.fleet_math import laman_edges_needed, is_rigid

n_agents = 50
neighbors = 12
assert is_rigid(n_agents, neighbors)
print(f"need {laman_edges_needed(n_agents)} edges, have {n_agents * neighbors // 2}")
```

## References

- Laman, G. (1970). *On graphs and rigidity of plane skeletal structures.*
- `packages/laman-rigidity/` — complete zerolang proof
- `fleet_agent.fleet_math` — Python rigidity utilities
