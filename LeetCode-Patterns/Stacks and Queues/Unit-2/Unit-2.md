
---

# 🏗️ UNIT 2 — Monotonic Structures (Interview Filter Zone)

## 📦 Modules Inside This Unit

- 2.1 — Monotonic Stack
    
- 2.2 — Monotonic Queue
    

---

## 📦 Module 2.1 — Monotonic Stack

- 2.1.1 — Next Greater / Next Smaller Pattern ⬅️ (Current)
    
- 2.1.2 — Range Influence & Span Computation
    

---

# 🧩 Submodule 2.1.1 — Next Greater / Next Smaller Pattern

This is where interviews start separating shallow stack users from real pattern thinkers.

---

## 🟢 1. Mental Model

Normal stack = LIFO constraint.

Monotonic stack = LIFO + ordered invariant.

You maintain the stack such that:

- Elements are always increasing  
    or
    
- Always decreasing
    

Depending on the problem.

The stack is no longer just storage.

It becomes a **filter that eliminates useless elements in one pass**.

---

## 🔵 2. Why This Exists

Brute force approach for "Next Greater Element":

For every element:

- Scan right
    
- Find first greater
    

Time: O(n²)

Monotonic stack compresses that into:

O(n)

How?

By removing elements that can never be answers again.

---

## 🟣 3. Core Building Blocks

Let’s consider “Next Greater Element”.

We process from left → right.

We maintain a decreasing stack.

Invariant:

```
Stack elements are strictly decreasing from bottom to top.
```

Why decreasing?

Because when we see a new element `x`:

If `x` is greater than stack.top:

Then stack.top has found its next greater.

So we pop until invariant holds.

Each pop resolves one element permanently.

---

### Structural Behavior

Imagine array:

```
2 1 3
```

Step-by-step:

Push 2  
Stack: [2]

Push 1  
Stack: [2, 1]

See 3  
3 > 1 → pop 1  
3 > 2 → pop 2

Both resolved in one step.

Push 3  
Stack: [3]

Each element is pushed once and popped once.

That’s why O(n).

---

## 🧪 4. How It Behaves in the Wild

### Normal Case

Each element:

- Enters stack once
    
- Leaves once
    

Total operations = 2n → O(n)

---

### Failure Mode

If you forget invariant:

You fall back to nested loops.

---

### Important Variation

You can compute:

- Next Greater to right
    
- Next Greater to left
    
- Next Smaller
    
- Previous Smaller
    

By changing:

- Direction of traversal
    
- Comparison operator
    

Same skeleton.

---

## 🛠️ 5. Real-World Usage Patterns

Monotonic stacks appear in:

- Daily Temperatures
    
- Stock Span
    
- Largest Rectangle in Histogram
    
- Trapping Rain Water
    
- Sum of Subarray Minimums
    

This is one of the highest ROI patterns in interviews.

---

Perfect.

Here is the **Ultimate Summary — Unit 2.1.1 Only**  
(Submodule: Next Greater / Next Smaller Pattern)

No spillover. Only this topic.

---

# 🧠 UNIT 2.1.1 — Monotonic Stack

## Pattern: Next Greater / Next Smaller

---

## 1️⃣ What Problem It Solves

Brute force idea:

For every element:

- Scan right
    
- Find first greater (or smaller)
    

Time = O(n²)

Monotonic stack compresses this to:

Time = O(n)

---

## 2️⃣ Core Structural Invariant

For Next Greater (scan left → right):

Maintain a **decreasing stack**.

Invariant:

```
Stack elements strictly decreasing from bottom → top
```

Why?

Because if current element `x` is greater than stack.top:

Then stack.top has found its next greater.

So we pop it.

That pop is irreversible.

---

## 3️⃣ Core Algorithm Skeleton

For each element `arr[i]`:

```
while stack not empty AND arr[i] > stack.top:
    pop stack.top (its answer is arr[i])

push arr[i]
```

That’s the entire mechanism.

---

## 4️⃣ Why It Is O(n)

Each element:

- Is pushed once.
    
- Is popped at most once.
    

Total stack operations:

```
pushes ≤ n
pops ≤ n
```

Total ≤ 2n → O(n)

---

## 5️⃣ Why Each Element Pops Only Once

Because:

- We scan strictly in one direction.
    
- Once popped, an element is permanently resolved.
    
- It never re-enters the stack.
    
- There is no mechanism to revisit earlier elements.
    

This is an **irreversible elimination process**.

No cycles.  
No resurrection.

---

## 6️⃣ What Would Break O(n)

If:

- We re-scan the array.
    
- Or reinsert popped elements.
    
- Or revisit earlier indices.
    

Then elements could:

- Be pushed again.
    
- Be popped again.
    

Amortization collapses.

The single-pass + irreversibility guarantee is critical.

---

## 7️⃣ Variations You Must Recognize

Same skeleton, only change:

- Traversal direction
    
- Comparison operator
    

You can compute:

- Next Greater Right
    
- Next Greater Left
    
- Next Smaller Right
    
- Previous Smaller
    
- Distance to next greater
    
- Indices instead of values
    

It’s the same invariant engine.

---

## 8️⃣ Mental Compression Model

Monotonic stack =

- One-pass scan
    
- Stack enforces order
    
- New element eliminates weaker previous elements
    
- Each element dies once
    

Think of it as:

> Competitive elimination tournament.

When a stronger element appears, weaker ones lose permanently.

---

## 9️⃣ What You Must Internalize

This is not a stack problem.

It is a:

> One-directional irreversible elimination system.

The stack is just the tool.

The real idea is:

- Eliminate useless elements immediately.
    
- Never revisit resolved states.
    

---

## 🔟 Where This Pattern Appears

- Daily Temperatures
    
- Next Greater Element I & II
    
- Stock Span
    
- Largest Rectangle in Histogram
    
- Trapping Rain Water
    
- Sum of Subarray Minimums
    

High-frequency interview territory.

---

## 🚀 Final Structural Takeaway

Monotonic stack works because:

- Traversal is one-directional.
    
- Decisions are irreversible.
    
- Each element participates in ≤ 2 stack events.
    

That guarantees linear time.

---

# 🧩 Submodule 2.1.2 — Span & Distance Formulation

---

## 🟢 1. Mental Model

2.1.1 was about:

> “What is the next greater value?”

2.1.2 shifts to:

> “How far does dominance extend?”

This is not about finding a single next element.

It is about measuring a **range of influence**.

Example mental shift:

Instead of:

- “Who is my next greater?”
    

Think:

- “How long can I survive before someone stronger appears?”
    

This is distance thinking.  
Not mapping thinking.

---

## 🔵 2. Why This Exists

In many problems:

- You don’t just need the next greater element.
    
- You need how many elements you dominate.
    
- Or how far you can extend before being stopped.
    

Brute force:

For each element:

- Walk left or right until blocked.
    

Time = O(n²)

Monotonic stack turns this into:

Time = O(n)

Because each element enters and leaves once.

---

## 🟣 3. Core Building Blocks

This submodule introduces:

### 1️⃣ Index-based stack (mandatory)

Distance requires:

```
distance = i - previous_index
```

Values alone are useless.

---

### 2️⃣ Direction matters

Span problems often require:

- Scan left → compute previous greater
    
- Or scan right → compute next greater
    

You must choose direction based on:

- What the question measures
    

---

### 3️⃣ Strict vs Non-Strict Comparison

Critical detail:

- `>` vs `>=`
    
- `<` vs `<=`
    

This changes span behavior.

Example:  
Duplicates behave differently depending on comparison.

This is not cosmetic.  
It affects correctness.

---

## 🧪 4. How It Behaves in the Wild

Common span-style problems:

- Stock Span
    
- Largest Rectangle in Histogram
    
- Sum of Subarray Minimums
    
- Previous Smaller Element
    

All follow this structure:

1. Maintain monotonic invariant
    
2. When popping:
    
    - Calculate span or contribution
        
3. Push index
    

---

## 🛠️ 5. Real-World Usage Patterns

This pattern models:

- Influence range
    
- Dominance window
    
- Valid interval expansion
    

In production systems, this appears in:

- Time-series peak detection
    
- Window-based anomaly detection
    
- Trend duration tracking
    

The stack is just a device.

The real concept is:

> Dominance propagation until blocked.

---
# 🧠 UNIT 2.1.2 — Span & Distance Formulation

## Structured Q&A Session (Encapsulated)

---

## ❓ Q1 — What exactly is “Span” measuring?

**Answer**

Span measures:

> How far an element can extend before being blocked by a stronger constraint.

It is not about:

- Finding the next element
    
- Mapping value → value
    

It is about:

> Measuring the uninterrupted region where a condition holds.

In Stock Span:

- Condition = previous prices ≤ today
    
- Blocker = previous price > today
    

Span ends at the first blocker.

---

## ❓ Q2 — Why do we use indices instead of values?

**Answer**

Because span is distance-based.

We need:

```
distance = current_index - previous_blocker_index
```

Values alone cannot compute distance.

Indices give:

- Value lookup
    
- Unique identity
    
- Direct distance computation
    

Span problems are index-driven.

---

## ❓ Q3 — Why does the comparison operator matter so much?

**Answer**

Because the operator defines:

> What is considered a blocker.

If problem says:

“previous days with price ≤ today”

Then blocker is:

price > today

So equal prices must not block.

Hence we pop while:

```
price[stack.top] <= current
```

The operator encodes the boundary rule.

It is not arbitrary.

---

## ❓ Q4 — Why does each element enter and leave the stack only once?

**Answer**

Because:

- We scan in one direction.
    
- Once popped, an index is resolved permanently.
    
- It never re-enters.
    

This creates an irreversible elimination system.

Total operations:

- ≤ n pushes
    
- ≤ n pops
    

Time = O(n)

---

## ❓ Q5 — What breaks O(n) in span problems?

**Answer**

If you:

- Revisit indices
    
- Reinsert popped elements
    
- Scan backward repeatedly
    

You destroy irreversibility.

Monotonic stack works because:

> No element is reconsidered after being resolved.

---

## ❓ Q6 — Why do equal elements sometimes need to be merged?

**Answer**

Because in many span problems:

Equal values do not block each other.

Example:

```
80 80 80
```

Each 80 should absorb previous 80.

If you treat equal as blocker:

You fragment the span incorrectly.

That’s why operator changes per problem.

---

## ❓ Q7 — How is this different from 2.1.1 (Next Greater)?

**Answer**

2.1.1:

- We stored value → next greater.
    
- Output was mapping.
    

2.1.2:

- We measure distance or width.
    
- Output depends on index spacing.
    

Both use monotonic stack.

But the abstraction is different:

- 2.1.1 → Deferred resolution of value.
    
- 2.1.2 → Dominance range measurement.
    

---

## ❓ Q8 — What mental mistake causes confusion in 2.1.2?

**Answer**

Treating it as:

“Just another next greater element.”

Instead of:

“How long does this element dominate?”

Span problems are about influence zones.

Not about one-to-one mapping.

---

## ❓ Q9 — When do we choose `<`, `<=`, `>`, `>=`?

**Answer**

Ask:

> What exactly blocks extension?

- If equal should merge → pop equals.
    
- If equal should block → keep equals.
    

Operator reflects blocker definition.

Never copy it from another problem.

Derive it from boundary rule.

---

## ❓ Q10 — What is the invariant of 2.1.2?

**Answer**

The stack always stores:

Indices of elements that have not yet found their blocker.

And maintains monotonic order such that:

- The nearest stronger blocker is always accessible at top.
    

This guarantees correct span calculation.

---
