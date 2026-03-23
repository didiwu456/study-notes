Good question — and the answer is that they're not directly comparable, because they're solving different sub-problems at different levels. Let me lay out the exact structure, then the complexity, then how each API maps onto each design.

The core conceptual split: Zanzibar's M2G+G2G stores **edges and computes closures at read time**. The Leopard forward closure stores **precomputed closures and does zero computation at read time**. The data structure choice (skip list vs Roaring Bitmap) is downstream of that strategic decision.**On the "more efficient than skip list" hint in the paper:** The paper acknowledges that the sorted list for G2G can be replaced with a denser structure. If group IDs are numbered sequentially, a plain **bit array** gives O(1) membership (bit index) and O(G/64) intersection instead of O(log G) and O(G) — which is essentially a Roaring Bitmap applied at the group layer. Our approach takes this one step further: instead of storing group-level sets that still require intersection at query time, we precompute the transitive closure all the way down to individual user IDs. This eliminates intersection at read time entirely.

Now the full API-by-API complexity comparison:Here is the API-by-API walkthrough:

---

### Write

**M2G+G2G:** Insert one edge into the M2G skip list for the target user, and one edge into the G2G skip list if it's a group-to-group membership. O(log N) — extremely cheap. No propagation, no precomputation.

**Roaring Bitmap:** Must propagate the change through the RDG upward, unioning the affected user IDs into every transitively dependent entry. If user:alice joins group:backend, and group:backend feeds into group:eng which feeds into 10,000 documents, those 10,000 entries all need a bitmap union operation. Each union is O(U/64) on modern SIMD hardware (~1 ms for 1M users), but F (fan-out) can be large. Write is the only place Roaring Bitmap is meaningfully slower. This is the fundamental trade-off: you pay upfront at write time to earn O(1) at read time.

---

### Check

**M2G+G2G:** Look up M2G(user) to get the set of direct groups `m`. Look up G2G(target group) to get its full transitive subgroup set `s`. Return true if `m ∩ ({target} ∪ s) ≠ ∅`. With skip lists, the intersection is O(m · log G). With a bitmap for G2G (the "more efficient" structure hinted at in the paper), this becomes O(m · G/64) — still non-trivial for large hierarchies.

**Roaring Bitmap:** Redis GET on one key, deserialize bitmap, call `bitmap.Contains(intern(user))`. The intern lookup is an O(1) HGET. The Contains call on a Roaring Bitmap is O(1) — the container for the relevant 2^16 chunk is found in a small sorted array of at most 65,536 containers, then either a direct bit test (bitmap container), binary search (array container), or linear scan of run pairs (RLE container). In practice this is a handful of nanoseconds.

---

### ListObjects

This is where M2G+G2G has no native story. To find all objects a user can access under a given relation, you would need to: (1) walk M2G+G2G to get all groups the user transitively belongs to, (2) for each group look up a separate object-to-group index, (3) union the results. Zanzibar doesn't describe this index; it would need to be an additional inverted structure. The total complexity depends heavily on how many objects each group can access.

**Roaring Bitmap:** The explicit `inv:{store}|uid|rel` SET is maintained by the Compute Tier on every write. `ListObjects` is two Redis commands: HGET for the user's intern ID, then SMEMBERS on the inverted index key. Cost is O(1) + O(result set size).

---

### ListUsers

**M2G+G2G:** Start from the target object's access group, use G2G to expand all subgroups, then for each subgroup apply the inverse of M2G to get member users. This requires either a separately maintained inverse M2G index (users per group) or a full scan of M2G. Complexity scales with the number of subgroups times the average group size.

**Roaring Bitmap:** The forward index entry `idx:{store}|object|relation` already contains the complete transitive member set as a bitmap. GET the key, deserialize the bitmap, iterate. The only cost is deserializing O(U/64) words — for a 1-million-user group, that's ~125 KB of bytes to walk through, taking microseconds.

---

### ListRelations

**M2G+G2G:** No better approach than running R separate Check calls, one per relation type defined in the authorization model. Cost scales linearly with R and the per-Check traversal cost.

**Roaring Bitmap:** Probe R forward index keys (`idx:{store}|object|rel1`, `…|rel2`, etc.) and call `bitmap.Contains` on each. Since all keys for a store share the same Redis hash tag and thus the same cluster node, this can be pipelined as a single Redis round-trip of R commands. O(R) total.

---

### The key space trade-off

The table shows M2G+G2G winning on Write and on total stored data. That stored-space win is real and significant: M2G+G2G stores O(E) edges where E is the number of direct membership tuples — compact and proportional to what you actually wrote. Roaring Bitmap stores O(O × R × U/64) — every object–relation pair gets an entry, and that entry can be hundreds of kilobytes for large organizations. For 10,000 documents, 5 relations, and 1 million users, the Roaring index is on the order of 6 GB; M2G+G2G for the same data might be a few hundred MB.

The bet in our design is that **read volume so vastly dominates write volume** in authorization systems (easily 1000:1) that trading write cost and RAM for O(1) reads is worthwhile. For systems with very frequent permission changes and relatively few `Check` calls — an unusual profile for authorization — M2G+G2G (especially with bitmap-accelerated G2G) would be the better fit.