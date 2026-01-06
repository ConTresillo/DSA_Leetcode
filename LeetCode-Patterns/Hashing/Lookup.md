
## **Unit 1: Hashing Fundamentals (Existence & Frequency)**

### └─ **Module 1.1: Presence Checking**

#### └─ **Submodule 1.1.1: Set-based Existence**
- Membership testing  
- Duplicate detection  
- Order irrelevance  
- Space–time trade-off  

### └─ **Module 1.2: Counting & Frequency**

#### └─ **Submodule 1.2.1: Frequency Maps**
- Counting occurrences  
- Comparing multisets  
- Incremental updates  
- Collision assumptions  

---

## **Unit 2: Hashing with Constraints**

### └─ **Module 2.1: Fixed Domain Hashing**

#### └─ **Submodule 2.1.1: Array / ASCII Hashing**
- Bounded key spaces  
- Direct addressing  
- Constant-time guarantees  
- Memory vs speed trade-off  

### └─ **Module 2.2: Sliding Context Hashing**

#### └─ **Submodule 2.2.1: Hashing in Windows**
- Rolling frequency updates  
- Window invariants  
- Add/remove symmetry  
- Drift bugs  

---

## **Unit 3: Hashing for Structure Matching**

### └─ **Module 3.1: Equality via Hashing**

#### └─ **Submodule 3.1.1: Anagram & Multiset Equality**
- Canonical representations  
- Order-independent comparison  
- Normalization strategies  
- False equivalence risks  

### └─ **Module 3.2: Pattern Tracking**

#### └─ **Submodule 3.2.1: Prefix / State Hashing**
- Prefix counts  
- State compression  
- Difference hashing  
- Zero-sum detection  

---

## **Unit 4: Hashing + Other Patterns**

### └─ **Module 4.1: Hashing with Two Pointers**

#### └─ **Submodule 4.1.1: Complement Tracking**
- Lookup-before-insert  
- Pair existence logic  
- Early termination  
- Duplicate handling  

### └─ **Module 4.2: Hashing with Greedy / Scans**

#### └─ **Submodule 4.2.1: Seen-State Greedy**
- Tracking visited states  
- Greedy correctness via memory  
- Pruning via hash  
- Over-counting failures  

---

## **Unit 5: Robustness & Failure Modes**

### └─ **Module 5.1: Hashing Pitfalls**

#### └─ **Submodule 5.1.1: Overuse & Misuse**
- When hashing is unnecessary  
- Hidden O(n²) patterns  
- Memory blowups  
- Hash as a crutch  

### └─ **Module 5.2: Design Judgment**

#### └─ **Submodule 5.2.1: Choosing Hashing vs Alternatives**
- Sorting vs hashing  
- Bitsets vs maps  
- Constraints-driven choice  
- Interview signal quality  