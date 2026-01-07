### 🧱 **UNIT 1: Recursion Fundamentals**

**Goal:** Build the mental model required to reason about any recursive solution.

#### 📦 Module 1.1: Core Recursion Mechanics

- **Submodule 1.1.1: Definition of Recursion**
    
    - Self-referential function concept
        
    - Direct vs indirect recursion
        
    - Mathematical vs programmatic recursion
        
- **Submodule 1.1.2: Base Case**
    
    - Termination condition
        
    - Single vs multiple base cases
        
    - Consequences of missing base case
        
- **Submodule 1.1.3: Recursive Case**
    
    - Problem size reduction
        
    - Relationship to base case
        
    - Progress guarantee
        
- **Submodule 1.1.4: Call Stack Behavior**
    
    - Stack frames
        
    - Function state preservation
        
    - Stack overflow conditions
        

### 🧱 **UNIT 2: Structural Recursion Patterns**

**Goal:** Recognize recursion based on input structure.

#### 📦 Module 2.1: Linear Recursion

- **Submodule 2.1.1: Single Recursive Call Pattern**
    
    - One recursive call per function
        
    - Linear depth growth
        
    - Tail vs non-tail distinction
        
- **Submodule 2.1.2: Tail Recursion**
    
    - Last operation recursion
        
    - Accumulator usage
        
    - Optimization relevance
        

#### 📦 Module 2.2: Tree Recursion

- **Submodule 2.2.1: Multiple Recursive Calls**
    
    - Branching factor
        
    - Recursive tree growth
        
    - Overlapping subproblems
        
- **Submodule 2.2.2: Divide and Conquer Structure**
    
    - Split → solve → combine
        
    - Balanced vs unbalanced splits
    
- **Submodule 2.2.3: Return Contracts & Combine Invariants** _(added)_
    
    - Defining the return contract before implementation
        
    - Return vocabulary and meaning mapping
        
    - Combine rules for merging child results
        
    - Preventing information loss during unwinding
        
    - When a single return value encodes multiple states
        
- **Submodule 2.2.4: Failure Modes in Tree Recursion** _(added)_
    
    - Case explosion from missing contracts
         
    - Early returns that kill propagation
        
    - Path-based thinking in non-path problems
        
    - Overuse of flags, wrappers, or globals
        
    - Diagnosing overcomplicated recursion    

### 🧱 **UNIT 3: Decision & Enumeration Patterns**

**Goal:** Handle problems involving choices, paths, and combinations.

#### 📦 Module 3.1: Choice-Based Recursion

- **Submodule 3.1.1: Pick / Not Pick Pattern**
    
    - Binary decision tree
        
    - Inclusion–exclusion structure
        
- **Submodule 3.1.2: Multiple Choice Recursion**
    
    - Iterative branching inside recursion
        
    - Loop + recursive call pattern
        

#### 📦 Module 3.2: Backtracking Core Pattern

- **Submodule 3.2.1: State → Explore → Undo**
    
    - State modification
        
    - Recursive exploration
        
    - Reversal (backtrack step)
        
- **Submodule 3.2.2: Constraint Pruning**
    
    - Early termination logic
        
    - Validity checks before recursion
        

### 🧱 **UNIT 4: Problem-Type–Driven Patterns**

**Goal:** Map problem statements directly to recursion templates.

#### 📦 Module 4.1: Subset & Combination Patterns

- **Submodule 4.1.1: Subset Generation**
    
    - Power set structure
        
    - Depth = input size
        
- **Submodule 4.1.2: Combination Sum–Type Patterns**
    
    - Reuse vs non-reuse of elements
        
    - Index-controlled recursion
        

#### 📦 Module 4.2: Permutation Patterns

- **Submodule 4.2.1: Fixed Position Recursion**
    
    - Swapping logic
        
    - Position-based decisions
        
- **Submodule 4.2.2: Used-Array / Visited Pattern**
    
    - Tracking chosen elements
        
    - Avoiding reuse
        

### 🧱 **UNIT 5: Recursion on Data Structures**

**Goal:** Apply recursion where the data structure naturally suggests it.

#### 📦 Module 5.1: Array & String Recursion

- **Submodule 5.1.1: Index-Based Recursion**
    
    - Start/end pointer movement
        
    - Shrinking problem window
        

#### 📦 Module 5.2: Linked List Recursion

- **Submodule 5.2.1: Head–Rest Decomposition**
    
    - Current node processing
        
    - Recursive call on next node
        

#### 📦 Module 5.3: Tree Recursion

- **Submodule 5.3.1: Preorder / Inorder / Postorder Patterns**
    
    - Visit placement logic
        
    - Recursive traversal structure
        

### 🧱 **UNIT 6: Optimization & Limits**

**Goal:** Understand why naïve recursion fails and how to fix it.

#### 📦 Module 6.1: Overlapping Subproblems

- **Submodule 6.1.1: Exponential Blow-Up**
    
    - Repeated computations
        
    - Recursive tree visualization
        

#### 📦 Module 6.2: Memoization Bridge

- **Submodule 6.2.1: Recursion → DP Transition**
    
    - State definition
        
    - Cache usage logic
        

### 🧱 **UNIT 7: Recursion Thinking for Interviews**

**Goal:** Convert intuition into structured answers.

#### 📦 Module 7.1: Recursion Framing

- **Submodule 7.1.1: How to Think Recursively**
    
    - Faith in recursion
        
    - Function contract mindset
        
- **Submodule 7.1.2: Common Interview Failure Points**
    
    - Stack misunderstanding
        
    - Incorrect base cases