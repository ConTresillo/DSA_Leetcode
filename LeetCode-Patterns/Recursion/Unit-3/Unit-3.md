
---

# 🧱 UNIT 3 — Decision & Enumeration Patterns

## 📦 Module 3.1 — Choice-Based Recursion

### 🧩 Submodule 3.1.1 — **Pick / Not Pick Pattern**

---

## 🟢 1. Mental Model

This pattern is **not** about recursion.  
It is **not** about trees.  
It is **not** about generating combinations.

It is about **forced decisions**.

You are standing at position `i` in a sequence.  
The problem **forces** you to answer exactly one question:

> “Do I include this element, or do I exclude it?”

There is no third option.  
There is no skipping ahead.  
There is no rearranging order.

The recursion does not _search_ for solutions.  
It **enumerates consequences** of decisions already imposed by the problem.

Think of it as a **decision log**, not a computation.

Each recursive call represents:

- “Given all decisions so far, what happens next?”
    

Once you see it this way, the recursion tree stops feeling optional.  
It becomes **inevitable**.

---

## 🔵 2. Why This Exists

This pattern exists because **some problems do not allow creativity**.

They look flexible, but they aren’t.

Problems like:

- subsequences
    
- subsets
    
- include/exclude sums
    
- yes/no selection constraints
    

already define the structure of the solution space.

Before this pattern, people tried:

- nested loops
    
- ad-hoc backtracking
    
- generating then filtering
    
- bitmasks without understanding
    

Those approaches fail because:

- they ignore decision order
    
- they over-generate
    
- they mix enumeration with validation
    

Pick / Not Pick exists to **respect the problem’s structure instead of fighting it**.

Ignoring this pattern leads to:

- duplicated results
    
- missing paths
    
- fragile code that only works for small inputs
    

---

## 🟣 3. Core Building Blocks

No syntax yet. Only roles.

- **Position**
    
    - Where you are in the sequence
        
    - Time-like, not spatial
        
- **Decision**
    
    - Binary: include or exclude
        
    - Must be taken for every element
        
- **State**
    
    - Accumulated effect of past decisions
        
    - Often implicit (path, sum, string, list)
        
- **Termination**
    
    - Reached end of sequence
        
    - A full decision history now exists
        

Key point:  
The pattern does **not** care what the state is.  
It only cares that **every position forces a decision**.

---

## 🧪 4. How It Behaves in the Wild

### Normal behavior

- Depth = number of elements
    
- Branching factor = 2
    
- Every leaf is meaningful
    
- No pruning by default
    

### Edge behavior

- Empty input still has **one valid outcome**
    
- Single element → two outcomes
    
- Large inputs explode exponentially (by design)
    

### Failure modes

- Introducing loops inside recursion
    
- Skipping indices
    
- Reordering elements
    
- Treating results as combinations instead of decision histories
    

If any of those happen, you are no longer using this pattern — even if recursion exists.

---

## 🛠️ 5. Real-World Usage Patterns

This pattern appears in production more than people admit:

- Feature flags (enable / disable decisions)
    
- Permission matrices (allow / deny per rule)
    
- Configuration toggles
    
- Rule engines with binary gates
    
- Exhaustive testing of option combinations
    

Professional misuse usually looks like:

- premature pruning
    
- clever but brittle optimizations
    
- collapsing decisions into loops
    

Expert usage looks boring:

- clean recursion
    
- explicit decisions
    
- no cleverness
    

That’s a feature, not a flaw.

---
