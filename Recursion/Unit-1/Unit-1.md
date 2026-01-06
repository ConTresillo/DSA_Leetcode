
```handdrawn-ink
{
	"versionAtEmbed": "0.3.4",
	"filepath": "Ink/Drawing/2026.1.3 - 11.10am.drawing",
	"width": 500,
	"aspectRatio": 1
}
```
# 🧩 **Submodule 1.1.1: Definition of Recursion**
## 🟢 1. What it is (Concept)

- **Recursion** is a problem-solving technique where:
    
    - A function **calls itself**
        
    - To solve a **smaller instance of the same problem**
        
- Formal view:
    
    - A problem is defined **in terms of itself**, with a **termination condition**
        
- Two forms:
    
    - **Direct recursion**: function calls itself directly
        
    - **Indirect recursion**: function A → function B → function A
        
- Key idea:
    
    - The function trusts its **future self** to solve the smaller problem
## 🔵 2. Why it exists (Purpose)

- Some problems are **naturally self-similar**
    
    - Same structure repeats at smaller scales
        
- Recursion allows:
    
    - Cleaner logic than loops for hierarchical problems
        
    - Direct mapping from **problem definition → code**
        
- Without recursion:
    
    - Code becomes complex
        
    - State management becomes manual and error-prone
        
- **Why exams test this**
    
    - Tests abstraction
        
    - Tests termination reasoning
        
    - Tests stack understanding
## 🟣 3. Core Components / Terminology

- **Recursive function** — function that calls itself
    
- **Self-reference** — function name appears inside its body
    
- **Problem reduction** — input size strictly decreases
    
- **Termination guarantee** — recursion must reach a stopping state
    
- ❌ Recursion is **not** looping
    
    - Loop = same state space
        
    - Recursion = new function state each call
## 🧪 4. Structure / Logical Flow

**Conceptual flow (exam-ready):**

- Define the problem in terms of:
    
    - Same problem
        
    - Smaller input
        
- Ensure:
    
    - Each call moves closer to termination
        
- Control:
    
    - Function execution flows **downward**
        
    - Returns happen **upward**
        

**Answer-writing flow:**

1. State recursion definition
    
2. Mention self-call + reduced problem
    
3. Mention termination requirement
## 🛠️ 5. Deep Example / Case Study

**Conceptual POV**

- If a problem can be broken into:
    
    - One smaller version of itself
        
    - Plus some local work
        
- Recursion fits naturally
    

**Logical POV**

- Assume:
    
    - Function works correctly for smaller input
        
- Then:
    
    - Extend logic to current input
        

**Exam-answer POV**

- “Recursion works by reducing a problem to
    
- smaller instances of the same problem until a base condition is reached.”
## 🎯 6. Exam Perspective

- Frequently asked as:
    
    - Definition question (2–3 marks)
        
    - Introductory part of long answers
        
- Examiner expects:
    
    - Clear definition
        
    - Mention of self-call
        
    - Mention of termination
        
- Missing termination → **answer penalty**
## ⚠️ 7. Common Mistakes & Traps

- ❌ Saying “recursion is looping”
    
- ❌ Forgetting to mention **smaller problem**
    
- ❌ Ignoring termination condition
    
- ❌ Writing code-like explanation in theory answers
## 🧠 8. Memory Hook (Logical)

- **Same problem**  
    → **smaller input**  
    → **trust recursion**  
    → **stop at base**
    

Cause → effect chain, no metaphor.
## 📝 9. Ultra-Short Revision Sheet

- Function calls itself
    
- Solves smaller instance
    
- Requires termination
    
- Uses call stack
    
- Matches self-similar problems
    
- Cleaner than loops for hierarchy
    

(< 60 seconds)
## 📌 Expected Exam Keywords

- Recursion
    
- Recursive function
    
- Self-referential definition
    
- Smaller subproblem
    
- Termination condition
    
- Direct recursion
    
- Indirect recursion