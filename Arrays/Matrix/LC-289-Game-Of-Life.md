## Code
```java
class Solution {

    int kMax = 8;

    //                 N   NE E  SE S   SW  W   NW

        int[] rows = {-1, -1, 0, 1, 1,  1,  0, -1};

        int[] cols = { 0,  1, 1, 1, 0, -1, -1, -1};

  

    boolean isValidBounds(int m, int n, int i, int j){

        return (i>-1 && i<m) && (j>-1 && j<n);

    }

  

    int checkNeighbors(int[][] board, int m, int n, int i, int j){

        //Clock Wise Check

  

        int cnt = 0;

        int r;

        int s;

        int k = 0;

  

        while(k < kMax){

            r = i + rows[k];

            s = j + cols[k];

            if(isValidBounds(m,n,r,s) && board[r][s]%10==1) cnt++;

            k++;

        }

  

        return cnt;

    }

  

    //Start

    public void gameOfLife(int[][] board) {

        //Global Init

        int t=0;

        int T=1;

        int m = board.length;

        int n = board[0].length;

        /*

        t -> current Time

        T -> stop Time

        m -> no. of rows

        n -> no. of cols

        If Dimensions change ingame move to Local Init

        */

  

        //Start Loop

        while (t < T){

            //Local Init

            int cnt;

            int cur;

            int nxt=0;

            /*

            cnt -> no. of alive neighbors

            cur -> current state of being

            nxt -> next state of being

            */

  

            //Start Check Loop

            for(int i=0; i<m; i++){

                for(int j=0; j<n; j++){

                    cur = board[i][j] % 10;

                    nxt = 0;

                    //Mod 10 because we just need units place

  

                    //Find Next State

                    //Check Neighbors

                    cnt = checkNeighbors(board,m,n,i,j);

  

                    //Decide

                    if(cur == 1){

                        if(cnt < 2) nxt = 0;

                        else if(cnt>=2 && cnt <= 3) nxt = 1;

                        else if(cnt > 3) nxt = 0;

                    }

                    else if(cur == 0){

                        if(cnt == 3) nxt=1;

                    }

  

                    //Encode

                    board[i][j] = nxt*10 + cur;

                }

            }

  

            //Once this is done

            //Decode

            for(int i=0; i<m; i++){

                for(int j=0; j<n; j++){

                    board[i][j] = board[i][j]/10;

                }

            }

            t++;

        }

    }

}
```

## FlowChart
![[Pasted image 20260105120820.png]]

## 🧠 Learning Summary — Game of Life (Deep Technical Post-Mortem)

---

## 🟢 1️⃣ Mental Model vs Memorization

- **Primary mental representation**: synchronous **state-transition system on a grid** with a strict _time-layer invariant_.
    
- **What would require memorization without this model**:
    
    - All 4 Game-of-Life rules and their edge cases.
        
    - Special handling to “remember” which cells were updated first.
        
- **What replaced memorization**:
    
    - Invariant: _during decision phase, every read observes state at time t_.
        
    - Enforced by encoding `(old_state, new_state)` in the same cell.
        
- **Code linkage**:
    
    - `board[i][j] % 10` enforces “read old state only”.
        
    - `board[i][j] = nxt*10 + cur` enforces “write without commit”.
        
- **Effect**:
    
    - Correctness flows from invariant preservation, not rule recall.
        

---

## 🔵 2️⃣ Problem Classification

- **Rejected classification #1: Dynamic Programming**
    
    - Assumption: overlapping subproblems with reusable results.
        
    - Breaks here: neighbor counts depend on _current frame_, not reusable subresults.
        
    - Bug if misclassified: caching neighbor counts → stale values across frames.
        
- **Rejected classification #2: Recursion**
    
    - Assumption: problem decomposes into smaller independent subinstances.
        
    - Breaks here: all cells depend on the same global time slice.
        
    - Bug if misclassified: depth-first updates → mixed-time reads.
        
- **Correct classification**: **synchronous grid simulation / cellular automaton**.
    
    - Properties mapped to code:
        
        - global time step → two passes (encode, then decode),
            
        - local dependency → `checkNeighbors`,
            
        - simultaneity → old/new state separation.
            
- **Bug class from misclassification**:
    
    - Temporal inconsistency (reading partially updated neighbors).
        

---

## 🟣 3️⃣ Design Decisions (Explicit or Implicit)

- **Decision: two-pass algorithm**
    
    - Constraint: simultaneity of updates.
        
    - Rejected alternative: single-pass overwrite.
        
    - Failure mode: neighbor counts polluted by new states.
        
- **Decision: in-place decimal encoding**
    
    - Constraint: O(1) auxiliary space.
        
    - Rejected alternative: separate `nextBoard`.
        
    - Failure mode avoided: extra memory, but not correctness; chosen tradeoff.
        
- **Decision: helper `checkNeighbors`**
    
    - Constraint: isolate aggregation logic.
        
    - Rejected alternative: inline neighbor loops.
        
    - Failure mode: harder to enforce `%10` invariant consistently.
        
- **Decision: explicit decode phase**
    
    - Constraint: clean commit boundary.
        
    - Rejected alternative: commit per cell.
        
    - Failure mode: mixed-time grid.
        

---

## 🟡 4️⃣ Reasoning Artifacts

- **Flowchart**
    
    - Ruled out illegal state: “read after commit”.
        
    - Prevented bug class: temporal state corruption.
        
    - Reflected in code by strict ordering: decision loop → decode loop.
        
- **Implicit invariant list**
    
    - `old_state == cell % 10` must hold until decode.
        
    - Reflected in every neighbor read.
        
- **Effect on search space**
    
    - Eliminated all designs that interleave read/write.
        
    - Reduced problem to enforcing one invariant correctly.
        

**Generalizable takeaways**

- If a future problem has _simultaneous updates_, enforce _time separation_ using _explicit state layering_.
    
- If correctness depends on read purity, encode the read layer directly in data representation.
    

---

## 🟠 5️⃣ Formal Algorithm Description

- **Preconditions**
    
    - Grid entries ∈ {0,1}.
        
- **Transformations**
    
    1. For each cell, count neighbors using `cell % 10`.
        
    2. Compute `nxt` via transition function.
        
    3. Encode `(nxt, cur)` into same cell.
        
    4. Decode all cells by integer division.
        
- **Execution invariants**
    
    - Local: neighbor reads always apply `% 10`.
        
    - Global: no cell is decoded before all cells are encoded.
        
- **Postcondition**
    
    - Grid represents state at time `t+1`.
        

---

## 🔴 6️⃣ Error & Bug Taxonomy

- **Missing `k++`**
    
    - Type: control-flow bug.
        
    - Violated invariant: loop termination.
        
    - Consequence: infinite loop → non-termination (explosive).
        
- **Missing `% 10` in neighbor read**
    
    - Type: state-handling bug.
        
    - Violated invariant: read-only old state.
        
    - Consequence: mixed-time neighbor counts → silent wrong output.
        
- **Stray semicolon after `if`**
    
    - Type: logic bug.
        
    - Violated invariant: transition function correctness.
        
    - Consequence: unconditional revival → silent wrong output.
        
- **Not resetting `nxt`**
    
    - Type: variable-lifetime bug.
        
    - Violated invariant: cell independence.
        
    - Consequence: state leakage across cells → silent wrong output.
        

---

## 🟣 7️⃣ Structural Code Analysis

- **`isValidBounds`**
    
    - Protects geometry invariant.
        
    - Merging into loops risks inconsistent boundary checks.
        
- **`checkNeighbors`**
    
    - Protects aggregation invariant (exactly 8 checks).
        
    - Inlining risks missing `%10` in some branches.
        
- **Main loop vs decode loop**
    
    - Protects global commit invariant.
        
    - Reordering would cause partial commits and temporal corruption.
        
- **Structure as correctness**
    
    - Each function boundary corresponds to an invariant boundary.
        

---

## 🔵 8️⃣ Constraints & Tradeoffs

- **Time**: O(m·n).
    
- **Space**: O(1) extra.
    
- **Dominant constraint**: simultaneity + in-place requirement.
    
- **Unavoidable tradeoff**:
    
    - Two passes instead of one.
        
- **Chosen tradeoff**:
    
    - Decimal encoding over bit encoding (clarity > bit-level compactness).
        
- **If constraints changed**:
    
    - Allowing extra space → simpler single-pass using auxiliary grid.
        

---

## 🧩 9️⃣ Interview / Real-World Signal

- **Demonstrates**
    
    - Ability to enforce temporal invariants.
        
    - Discipline in in-place state transitions.
        
    - Correct handling of partial mutation.
        
- **Does not demonstrate**
    
    - DP optimization skills.
        
    - Recursive decomposition.
        
- **Observable evidence**
    
    - Explicit state layering (`%10`, `/10`).
        
    - Strict phase separation in code structure.
    - 