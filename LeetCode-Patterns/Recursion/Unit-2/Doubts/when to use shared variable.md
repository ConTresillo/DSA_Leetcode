This is a **very mature question**. You’re not asking “what works”, you’re asking **when a rule stops applying**. Let’s resolve the tension properly.

---

## The short answer (then we unpack it)

Yes — **sometimes shared variables are not only acceptable, they are the cleanest expression of the problem**.

They’re “bad practice” **only when they hide structure**.  
Here, they **reveal structure**.

That distinction is everything.

---

## Why “avoid shared state” is taught in the first place

The warning exists because shared state often causes:

- hidden dependencies
    
- order-sensitive bugs
    
- reasoning that spans multiple functions
    
- difficulty testing or reusing components
    

In other words, shared state is bad when:

> **the correctness of a function depends on invisible external context**

That’s the key criterion.

---

## What makes _this_ case different

In **Kth Smallest in BST**, the shared variable (`i` or `k`) is not:

- optional
    
- incidental
    
- mutable for convenience
    

It represents something **fundamental**:

> the **global position** in an ordered traversal

That position:

- cannot be localized to a subtree
    
- cannot be computed independently
    
- is inherently sequential
    

This is not “state leakage”.  
This is **state modeling**.

---

## Why you felt “stuck” (this is important)

You were stuck because you were trying to satisfy **two conflicting goals**:

1. Avoid shared state
    
2. Avoid composite return values
    

But the problem itself demands:

- **one global cursor**
    
- **early termination**
    
- **no full traversal**
    

Those three constraints **force** one of these:

- a shared variable
    
- or a composite return that _carries the cursor implicitly_
    

There is no third option.

Your discomfort was your brain noticing:

> “I’m trying to enforce a design rule that doesn’t match the problem’s shape.”

That’s a good signal, not a failure.

---

## Let’s examine the alternatives you _could_ use

### Option 1: Composite return (what you tried)

You return something like:

- `(foundValue, countSoFar)`
    

This works, but notice what happens:

- `countSoFar` is effectively **shared state**, just threaded manually
    
- logic becomes more complex
    
- you simulate sequencing through structure that isn’t sequential
    

So you didn’t eliminate shared state —  
you **distributed it**.

That’s why it felt heavy.

---

### Option 2: Explicit stack / iterative inorder

You simulate recursion with a stack and a loop.

This avoids:

- recursion
    
- shared mutable variables across calls
    

But it still has:

- a loop counter
    
- a global `k` decrement
    

The state didn’t disappear.  
It just moved.

---

### Option 3: Precompute subtree sizes

Now you _can_ avoid shared state.

But notice the cost:

- extra traversal
    
- extra memory
    
- more complex invariants
    
- overkill if you only need one query
    

This is only justified when:

- many queries exist
    
- tree is static
    
- preprocessing is amortized
    

For the single-query version, this solution **fights the problem**.

---

## So what’s the real rule about shared state?

Here’s the refined rule — this is the one that actually holds:

> **Shared state is acceptable when it represents a single global invariant that cannot be decomposed.**

Bad shared state:

- represents _incidental implementation detail_
    
- could be localized but wasn’t
    

Good shared state:

- represents _the problem’s irreducible global quantity_
    

In this problem, that quantity is:

> “How many values have I seen so far in sorted order?”

There is no local version of that.

---

## Why this does NOT contradict good engineering practice

A few important points:

- The shared variable has:
    
    - a clear meaning
        
    - a single writer
        
    - a monotonic update
        
- It is not modified arbitrarily
    
- Its lifecycle is tied to one traversal
    

That makes it:

- predictable
    
- testable
    
- easy to reason about
    

This is **disciplined shared state**, not accidental shared state.

---

## A useful mental test (keep this)

When you’re unsure, ask:

> “Is this variable tracking _progress of the algorithm_ or _incidental convenience_?”

- If it tracks **progress** → shared state is often right
    
- If it’s just convenience → it’s probably a smell
    

In BST Kth smallest:

- `k` / `i` tracks progress
    
- removing it makes the solution worse, not better
    

That’s your answer.

---

## Final reframing (this is the realization)

You didn’t “break a rule”.

You learned the **actual rule** behind the slogan.

> Avoid shared state **until** the problem itself is inherently sequential.

When order matters globally and early exit is essential,  
shared state stops being a hack and becomes the **clearest model**.

The fact that you _noticed the tension_ is proof your intuition is getting sharper, not weaker.