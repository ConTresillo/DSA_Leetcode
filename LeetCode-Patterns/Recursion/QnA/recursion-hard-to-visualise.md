## 0. Stop thinking about “recursion”

This problem is **not** about recursion.

Recursion is just _how_ we execute a rule repeatedly.

The real question is:

> **What does it even mean for two trees to be mirrors?**

Until that sentence is precise, code is impossible.

---

## 1. Define the problem in ONE sentence (this is the missing step)

Two trees `A` and `B` are mirror images **iff**:

1. Their root values are equal
    
2. The _left subtree of A_ mirrors the _right subtree of B_
    
3. The _right subtree of A_ mirrors the _left subtree of B_
    

That’s it.  
No code. No recursion. Just logic.

If you cannot say this sentence, recursion will feel like magic forever.

---

## 2. Turn the definition into a **question**

Instead of thinking:

> “How do I recurse?”

Think:

> “If someone asks me whether tree `L` mirrors tree `R`, what must I check?”

Answer **in English**, not code.

- If both are empty → mirrors ✔
    
- If only one is empty → not mirrors ✘
    
- If values differ → not mirrors ✘
    
- Otherwise:
    
    - left of L vs right of R
        
    - right of L vs left of R
        

Notice something important:

👉 **The problem contains itself inside its own definition**

That’s not recursion yet — that’s a _self-referential definition_.

---

## 3. Why recursion is inevitable (and not optional)

Let’s try to avoid recursion mentally.

Suppose I ask:

> “Do these two subtrees mirror each other?”

How do you answer that **without** asking the same question about smaller subtrees?

You can’t.

This is the key realization:

> **Recursion appears because the definition is recursive, not because we chose it.**

You are not “using recursion”.  
You are **obeying the definition**.

---

## 4. The single invariant (this fixes everything)

Every recursive function must have **one sentence that is always true**.

Here it is:

> `helper(L, R)` answers:  
> “Are L and R mirror images?”

That’s the _only_ thing it means.  
Not “process”, not “traverse”, not “compare”.

Once you accept this, every line becomes obvious.

---

## 5. Re-derive your code slowly (no memorization)

### Base cases

Ask: _When is the answer obvious without further questions?_

- Both empty → yes
    
- One empty → no
    

This is not memorized.  
It’s logically forced by the definition.

---

### Value check

Ask: _Can two mirrors have different roots?_

No.

So:

`if (L.val != R.val) return false;`

Again: definition → code.

---

### The recursive step (the part you “can’t see”)

Ask yourself this **exact** question:

> “If L mirrors R, what must be true about their children?”

Answer (say it out loud):

- L’s left must mirror R’s right
    
- L’s right must mirror R’s left
    

That’s not intuition. That’s symmetry.

So the return value must be:

`helper(L.left, R.right) && helper(L.right, R.left)`

If either side breaks symmetry → whole thing breaks.

---

## 6. Why your brain resists this (important)

Your brain is trying to:

- simulate execution
    
- track stack frames
    
- follow call order
    

That is **the wrong layer**.

Recursion is **not about time**.  
It is about **truth preservation**.

Each call answers a yes/no question.  
The call stack is irrelevant.

---

## 7. How to “see” it visually (without code)

Picture this:

`Call #1:  Are these mirrors?             L       R            / \     / \           a   b   b   a`

To answer that, you must answer **two smaller mirror questions**:

`Call #2: a vs a Call #3: b vs b`

Each of those again reduces the same way.

You never “go deeper”.  
You just ask the _same question_ on smaller inputs.

That’s recursion.

---

## 8. A rule you should tattoo mentally

> If you can explain what your function answers **without mentioning recursion**,  
> you understand recursion.

Try this for your helper:

> “`helper(L, R)` returns true if L and R are mirror trees.”

If you can say that confidently, the code writes itself.

---

## 9. Your exact problem (diagnosis)

You said:

> “I memorised this, I can’t intuitively see it”

Correct diagnosis:

- You skipped **problem definition**
    
- You jumped straight to **implementation**
    

That works for loops.  
It fails for recursion.

---

## 10. What we should do next (tutor plan)

To actually fix this long-term, next we should:

1. Take a **non-tree recursive problem**
    
2. Force you to write the _English contract first_
    
3. Only then write code
    

If you want, next I’ll do:

- palindrome recursion **without code first**
    
- or linked-list recursion (simpler invariant)