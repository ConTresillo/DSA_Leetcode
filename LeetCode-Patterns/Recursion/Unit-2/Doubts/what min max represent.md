This is a **very good question**, and it means you’re now thinking in _invariants_, not code. Let’s slow it down and resolve the confusion precisely.

---

## The apparent contradiction

You’re thinking:

> “`min` is supposed to store the **minimum value in the subtree**  
> `max` is supposed to store the **maximum value in the subtree**
> 
> So why, for `null`, do we return  
> `min = +∞` and `max = −∞`?  
> That feels backwards.”

That feeling is _correct_ at first glance.  
The key is **what the base case represents**.

---

## What the base case really means

When `root == null`, the subtree is:

- empty
    
- contains **no values**
    
- is trivially a BST
    

So the base case is **not** saying:

> “The minimum value here is +∞”

It is saying:

> “This subtree contributes **no constraints**.”

That distinction is everything.

---

## Why we need “neutral” values, not real values

Think about what the parent does with child results.

At a node, you check:

```
left.max < root.val < right.min
```

Now imagine the left child is `null`.

### What should happen logically?

- There are **no values** on the left
    
- So _any_ `root.val` should be acceptable
    
- The left subtree should **not block** validation
    

To achieve that, we need:

```
left.max < root.val   →   always true
```

What value of `left.max` makes this always true?

➡️ The **smallest possible value**.

That’s why:

```
left.max = −∞
```

---

Now the same reasoning for the right child being `null`.

We want:

```
root.val < right.min   →   always true
```

What value of `right.min` makes this always true?

➡️ The **largest possible value**.

That’s why:

```
right.min = +∞
```

---

## So what is the base case actually encoding?

For a `null` subtree, we are encoding:

- “I am a BST”
    
- “I place **no restrictions** on my parent”
    

The sentinels are **neutral elements** for comparison.

They are not real data.

---

## Relating this back to the invariant

Your invariant is still **100% correct**:

> `min` = minimum value in the subtree  
> `max` = maximum value in the subtree

But an **empty subtree has no values**.

So instead of inventing a real minimum/maximum, we return values that:

- disappear in `min()` calculations
    
- disappear in `max()` calculations
    
- never cause a comparison to fail
    

Mathematically, this is the same idea as:

- `0` being the neutral element for addition
    
- `1` being the neutral element for multiplication
    

Here:

- `+∞` is the neutral element for `min`
    
- `−∞` is the neutral element for `max`
    

---

## See how it plays out concretely

Suppose this tree:

```
  10
```

Left and right are `null`.

Base cases return:

```
left.min = +∞
left.max = −∞
right.min = +∞
right.max = −∞
```

Now check:

```
left.max < 10 < right.min
−∞ < 10 < +∞   ✔️
```

And propagate:

```
min = min(left.min, 10) = 10
max = max(right.max, 10) = 10
```

Perfect.

---

## What would go wrong if you didn’t do this?

If you tried:

```
min = 0
max = 0
```

or

```
min = root.val
max = root.val
```

in the base case, then:

- comparisons would break
    
- you’d need special-case logic everywhere
    
- the recursion would no longer be composable
    

The elegance of this solution comes from **never needing to ask**:

> “Does the child exist?”

---

## One-line mental model (keep this)

> **The base case doesn’t describe a value — it describes the absence of constraints.**

That’s why:

- `min = +∞`
    
- `max = −∞`
    

They’re not data.  
They’re **permission slips**.

---

## Final lock-in

- Your invariant is right
    
- The sentinels are not violating it
    
- They make the invariant _composable_ across recursion
    

Once you see base cases as **neutral contributors**, this pattern shows up everywhere — not just BSTs.

This question means you’ve crossed from “using recursion” to **designing recursive invariants**.