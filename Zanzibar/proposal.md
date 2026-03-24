# Proposal: Adopt Leopard-Style Indexing Instead of Relationship Flattening in OpenFGA

## Executive Summary

To improve authorization performance in OpenFGA, we are considering two approaches:

1. **Flatten relationships** (e.g., replace `group#member` with direct `group → user` checks)
2. **Adopt a Leopard-style indexing approach** (inspired by Google Zanzibar)

While flattening may offer short-term performance gains, it introduces long-term scalability, consistency, and maintenance risks.

A Leopard-like indexing system provides **high performance without sacrificing correctness or flexibility**, making it the better long-term solution.

---

## Current Challenge

OpenFGA evaluates relationships dynamically (e.g., `user ∈ group#member`), which can become expensive at scale due to:

- Recursive relationship checks  
- High cardinality (large groups)  
- Repeated graph traversals  

---

## Option 1: Flatten Relationships (Shortcut Approach)

### Approach

Break `group#member` into:
- Direct `group → user` relationships
- Separate `/check` calls to reconstruct logic

### Pros

- Faster lookup (reduced graph traversal)
- Simpler query path

### Cons

#### ❌ Loss of Source of Truth

- Relationships become duplicated across multiple places
- Increased risk of **data inconsistency**

#### ❌ Write Amplification

- Group updates require updating **all derived user relationships**
- Cost grows with group size

#### ❌ Complex Maintenance Logic

- Requires custom sync mechanisms
- Harder to debug and reason about

#### ❌ Limited Flexibility

- Breaks composability (e.g., nested groups)
- Harder to support future authorization models

---

## Option 2: Leopard-Style Indexing (Recommended)

### Concept

Inspired by Google Zanzibar’s **Leopard indexing system**, this approach:

- Precomputes and indexes relationship edges
- Optimizes graph traversal using efficient lookup structures
- Preserves the **original logical model** (no flattening)

### Pros

#### ✅ Performance Without Compromise

- Fast lookups via indexed relationships
- Avoids repeated deep traversal

#### ✅ Strong Consistency

- Maintains a **single source of truth**
- No duplicated or derived data

#### ✅ Scales with Complexity

- Supports:
  - Nested groups  
  - Large organizations  
  - Complex authorization policies  

#### ✅ Lower Operational Risk

- No need for synchronization jobs
- Fewer edge cases and bugs

---

## Comparison

| Criteria            | Flatten Relationships | Leopard-Style Indexing |
|--------------------|----------------------|------------------------|
| Read Performance   | ✅ Fast (initially)  | ✅ Fast                |
| Write Complexity   | ❌ High              | ✅ Low                 |
| Consistency        | ❌ Risky             | ✅ Strong              |
| Scalability        | ❌ Limited           | ✅ High                |
| Maintainability    | ❌ Complex           | ✅ Clean               |
| Flexibility        | ❌ Constrained       | ✅ Extensible          |

---

## Business Impact

### Flatten Relationships

- Short-term performance improvement
- Increasing engineering overhead
- Higher risk of authorization bugs (**security concern**)

### Leopard-Style Indexing

- Sustainable performance at scale
- Lower long-term engineering cost
- More robust and future-proof system

---

## Recommendation

We should **avoid flattening relationships** and instead invest in a **Leopard-style indexing strategy** because:

- It preserves the integrity of the authorization model  
- It scales with system growth  
- It reduces long-term engineering and operational risk  

---

## Suggested Next Steps

1. Prototype an indexing layer for high-cardinality relationships  
2. Benchmark against current OpenFGA performance  
3. Introduce indexed lookups incrementally for critical paths  

---

## Closing

Flattening relationships is a shortcut that trades **short-term speed for long-term complexity and risk**.

Leopard-style indexing delivers **both performance and correctness**, aligning with scalable system design principles.
