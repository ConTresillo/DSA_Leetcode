## **Unit 1: Hashing Fundamentals (Existence & Frequency)**

### **Modules in this Unit**

- **Module 1.1:** Presence Checking
    
    - Submodule 1.1.1: Set-based Existence
        
- **Module 1.2:** Counting & Frequency
    
    - Submodule 1.2.1: Frequency Maps
        

---

## 🧩 **Submodule 1.1.1: Set-based Existence**

### 🟢 1. Mental Model (What it really is)

A **set** is a _memory of what has been seen_, not a container of values.

Correct model:

> “I trade memory to avoid re-checking the past.”

You are encoding _knowledge_ (“seen / not seen”), not structure.

---

### 🔵 2. Why This Exists

Before sets:

- Existence checks → nested scans
    
- Duplicate detection → O(n²)
    
- No early termination
    

What sets fix:

- Repeated work
    
- Redundant comparisons
    
- Accidental quadratic behavior
    

Ignoring this in real systems causes unnecessary re-validation and latency blowups.

---

### 🟣 3. Core Building Blocks (Machine View)

- **Universe of items**: values, states, identifiers
    
- **Seen memory**: hash-backed membership table
    
- **Operations**:
    
    - Insert → remember
        
    - Lookup → have I seen this?
        
- **Assumption**: average-case constant time
    

No order. No counts. Only existence.

---

### 🧪 4. How It Behaves in the Wild

**Normal case**

- One pass
    
- Early exit on detection
    
- Stable performance
    

**Edge cases**

- Empty input → no-op
    
- Single element → trivial
    
- High collision domains → degraded performance
    

**Failure modes**

- Using a set when order matters
    
- Using a set when counts matter
    
- Inserting before checking
    

**Trade-off**

- Time ↓
    
- Memory ↑
    

---

### 🛠️ 5. Real-World Usage Patterns

Where it appears:

- Deduplication pipelines
    
- Cycle detection
    
- Authorization checks
    
- Guarding expensive operations
    

Misuse:

- Replacing simpler boolean arrays
    
- Using sets where sorting suffices
    
- Defaulting to hashing without constraints analysis
    

Expert usage:

- Early termination
    
- State-space pruning
    
- Cheap guards before costly work
    

---

### 🔑 6. Key Invariant (Non-Negotiable)

**Invariant:**

> _At any point during traversal, the set contains **exactly** the elements that have appeared before the current position._

Implications:

- Lookup must happen **before** insertion.
    
- Order of traversal does not affect correctness.
    
- The invariant is monotonic: the set only grows.
    

If this invariant is broken:

- False negatives (missed duplicates)
    
- Logic becomes order-dependent
    
- Early termination becomes unsound
    

This invariant is the _proof_ that one pass is sufficient.

---

## 🚧 **MINI PROJECT GATE**

### 🔨 Mini Project: Duplicate Sentinel

**Goal**

- Detect whether an input stream contains _any_ duplicate.
    

**Constraints**

- One pass only
    
- No sorting
    
- No nested loops
    
- Stop immediately when a duplicate is found
    

**Input**

- A list / stream of integers or strings
    

**Output**

- Boolean: duplicate exists or not
    

**Proof of learning**

- Explain:
    
    - Why order doesn’t matter
        
    - Why check-before-insert is mandatory
        
    - Why this fails if counts are required
        

⛔ Do not optimize further.

---

When ready:

- Explain the invariant in your own words.
    
- Describe one scenario where this invariant would not hold.
    

Then we proceed.


## 🧩 **Submodule 1.2.1: Frequency Maps**

---

### 🟢 1. Mental Model (What it really is)

A **frequency map** is not “counting for convenience.”  
It is a **state tracker**.

Correct mental model:

> “I am tracking how the state of the system evolves as elements arrive.”

Each key represents an identity.  
Each value represents **how many times that identity has been observed so far**.

A set answers _“have I seen this?”_  
A frequency map answers _“how many times have I seen this?”_

That difference is fundamental.

---

### 🔵 2. Why This Exists

Before frequency maps:

- Problems were incorrectly solved with sets
    
- Multiplicity information was lost
    
- Equality checks failed for reordered data
    

What broke:

- Anagram detection
    
- Inventory reconciliation
    
- Balance / cancellation logic
    

What frequency maps fix:

- Loss of count information
    
- Order dependence
    
- Multi-pass hacks
    

If you ignore this in real systems:

- You mis-handle duplicates
    
- You accept invalid states
    
- You silently produce wrong answers
    

---

### 🟣 3. Core Building Blocks (Machine View)

- **Key**: identity (number, char, string, state)
    
- **Value**: count (non-negative integer)
    
- **Operations**:
    
    - Increment → observation
        
    - Decrement → consumption / cancellation
        
    - Lookup → current state
        
- **Invariant expectation**: counts reflect history so far
    

This is not storage.  
This is **state accounting**.

---

### 🧪 4. How It Behaves in the Wild

**Normal case**

- Single pass
    
- Increment on read
    
- Optional decrement in paired logic
    

**Edge cases**

- Missing keys → implicit zero
    
- Count dropping to zero → key may be removed
    
- Large domains → memory growth
    

**Failure modes**

- Using a set when counts matter
    
- Forgetting to decrement
    
- Allowing negative counts without meaning
    
- Comparing maps incorrectly
    

**Trade-offs**

- More memory than sets
    
- More logic than existence checks
    
- Much higher expressive power
    

---

### 🛠️ 5. Real-World Usage Patterns

Where it appears:

- Anagram and permutation checks
    
- Rate limiting
    
- Inventory systems
    
- Log aggregation
    
- Sliding window problems
    

Common misuse:

- Using frequency maps when a fixed-size array suffices
    
- Forgetting to clean zero-count keys
    
- Overengineering simple existence checks
    

Expert usage:

- Difference maps (increment one side, decrement the other)
    
- Early invalidation when counts go negative
    
- Maintaining invariants instead of post-checks
    

---

### 🔑 6. Key Invariant (Non-Negotiable)

**Invariant:**

> _At any point, the frequency map exactly represents the counts of all elements seen so far (or the net difference between two sequences)._

Implications:

- Increment and decrement must be symmetric.
    
- Zero has semantic meaning (balanced state).
    
- Negative counts indicate invalid or impossible states (unless explicitly allowed).
    

If this invariant is broken:

- Equality checks lie
    
- Anagram logic fails
    
- Sliding windows drift silently
    

This invariant is the **proof of correctness**.

---

## 🚧 **MINI PROJECT GATE**

### 🔨 Mini Project: Multiset Equality Checker

**Goal**

- Determine whether two inputs represent the same multiset.
    

**Constraints**

- Order must not matter
    
- One pass per input
    
- No sorting
    
- Use a single frequency structure
    

**Input**

- Two strings or integer arrays
    

**Output**

- Boolean: equal multiset or not
    

**Proof of learning**

- Explain:
    
    - Why a set fails here
        
    - Why zero counts matter
        
    - Why negative counts signal failure
        

⛔ Do not optimize with arrays unless the domain is provably bounded.

---

When done:

- Explain the invariant in your own words.
    
- State one scenario where a frequency map is strictly necessary and a set is insufficient.
    

After this, **Unit 1 is complete** and we move to **Unit-level LeetCode gate**.