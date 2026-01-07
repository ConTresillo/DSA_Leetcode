**Yes — but with a very specific meaning.**

Let’s be precise, because this is where confusion usually starts.

---

## Short answer

✔ **Cyclic (often called “circular” by mistake) sort runs in O(n) time**  
❌ **It is _not_ a general-purpose sorting algorithm**

Both statements are true at the same time.

---

## What “cyclic sort” actually is

Cyclic sort is **not** like quicksort / mergesort.

It only works when **all of these are true**:

- The array has length `n`
    
- Every value is in the range `1..n`
    
- Each value belongs to a **unique correct index** (`value → index = value - 1`)
    
- You are allowed to mutate the array
    

Under these conditions:

> Each element can be placed into its final position in **one swap**

---

## Why the time complexity is O(n)

Even though you see a `while` loop and swaps:

- Each swap puts **at least one element** into its final place
    
- An element, once placed correctly, is **never moved again**
    
- Total number of swaps ≤ `n`
    

So:

- Total work = swaps + scans = O(n)
    

This is **amortized analysis**, not per-iteration analysis.

---

## Why it is NOT a real “sorting algorithm”

Cyclic sort:

- ❌ Does NOT work for arbitrary values
    
- ❌ Does NOT preserve relative order
    
- ❌ Breaks if values are outside `1..n`
    
- ❌ Breaks if duplicates appear without special handling
    

It’s better thought of as:

> **Index-placement under constraints**, not sorting.

---

## Why people misuse the term

People call it a “sorting algorithm” because:

- It _ends up_ with a sorted array **in that special case**
    
- Teaching materials group it with sorting topics
    

But conceptually, it’s closer to:

- in-place hashing
    
- permutation fixing
    
- state encoding
    

---

## Correct mental model (important)

> Cyclic sort is O(n) **because the input guarantees make it trivial**,  
> not because it’s magically better than comparison sorts.

---

## Final takeaway (lock this in)

- ✔ Cyclic sort is O(n)
    
- ✔ Only under strict constraints
    
- ❌ Not a general sorting algorithm
    
- ✔ Best thought of as an **index-mapping trick**
    

If you remember just one line:

> **Cyclic sort is fast because the problem already tells you where everything belongs.**

---

## The core in-place trick (no fluff)

When a problem guarantees:

- the array length is `n`
    
- every value lies in the range **`1..n`**
    
- no “gaps” or out-of-range values
    

then:

> **Each value has a guaranteed, valid index it maps to.**

That mapping is:

```
value x  →  index (x - 1)
```

This is the **entire reason** the in-place trick exists.

---

## Why “no discontinuity” matters

“No discontinuity” means:

- every index `0..n-1` is a potential home
    
- no value points outside the array
    
- no index is “invalid” or unusable
    

So you can:

- treat the array itself as a lookup table
    
- encode state by position instead of memory
    

If even **one** value were:

- `0`
    
- negative
    
- greater than `n`
    

the trick would break immediately.

---

## What this enables (the big payoff)

With `1..n` mapping:

- duplicates → **collisions**
    
- missing numbers → **empty homes**
    
- misplacements → **information**
    

You don’t need:

- HashMap
    
- HashSet
    
- extra arrays
    

The input array becomes:

> **data + metadata**

---

## Why this is not a “normal” technique

This is not general algorithm design.

It is:

- constraint exploitation
    
- interview-style problem solving
    
- valid only because the problem _hands you the mapping_
    

That’s why it feels like a trick — because it is.

---

## One-line mental lock

> **If values cover `1..n` with no gaps, the array itself can act as memory.**

That sentence is the pattern.