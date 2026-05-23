# Performance Optimization Task Layer

Use this task layer when improving latency, throughput, memory use, CPU use, database load, network usage, cost, scalability, or resource contention.

## Required Behavior

- Establish the performance target or bottleneck before optimizing when feasible.
- Prefer measurement-driven changes over speculative optimization.
- Preserve correctness, security, and reliability.
- Consider whether the change shifts cost or load to another system.
- Avoid trading maintainability for performance unless the gain is material and documented.
- Keep optimizations scoped and reversible.
- Watch for cache invalidation, stale data, concurrency, backpressure, and retry amplification risks.

## Investigation Checklist

- What metric is being improved?
- What baseline or evidence identifies the bottleneck?
- What path is latency-sensitive or resource-sensitive?
- What data size, concurrency level, or traffic pattern matters?
- Does the change affect consistency, availability, cost, or operational complexity?

## Validation Expectations

- Compare before/after metrics when feasible.
- Use existing benchmarks, load tests, profiling, query plans, or targeted timing checks where available.
- Validate correctness on the optimized path and relevant edge cases.
- If measurement is not available, explain the reasoning and residual uncertainty.

## Output Expectations

- Bottleneck or hypothesis
- Optimization summary
- Before/after evidence, if available
- Correctness validation
- Trade-offs and residual risk
