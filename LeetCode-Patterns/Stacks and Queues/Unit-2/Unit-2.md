
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

# 🔷 Monotonic Stack — Minimal Guide

## 1️⃣ What It Solves

Use it when the problem asks for:

- Next smaller
    
- Previous smaller
    
- Next greater
    
- Previous greater
    
- “First element that breaks monotonic order”
    

That’s it.

---

# 2️⃣ Core Idea

Stack stores **indices**.

Stack is kept either:

- Increasing (values increasing bottom → top)
    
- Decreasing (values decreasing bottom → top)
    

When the order breaks, you pop.

Each element:

- Pushed once
    
- Popped once  
    → O(n)
    

---

# 3️⃣ The Only Rule That Matters

Ask:

> What invalidates dominance?

If a smaller element invalidates bigger ones →  
Pop while `stack_top > current`

If a larger element invalidates smaller ones →  
Pop while `stack_top < current`

That’s the whole decision.

---

# 4️⃣ Stack Type Cheat Table

|Goal|Pop While|Stack Type|
|---|---|---|
|Next Smaller|top > current|Increasing|
|Next Greater|top < current|Decreasing|
|Prev Smaller|top >= current|Increasing|
|Prev Greater|top <= current|Decreasing|

(Strict vs non-strict handles duplicates.)

---

# 5️⃣ Direction Guide

You can go left → right always.

Left → Right naturally gives:

- Previous answers directly (after popping)
    
- Next answers during pop
    

Right → Left is optional convenience.

Direction does NOT define next/prev automatically.

---

# 6️⃣ Pattern Template (Left → Right)

```
for i in range(n):
    while stack and arr[stack[-1]] > arr[i]:
        idx = stack.pop()
        # current is answer for idx (next smaller)

    # after popping:
    # stack[-1] is previous smaller (if exists)

    stack.append(i)
```

Change `>` to `<` for greater problems.

---

# 7️⃣ When You See a Problem

Step 1: Is this nearest greater/smaller?  
If yes → monotonic stack candidate.

Step 2: Who invalidates whom?  
That determines pop condition.

Step 3: Decide strict vs non-strict for duplicates.

Done.

---

# 8️⃣ Mental Shortcut

Smaller kills bigger → use increasing stack  
Bigger kills smaller → use decreasing stack

---

That’s the entire system compressed.

If you want, I can now give you 3 ultra-small test cases to see if this is fully clear.
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
Good.  
You don’t need more stack theory.  
You need to know how to _apply_ next/prev information.

Here is the practical guide.

---

# 🔷 Step 1: Recognize the Pattern

If the question involves:

- Subarrays
    
- “For every element, how many…”
    
- Range dominance
    
- First smaller/greater boundary
    
- Histogram-style expansion
    

Then you’re in **boundary counting mode**.

Stop thinking enumeration.  
Start thinking influence range.

---

# 🔷 Step 2: Translate to Boundary Language

Convert the problem into this form:

> For each index `i`, find how far it can expand before condition breaks.

That condition is either:

- Smaller element appears
    
- Greater element appears
    

Those breakpoints are exactly:

- Previous X
    
- Next X
    

---

# 🔷 Step 3: Once You Have Prev and Next

You usually do one of these:

---

## Case A: Contribution Counting

Used in:

- Sum of subarray minimums
    
- Sum of subarray maximums
    
- Total strength type problems
    

Formula:

[  
\text{Contribution} = value \times (i - prev) \times (next - i)  
]

Interpretation:

- Left choices × Right choices
    
- Number of subarrays where element dominates
    

---

## Case B: Width Calculation

Used in:

- Largest Rectangle in Histogram
    

Formula:

$$
[  
\text{Width} = next - prev - 1  
]
$$

Then:

$$
[  
\text{Area} = height \times width  
]

$$
Interpretation:

- This element is smallest in that entire width
    

---

## Case C: Immediate Query

Used in:

- Next Greater Element
    
- Stock Span
    

You directly store the next/prev answer.

---

# 🔷 Step 4: The General Decision Framework

When you see a question, ask:

### 1️⃣ Is this about nearest breakpoints?

If yes → monotonic stack.

### 2️⃣ Does each element “control” a range?

If yes → you’ll multiply distances.

### 3️⃣ Is this about largest area / max window?

If yes → width = next - prev - 1.

---

# 🔷 Step 5: Mental Checklist for New Problems

When reading a question:

- Is there “first greater/smaller” language?
    
- Is it about subarrays?
    
- Is it about dominance?
    
- Is brute force O(n²) too slow?
    
- Can I reframe it as: “How far can each element expand?”
    

If yes → think prev/next.

---

# 🔷 Why This Cracks Questions

Because most “hard” stack problems are secretly:

> Count how long an element survives before something stronger appears.

Once you see that,  
you stop memorizing solutions.

You identify structure.

---

If you want, give me any monotonic stack problem name and I’ll show you how to detect which pattern it belongs to in under 30 seconds.

Good.

We proceed in order.

---

# 🏗️ UNIT 2 — Monotonic Structures

## 📦 Module 2.2 — Monotonic Queue

### 🔹 Submodule 2.2.1 — Sliding Window Maximum Pattern

---

# 🧩 Submodule 2.2.1 — Sliding Window Maximum Pattern

---

## 🟢 1. Mental Model

Forget stack elimination.

Now think:

> “I have a moving window of size k.  
> At every position, I need the maximum inside that window.”

Brute force:

- For every window
    
- Scan k elements
    
- Pick max  
    → O(nk)
    

We want:

> O(n) total.

So the question becomes:

How do I maintain the maximum **while the window moves**, without rescanning?

This is not a boundary problem.

This is:

> Maintain optimal candidate under insert + expiry.

---

## 🔵 2. Why This Exists

Brute force fails because:

- Each element gets revisited multiple times.
    
- You recompute information you already knew.
    

What we want instead:

- Each element should be “processed” once.
    
- When something stronger appears, weaker elements become irrelevant.
    
- When something exits window, remove it cleanly.
    

This is dynamic elimination + expiry.

---

## 🟣 3. Core Building Blocks

We use a **deque**.

It maintains:

1. **Decreasing order of values**
    
2. Stores **indices**, not values
    
3. Removes elements from:
    
    - Back → if smaller than current
        
    - Front → if outside window
        

Two invariants:

### Invariant A — Monotonic Decreasing

Front always holds the maximum.

### Invariant B — Valid Window

Front index must be within current window.

---

## 🧪 4. How It Behaves in the Wild

Let’s reason structurally:

When a new element arrives:

- Remove all smaller elements from back.
    
- Why? Because they can never become max again.
    

When window moves:

- Remove front if it falls outside window.
    
- Why? Because it expired.
    

So each element:

- Enters once
    
- Leaves once
    

Amortized O(n).

---

## 🛠️ 5. Real-World Usage Patterns

This pattern appears in:

- Sliding window maximum
    
- Real-time streaming analytics
    
- Stock price tracking over intervals
    
- Temperature trends
    
- Rate limiting systems
    

Professionals misuse it when:

- They forget to remove expired indices.
    
- They store values instead of indices.
    
- They break the monotonic invariant.
    

---

Now let’s test understanding.

Imagine:

nums = [1, 3, -1, -3, 5, 3, 6, 7]  
k = 3

Before coding anything:

Tell me:

What exactly should be inside the deque  
after processing the first 3 elements?

Not values.

Indices.

Reason it.

Monotonic Stack = elimination logic  
Monotonic Queue = elimination + expiry logic

It’s the same dominance idea.

Just with an added boundary constraint.

