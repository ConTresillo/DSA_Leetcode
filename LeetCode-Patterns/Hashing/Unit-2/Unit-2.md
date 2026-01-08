## **Unit 2: Hashing with Constraints**

### **Modules in this Unit**

- **Module 2.1:** Fixed Domain Hashing
    
    - **Submodule 2.1.1:** Array / ASCII Hashing
        
- **Module 2.2:** Sliding Context Hashing
    
    - Submodule 2.2.1: Hashing in Windows
        

---

## 🧩 **Submodule 2.1.1: Array / ASCII Hashing**

### 🟢 1. Mental Model (What it really is)

Array / ASCII hashing is **not hashing** in the classical sense.

It is **direct addressing**.

Correct mental model:

> “Every possible key already has a reserved slot.”

There is:

- No compression
    
- No collision
    
- No recovery logic
    

The index _is_ the identity.

This works only because the domain is **finite, small, and known in advance**.

---

### 🔵 2. Why This Exists

Hash maps solve:

- Unknown domains
    
- Large key spaces
    

But they introduce:

- Hash computation
    
- Collision handling
    
- Memory overhead
    
- Non-deterministic constants
    

Array hashing exists to remove all of that **when the domain allows it**.

If you ignore this option:

- You overpay in memory indirection
    
- You write more code than needed
    
- You miss constant-factor optimizations that matter in tight loops
    

---

### 🟣 3. Core Building Blocks (Machine View)

- **Domain definition**  
    Example:
    
    - lowercase letters → 26
        
    - ASCII → 128 / 256
        
- **Index function**  
    Deterministic, collision-free mapping
    
- **Storage array**  
    Counts or booleans
    
- **Invariant**  
    Index `i` represents exactly one key, forever
    

Key point:

> The correctness comes from the _domain guarantee_, not the array.

---

### 🧪 4. How It Behaves in the Wild

**Normal case**

- O(1) access
    
- Predictable memory layout
    
- Extremely cache-friendly
    

**Edge cases**

- Unexpected characters → invalid index
    
- Mixed case / Unicode → domain violation
    
- Sparse usage → wasted space
    

**Failure modes**

- Using array hashing when domain is not bounded
    
- Forgetting normalization (case, offset)
    
- Assuming ASCII when input is Unicode
    

**Trade-offs**

- Speed ↑↑
    
- Simplicity ↑
    
- Flexibility ↓
    
- Domain rigidity ↑
    

---

### 🛠️ 5. Real-World Usage Patterns

Where it appears:

- Character frequency problems
    
- Protocol parsers
    
- Tokenizers
    
- Competitive programming hot paths
    

Common misuse:

- Using it for strings without validating charset
    
- Hardcoding 26 when input is not guaranteed lowercase
    
- Silently truncating invalid indices
    

Expert usage:

- Explicit domain checks
    
- Normalization as a first step
    
- Switching to hash maps when constraints weaken
    

---

### 🔑 6. Key Invariant (Non-Negotiable)

**Invariant:**

> _Each possible key maps to a unique, fixed index, and no two distinct keys ever share an index._

Implications:

- Collisions are impossible by construction
    
- Equality checks are unnecessary
    
- Any domain violation breaks correctness instantly
    

This invariant is **stronger than hash-table invariants**, but also **more fragile**.

---

## 🚧 **MINI PROJECT GATE**

### 🔨 Mini Project: Character Frequency Validator

**Goal**

- Validate whether two strings are equal under a constrained character domain.
    

**Constraints**

- Domain must be explicitly defined (e.g., lowercase `a–z`)
    
- Use an array, not a hash map
    
- Single pass per string
    

**Input**

- Two strings
    

**Output**

- Boolean: domain-valid and frequency-equal or not
    

**Proof of learning**

- Explain:
    
    - Why collisions are impossible here
        
    - What breaks if the domain changes
        
    - When you must abandon arrays for maps
        

⛔ Do not generalize beyond the stated domain.

---

**HARD STOP.**

Complete or reflect on the mini project.  
Then we proceed to **Submodule 2.2.1: Hashing in Windows**.

In direct addressing it's like a bijective fxn mapping?
one to one and onto?

Entire logic breaks if domain changes we may need to refactor it

when domain is extremely large
or they r using integers say -10^6 to 10^6 etc

Yes — and you’re thinking about it in exactly the **right mathematical terms**.  
Let’s tighten it, correct one subtle point, and then generalize.

---

## 1️⃣ Is direct addressing a bijection?

**Almost. Precise answer:**

- It **must be injective (one-to-one)**
    
- It is **not necessarily surjective (onto)**
    

### Why injective is mandatory

If two distinct keys map to the same index → collision → invariant broken.

So the mapping must satisfy:

> `key₁ ≠ key₂ ⇒ index(key₁) ≠ index(key₂)`

This is **non-negotiable**.

### Why surjective is optional

Some indices may never be used.

Example:

- Domain = lowercase letters `a–z`
    
- Input = only vowels
    

Unused indices are fine.  
So the mapping does **not** need to cover the entire array.

**Conclusion**

> Direct addressing requires an **injective mapping**, not a full bijection.

Good instinct though — most people miss this distinction.

---

## 2️⃣ “Entire logic breaks if domain changes” — correct

This is the **fragility trade-off** of direct addressing.

Your invariant is:

> “Every valid key maps to exactly one fixed index.”

If the domain expands:

- New keys have no assigned indices
    
- Old assumptions become invalid
    
- Silent bugs appear
    

That’s why array hashing is:

- Fast
    
- Simple
    
- **Brittle**
    

Refactoring is not optional — it’s required.

---

## 3️⃣ What about large integer domains (e.g. −10⁶ to 10⁶)?

This is the exact boundary case where people make mistakes.

### Domain size

```
−10⁶ to 10⁶  →  2,000,001 possible values
```

### Is direct addressing possible?

**Technically yes. Practically maybe.**

You can do:

```
index = value + 1_000_000
```

### But here’s the engineering judgment:

|Factor|Result|
|---|---|
|Memory|~2 million slots|
|Speed|Excellent|
|Cache|Questionable|
|Sparsity|Often high|
|Flexibility|Very low|

So:

- If input size is small → wasteful
    
- If constraints are tight → risky
    
- If range grows later → breaks instantly
    

This is **exactly** where hash maps become the safer abstraction.

---

## 4️⃣ Decision rule (this is the real takeaway)

Use **direct addressing** only if **all** are true:

1. Domain is **finite and known**
    
2. Domain size is **reasonably small**
    
3. Mapping is **injective**
    
4. Domain will **not evolve**
    

If **any one** fails → use `HashMap`.

This is not about cleverness.  
It’s about **maintaining invariants under change**.

---

## 5️⃣ Final mental model (lock this in)

- Direct addressing = _strong invariant, fragile system_
    
- Hash map = _weaker invariant, robust system_
    

You trade:

- raw speed  
    for
    
- adaptability and correctness under uncertainty
    

---

You’ve fully internalized **Submodule 2.1.1**.

When ready, say **“next”** and we’ll move to:

> **Submodule 2.2.1 — Hashing in Windows**

## 🧩 **Submodule 2.2.1: Hashing in Windows**

---

### 🟢 1. Mental Model (What it really is)

Window hashing is **state maintenance over a moving boundary**.

Correct mental model:

> “I am maintaining an invariant over a _changing slice_ of the input.”

You are not recomputing from scratch.  
You are **updating state incrementally** as elements **enter** and **leave** the window.

This is the key leap from Unit-1:

- Unit-1: state only grows
    
- Unit-2 (windows): state **changes in two directions**
    

---

### 🔵 2. Why This Exists

Without window hashing:

- Every subarray / substring check costs O(window size)
    
- Total complexity explodes to O(n²)
    

What breaks:

- Performance under constraints
    
- Online / streaming feasibility
    
- Real-time guarantees
    

Window hashing exists to:

- Preserve O(n) time
    
- Avoid recomputation
    
- Maintain correctness while state mutates
    

If you ignore this pattern:

- You write “almost works” code
    
- It passes small tests, TLEs on large ones
    
- Bugs appear only under edge windows
    

---

### 🟣 3. Core Building Blocks (Machine View)

- **Window boundaries**: left, right
    
- **State container**: frequency map / array
    
- **Enter operation**: add right element
    
- **Exit operation**: remove left element
    
- **Invariant**: window state reflects _exactly_ current contents
    

Critical symmetry:

> Whatever you do on entry, you must undo on exit.

Breaking this symmetry causes drift.

---

### 🧪 4. How It Behaves in the Wild

**Normal case**

- One pass
    
- Each element processed twice (enter + exit)
    
- Stable memory
    

**Edge cases**

- Window size = 0 or 1
    
- Repeated characters
    
- Rapid shrink/expand cycles
    

**Failure modes**

- Forgetting to decrement on exit
    
- Letting counts go negative silently
    
- Checking condition before state is valid
    
- Off-by-one window boundaries
    

**Trade-offs**

- Logic complexity ↑
    
- Time complexity ↓↓
    
- Debugging difficulty ↑
    

---

### 🛠️ 5. Real-World Usage Patterns

Where it appears:

- Substring problems
    
- Rate limiting (requests per window)
    
- Streaming analytics
    
- Network packet inspection
    

Common misuse:

- Rebuilding maps per window
    
- Using sets when counts matter
    
- Checking conditions before window stabilization
    

Expert usage:

- Difference maps
    
- Early invalidation
    
- Fixed-size windows with strict invariants
    

---

### 🔑 6. Key Invariant (Non-Negotiable)

**Invariant:**

> _At all times, the hash state represents exactly the elements currently inside the window — no more, no less._

Implications:

- Every right++ must be paired with a future left++
    
- Counts reaching zero must be handled deliberately
    
- Any stale entry breaks correctness
    

This invariant is **fragile but powerful**.

---

## 🚧 **MINI PROJECT GATE**

### 🔨 Mini Project: Fixed-Window Anagram Detector

**Goal**

- Determine if any substring of length `k` is an anagram of a given pattern.
    

**Constraints**

- Fixed window size
    
- Use hashing (array or map depending on domain)
    
- One pass
    
- No sorting
    

**Input**

- String `s`
    
- Pattern `p`
    

**Output**

- Boolean: does such a window exist
    

**Proof of learning**

- Explain:
    
    - What your window invariant is
        
    - Why decrementing matters
        
    - What breaks if exit logic is wrong
        

⛔ Do not generalize to variable window sizes yet.

---

**HARD STOP.**

Complete or reflect on the mini project.  
After this, **Unit-2 is complete**, and we move to **Unit-2 LeetCode gate**.

### Answer

say,
string `s` is of length `n` and
pattern `p` is of length `k`
it should be `k` otherwise tht can't be an anagram

`n` >= `k` otherwise its useless
if lesser thts a false default

`O(k)` for constructing window
loop:
	then u observe()
	then as u move ahead with `i=k` u must remove `i-k`
	loop until `i == n`

Your reasoning is **structurally correct**. I’ll restate it in a precise, invariant-driven form and fix one subtle indexing ambiguity.

---

## 1️⃣ Preconditions (these are necessary, not optional)

- Window length **must be exactly `k`**  
    → An anagram preserves length
    
- If `n < k`  
    → **Immediate false** (no window can exist)
    

These are **logical guards**, not optimizations.

---

## 2️⃣ Window construction cost

- Initial window setup: **O(k)**
    
- This cost is paid **once**
    
- Everything after must be **O(1) per step**
    

If you rebuild the window again, the entire approach collapses.

---

## 3️⃣ Correct iteration model (tightened)

Let’s index carefully.

### Step 1: Build initial window

- Window covers indices `[0 … k-1]`
    
- State now represents exactly `s[0:k]`
    

### Step 2: Sliding phase

For `i` from `k` to `n-1` (inclusive):

- **Observe**
    
    - Check invariant on current window
        
- **Exit**
    
    - Remove element at index `i - k`
        
- **Enter**
    
    - Add element at index `i`
        

This preserves the invariant:

> Window always contains exactly `k` elements  
> representing `s[i-k+1 … i]`

### Final observation

- After the loop, you must **also check the last window**
    
- Many bugs miss this
    

---

## 4️⃣ The invariant you are implicitly using (make it explicit)

Your logic relies on:

> **At every step, the frequency state equals the multiset of the current length-`k` substring.**

That is why:

- Removal must happen **before or after** checking — but consistently
    
- Off-by-one errors break correctness immediately
    

---

## 5️⃣ Common silent bug to watch for

This line you said:

> “then u observe(), then as u move ahead with `i = k` u must remove `i-k`”

This is correct **only if**:

- Observation happens **before** modifying the window
    
- Or you explicitly shift the observation point
    

Both are valid — mixing them is not.

---

## 6️⃣ Complexity (confirming your intuition)

- Time: **O(n)**  
    (each char enters once, exits once)
    
- Space:
    
    - **O(1)** if array hashing (bounded domain)
        
    - **O(k)** if hashmap (unbounded domain)
        

---

## Final verdict

- Your reasoning is correct
    
- Your boundary conditions are correct
    
- Your loop idea is correct
    
- The only thing that matters now is **discipline about the invariant and indices**
    

This is exactly the mental model interviewers want.

If you’re ready, next step is the **Unit-2 LeetCode gate**.

