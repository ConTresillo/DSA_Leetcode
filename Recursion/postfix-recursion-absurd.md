You’re not crazy. The problem _is_ awkward if you restrict yourself to **plain recursion with no auxiliary data structure**. That restriction removes the natural solution.

Here are **hints only**, no solution.

---

## First: sanity check (important)

Reverse Polish Notation is **inherently stack-based**.  
Any correct evaluation needs to model a stack _somehow_.

If you ban:

- an explicit stack
    
- arrays / lists
    
- global variables
    

then the **call stack itself must act as the stack**. That is the only remaining option.

This is why it feels “crazy”.

---

## Hint 1 — Stop thinking left → right

If you process tokens left to right, recursion fights you.

Think instead:

> What happens if you start from the **end** of the tokens?

In postfix, the **last operator** combines the two expressions immediately before it.

---

## Hint 2 — Define recursion in terms of “consume”

Instead of:

> “evaluate the whole expression”

Think:

> “evaluate one expression and return **two things**:
> 
> 1. its value
>     
> 2. how many tokens it consumed”
>     

This avoids global state.

Ask yourself:

- When I see a number, how many tokens does it consume?
    
- When I see an operator, what must I evaluate _before_ it?
    

---

## Hint 3 — Operator case thinking

When the current token is an operator:

- You need **two operands**
    
- Those operands are themselves postfix expressions
    
- They lie _immediately to the left_
    

So recursion must:

1. Evaluate the **right operand**
    
2. Then evaluate the **left operand**
    
3. Then apply the operator
    

Order matters.

---

## Hint 4 — The “aha” constraint

Pure recursion **can** do this, but only if:

- recursion returns **more than just a value**
    
- or you encode progress implicitly (e.g., index moving backward)
    

If your function returns _only_ an integer, you are stuck.

That’s not a skill issue — it’s a design constraint.

---

## Hint 5 — Interview reality check

In interviews:

- Stack solution → expected
    
- Recursive solution → accepted _only if_ it clearly simulates a stack
    
- “Plain recursion, no helpers” → **not expected**
    

So struggling here does **not** reflect weakness.

---

### Bottom line (no sugarcoating)

- Your intuition is correct
    
- The problem is unnatural without a stack
    
- Plain recursion works, but only with a **non-obvious mental model**