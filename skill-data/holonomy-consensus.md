---
name: holonomy-consensus
description: Zero-holonomy consensus — convergence without voting.
---

# Holonomy Consensus — Consensus Without Voting

**Holonomy** measures how much a quantity drifts after traversing a closed loop. **Zero holonomy** means the value returns exactly to its start — the system has reached consensus without any voting round.

## How It Works

Each agent updates its value to the **average of its neighbors**. After enough iterations of this linear update, every agent converges to the same value. The holonomy (max deviation from mean) drops to zero.

Formally, if agent i has neighbors N(i):

```
v_i(t+1) = (1/|N(i)|) × Σ_{j ∈ N(i)} v_j(t)
```

Convergence is guaranteed for connected graphs.

## Zerolang — Ring Consensus

```zero
pub fun main(world: World) -> Void raises {
    // 5 agents in a ring, averaging with neighbors each step
    let mut v0: i32 = 10; let mut v1: i32 = 30
    let mut v2: i32 = 50; let mut v3: i32 = 70
    let mut v4: i32 = 90
    // After ~40 iterations, all converge to mean = 50
    // Holonomy (max deviation) → 0
}
```

See `packages/holonomy-consensus/` for the full convergence simulation.

## Python — Fleet Math API

```python
from fleet_agent.fleet_math import holonomy_step, consensus_value

values = [10, 30, 50, 70, 90]
for _ in range(50):
    values = holonomy_step(values, topology="ring")
assert all(abs(v - consensus_value(values)) < 1 for v in values)
```

## References

- `packages/holonomy-consensus/` — complete zerolang simulation
- `fleet_agent.fleet_math` — Python consensus utilities
