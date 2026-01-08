**Pattern name:**  
**Isomorphic Mapping (Bijection Pattern)**

**Core invariant:**

- Each source symbol maps to **exactly one** target symbol
    
- No two source symbols map to the **same** target symbol
    
- Mapping must stay consistent across positions
    

**Typical tools:**

- One map/array for `source → target`
    
- One set or reverse map to prevent collisions
    

**Canonical problems:**

- Isomorphic Strings
    
- Word Pattern
    
- Pattern Matching with symbols
    
- Any “structure-preserving” string problem
    

**Anti-pattern (what NOT to use):**

- Frequency maps
    
- Buckets
    
- Sorting
    
- Any order-destroying technique