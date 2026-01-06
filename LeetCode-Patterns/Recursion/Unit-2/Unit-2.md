# 🧩 Submodule 2.1.1 — Single Recursive Call Pattern

## 🟢 1. What it is (Concept)

**Linear recursion**: each function call makes **exactly one recursive call**.

- Each call reduces the problem to a smaller instance  
    (usually by decrementing a counter or moving along a structure like an array, string, or list).
    
- Depth of recursion grows linearly with input size → **stack grows O(n)**.
    

### Tail vs Non-tail Distinction

- **Tail recursion**: recursive call is the **last operation** in the function.
    
- **Non-tail recursion**: additional computation happens **after** the recursive call returns.
    

### Fundamental For Problems Like

- Factorial calculation
    
- Sum of array elements
    
- Finding maximum / minimum in a list
    
- String length computation
    

---

## 🔵 2. Why it exists (Purpose / Use-case)

- Mirrors sequential structure in inputs.
    
- Provides a clean and elegant way to break down linear problems.
    
- Helps understand **call stack behavior** and **recursion depth**.
    

**Exams often test:**

- Ability to trace recursion
    
- Identifying tail vs non-tail recursion
    
- Computing time and space complexity
    

Without recursion, iterative loops can solve these problems, but **structural recursion emphasizes decomposition**, not just execution.

---

## 🟣 3. Core Components / Terminology

- **Base Case** → stops recursion and prevents infinite calls
    
- **Recursive Call** → single call with updated parameter(s)
    
- **Accumulator / Local Variable** → stores intermediate results (used in tail recursion)
    
- **Call Stack** → holds function frames; grows linearly
    
- **Tail vs Non-tail** → determines whether tail-call optimization is possible
    

---

## 🧪 4. Structure / Logical Flow

### Stepwise Approach

1. Check base case first
    
2. Perform local computation if needed
    
3. Make one recursive call with updated arguments
    
4. If non-tail, process returned result
    
5. Return value propagates up the stack
    

### Visual Stack Example (n = 3)

`sum(3)`

	`-> sum(2)`

		`-> sum(1)`

			`-> sum(0) // base case`

			`<- returns 0`

		`<- add 1`

	`<- add 2`

`<- add 3`

---

## 🛠️ 5. Deep Example / Case Study

### Problem: Sum of first `n` natural numbers

#### Non-tail Recursion

`int sum(int n) {`

`if(n == 0) return 0; // Base Case`

`return n + sum(n-1); // Single recursive call, non-tail`

`}``

![[Pasted image 20260104173217.png]]
#### Tail Recursion

`int sum(int n, int acc) {`

`if(n == 0) return acc;`

`return sum(n-1, acc+n); // Last operation is recursive call`

`}`
![[Pasted image 20260104173300.png]]

**Note:** Tail recursion allows stack optimization (if supported).

---

## ⚠️ 7. Common Mistakes & Pitfalls

- Forgetting the base case → stack overflow
    
- Misidentifying tail vs non-tail recursion
    
- Performing unintended work after recursion
    
- Confusing single recursion with multiple recursion
    
- Over-parameterization when simple state tracking suffices
    

---

## 🧠 8. Memory Hook

> “One branch at a time, depth grows linearly”

- Base case stops recursion
    
- Stack unwinds
    
- Results accumulate during return
    

---

## 📝 9. Ultra-Short Revision Sheet

- Single recursive call → linear depth **O(n)**
    
- Base case prevents infinite recursion
    
- Tail recursion → last operation
    
- Non-tail recursion → work after return
    
- Stack stores local variables and return addresses
    
- Example problems: factorial, array sum, max in list
    
- Exams: trace calls, explain stack growth, identify tail recursion

# 🧩 Submodule 2.1.2 — Tail Recursion

## 🟢 1. What it is (Concept)

**Tail recursion** is a special case of linear recursion where:

- The recursive call is the **very last operation** executed in the function.
- No computation is pending after the recursive call returns.
- The current function frame does **not depend** on results from deeper calls.
- Logical consequence: the current stack frame is **replaceable** by the next one.

### Key Defining Property (Must Be True)

- ✔️ `return recursiveCall(...)`
- ❌ `return something + recursiveCall(...)` → **NOT tail recursion**

---

## 🔵 2. Why it exists (Purpose / Motivation)

- Solves the stack growth problem of normal recursion.
- Enables **Tail Call Optimization (TCO)** in languages/compilers that support it.
- Makes recursion **memory-equivalent to iteration**.
- Allows safe execution for large input sizes.

**Exam focus areas:**
- Stack frame behavior
- Optimization potential
- Difference between *conceptual recursion* and *execution behavior*

**If tail recursion didn’t exist:**
- Even linear problems could cause stack overflow.
- Recursion would be impractical for large constraints.

---

## 🟣 3. Core Components / Terminology

- **Tail Position** → position where the recursive call is the last executed statement.
- **Accumulator** → parameter carrying partial results forward.
- **State Passing** → all required state passed via parameters.
- **Tail Call Optimization (TCO)** → compiler/runtime replaces recursion with iteration  
  *(language-dependent)*.

---

## 🧪 4. Structure / Logical Flow

### General Tail-Recursive Pattern

- Base case returns accumulated result directly.
- Recursive call updates the accumulator.
- No work remains after the recursive call.

### Stack Behavior

- **Conceptually** recursive
- **Physically** behaves like a loop (if optimized)

---

## 🛠️ 5. Deep Example / Case Study

### Example 1: Factorial

#### ❌ Non-Tail Recursive Factorial
- Pending multiplication remains.
- Stack frames cannot be discarded.

#### ✅ Tail Recursive Factorial
- Accumulator holds the result.
- No pending work after the call.

---

### Example 2: Climbing Stairs (Conceptual)

- Naive recursion → tree recursion → exponential complexity.
- Tail recursion collapses the problem into **linear state transitions**.

---

## 🎯 6. Exam Perspective

### Common Questions

- Identify whether a function is tail recursive.
- Convert non-tail recursion to tail recursion.
- Explain stack usage differences.

### Marking Focus

- Correct identification of **tail position**.
- Proper **accumulator** usage.

### Mandatory Keywords

- tail position  
- accumulator  
- stack frame reuse  
- optimization potential  

---

## ⚠️ 7. Common Mistakes & Traps

- Assuming “single recursive call = tail recursion” ❌
- Leaving hidden computation after recursion.
- Using an accumulator but still doing work after the call.
- Assuming Java guarantees TCO ❌ (it does not).
- Overusing accumulators when simple recursion is clearer.

---

## 🧠 8. Memory Hook (Logical)

- If recursion **returns a value** → not tail recursion.
- If recursion **returns immediately** → tail recursion.
- Accumulator = future result moved into the present.

---

## 📝 9. Ultra-Short Revision Sheet

- Tail recursion = recursive call is the last operation.
- Uses accumulator to carry results.
- No pending computation → stack frames reusable.
- Optimizable to iteration (language-dependent).
- Safer for large inputs.
- Exams: identify, convert, explain stack difference.

---

## 📌 Expected Exam Keywords

- tail recursion  
- tail position  
- accumulator  
- state passing  
- stack frame reuse  
- tail call optimization  
  