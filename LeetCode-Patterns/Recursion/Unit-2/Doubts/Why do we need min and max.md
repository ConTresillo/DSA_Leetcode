This is a **very good instinct**, and it’s also **slightly incomplete**.  
Let’s resolve it rigorously.

---

## Your assumption (state it clearly)

You’re thinking:

> If
> 
> - left subtree is a BST
>     
> - right subtree is a BST
>     
> - left.val < parent.val < right.val  
>     then the whole tree is a BST
>     

This **sounds** right — but it hides a bug.

---

## The precise flaw

**`left.val` and `right.val` are not sufficient.**

Why?

Because:

- `left.val` is just the **root value** of the left subtree
    
- `right.val` is just the **root value** of the right subtree
    

But BST validity depends on **all nodes** in the subtree, not just the root.

---

## Concrete counterexample (this kills the assumption)

```
        10
       /  \
      5    15
       \
        12
```

- Left subtree root = `5` ✔️
    
- Right subtree root = `15` ✔️
    
- `5 < 10 < 15` ✔️
    
- Left subtree **is internally a BST** ✔️
    

But:

- Node `12` is in the left subtree
    
- `12 > 10` ❌ violates BST rule
    

So the tree is **NOT** a BST.

Your rule accepts this tree.  
That means the rule is incomplete.

---

## The correct condition (fully general)

To validate BST at a node, you need:

```
max(left subtree) < node.val < min(right subtree)
```

Not:

```
left.root.val < node.val < right.root.val
```

This is the exact missing information.

---

## Why courses trick you here

Many explanations silently assume:

- values are placed “nicely”
    
- or examples are symmetric
    
- or trees are small
    

So the flaw doesn’t show up until:

- deeper trees
    
- adversarial test cases
    
- interviews
    

That’s why this problem is a **contract stress-test**.

---

## Minimal information required (no more, no less)

A child must report:

- Is my subtree a BST?
    
- What is my minimum value?
    
- What is my maximum value?
    

Anything less → incorrect in some cases  
Anything more → unnecessary

---

## Key takeaway (lock this in)

> **Local checks are insufficient for global properties.**

BST is a **range-based invariant**, not a local comparison.

That’s why:

- return contracts matter
    
- booleans alone fail
    
- tree recursion needs semantic design
    

---

If you want, next we can:

- walk through how this mistake appears in real code and why it _looks_ correct, or
    
- finish with **2.2.4 Failure Modes** as a diagnostic summary.
    

Just say which.