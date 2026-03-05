
---

# 🏗 SmartDB – Architecture Guideline

**Agentic Academic Evaluation System (LangGraph-based, DBMS-aligned)**

---

# 1️⃣ System Purpose

SmartDB is a **database-backed academic evaluation workflow system** that:

- Stores academic entities
    
- Stores answer scripts immutably
    
- Executes AI-based evaluation workflows
    
- Preserves evaluation history (append-only)
    
- Supports re-evaluation
    
- Maintains audit trace
    
- Can scale later without redesign
    

AI assists evaluation.  
Database preserves truth.  
Domain enforces policy.  
LangGraph orchestrates execution.

---

# 2️⃣ High-Level System Structure

```
Frontend
   ↓
Backend (Application Server)
   ├── API Layer
   ├── LangGraph Orchestration
   ├── Domain Services (Authority)
   ├── Repository Layer
   └── Infrastructure (LLM Client)
   ↓
Database (Source of Truth)
```

Each layer has strict ownership.

---

# 3️⃣ Layer Responsibilities

---

## 🖥 3.1 Frontend (Presentation Layer)

Responsibilities:

- Submit answers
    
- View marks
    
- Request re-evaluation
    
- Display evaluation trace
    

It must:

- Never call LLM directly
    
- Never contain business logic
    
- Never assume lifecycle state
    

It talks only to Backend APIs.

---

## 🌐 3.2 Backend (Application Server)

The backend is the system host.

It contains all runtime logic.

It exposes APIs and coordinates workflows.

---

# 4️⃣ Backend Internal Architecture

---

## 4.1 API Layer

Purpose:

- Request validation
    
- Authentication
    
- Authorization
    
- Trigger evaluation workflows
    
- Return structured responses
    

It does NOT:

- Run LLM directly
    
- Write SQL directly
    
- Contain lifecycle rules
    

Example flows:

- POST /submit-answer
    
- POST /evaluate
    
- POST /reevaluate
    
- GET /marks
    

---

## 4.2 LangGraph Orchestration Layer

Purpose:

- Execute AI-based workflows
    
- Route between evaluation steps
    
- Handle conditional branching
    
- Retry LLM if needed
    
- Aggregate intermediate outputs
    

It manages:

- Execution state
    
- Node transitions
    
- Tool invocation
    

It does NOT:

- Own academic lifecycle truth
    
- Enforce institutional rules
    
- Bypass domain services
    

Each evaluation request creates:

- One graph instance
    
- Independent execution state
    

Parallelism = multiple graph instances running concurrently.

---

## 4.3 Domain Layer (Policy Authority)

This is the most important layer.

Domain services enforce:

- Valid state transitions
    
- No mark overwrites
    
- Append-only evaluation runs
    
- Re-evaluation rules
    
- Confidence thresholds
    
- Idempotency
    
- Transactional consistency
    

If LangGraph attempts something illegal,  
Domain rejects it.

Domain is deterministic and testable without LLM.

Examples:

- validate_evaluation_allowed()
    
- record_evaluation_run()
    
- finalize_marks()
    
- request_reevaluation()
    

Domain wraps DB operations in transactions.

---

## 4.4 Repository Layer (Persistence Adapter)

Purpose:

- Execute SQL
    
- Interact with DB
    
- Isolate queries from domain logic
    

It contains:

- ScriptRepository
    
- EvaluationRepository
    
- MarksRepository
    
- AuditRepository
    

Repositories do not contain business rules.

They only implement data access.

---

## 4.5 Infrastructure Layer

Handles:

- LLM client
    
- Rate limiting
    
- Logging
    
- Configuration
    

LLM API key lives here.  
Not in frontend.  
Not in graph node directly.

---

# 5️⃣ Database (Source of Truth)

Your SmartDB schema supports:

- Academic entities
    
- AnswerScript (immutable)
    
- AIEvaluationRun (append-only)
    
- EvaluationDecision
    
- ConfidenceScore
    
- SimilarityIndex
    
- ReevaluationRequest
    
- AuditLog
    

Database responsibilities:

- Foreign key enforcement
    
- Unique constraints
    
- Transactional integrity
    
- Referential consistency
    

Database does NOT enforce lifecycle policy.

---

# 6️⃣ Execution Flow (Single Evaluation)

1. Student submits answer
    
2. Backend stores AnswerScript
    
3. Evaluation job triggered
    
4. LangGraph instance created
    
5. Graph nodes:
    
    - Load script
        
    - Call LLM
        
    - Compute confidence
        
    - Optional similarity
        
    - Produce structured result
        
6. Domain validates result
    
7. Domain persists evaluation run
    
8. Domain optionally finalizes marks
    
9. Audit log entry created
    

All persistent writes go through domain.

---

# 7️⃣ Concurrency & Scaling Strategy

Current phase:

- Single backend instance
    
- Controlled LLM concurrency
    

Future scaling:

- Add job queue
    
- Add worker pool
    
- Run multiple backend instances
    
- Each runs same graph definition
    
- DB remains shared source of truth
    

Scaling does NOT change:

- Domain logic
    
- Graph definition
    
- Schema
    

Only deployment topology changes.

---

# 8️⃣ Invariants (Must Never Break)

- AnswerScript is immutable
    
- Evaluation runs are append-only
    
- Final marks reference specific evaluation decision
    
- Re-evaluation creates new run
    
- Audit log is append-only
    
- Domain rejects illegal transitions
    
- No direct DB writes from graph nodes
    

---

# 9️⃣ What This Architecture Achieves

- Clean separation of concerns
    
- LangGraph used properly
    
- DBMS focus preserved
    
- AI is assistant, not authority
    
- Easy to test domain independently
    
- Future scalability possible
    
- Minimal rewrite required for horizontal scaling
    

---

# 🔟 What This Architecture Avoids

- Graph owning lifecycle
    
- LLM writing directly to DB
    
- Business rules in frontend
    
- Mixed responsibilities
    
- Premature distributed complexity
    
- Overuse of multiple graphs
    

---

# 1️⃣1️⃣ System Evolution Path

Phase 1:

- Single instance
    
- Controlled concurrency
    

Phase 2:

- Add async processing
    
- Add worker pool
    

Phase 3:

- Add distributed scaling
    
- Add monitoring
    
- Add retry & backoff strategies
    

Core architecture remains stable.

---

# Final Mental Model

```
Frontend = Interface
Backend = System host
LangGraph = Execution brain
Domain = Law
Database = Official record
LLM = Expert advisor
```

That is your architectural doctrine.

---

If you want to go deeper next, choose one:

1️⃣ Formal lifecycle state modeling  
2️⃣ Detailed EvaluationGraph node design  
3️⃣ Concurrency control & transaction strategy  
4️⃣ Failure handling & retry architecture

Pick one.