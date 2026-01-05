# 🔢 State Encoding & Decoding  
## Binary (Bitwise) and Decimal (Positional) Techniques

---

## 0️⃣ What Problem Encoding Solves (Formal)

Encoding is required when:

- Multiple **logical states** must be stored in **one physical variable**
- Reads must observe **only a specific layer**
- Writes must **not destroy other layers**
- Extra memory is constrained or disallowed

**Formal goal**

Encode:
```
(s₀, s₁, …, sₖ) → x
```

Such that:
- each `sᵢ` is independently retrievable
- updating `sᵢ` does not corrupt other layers

---

## 1️⃣ Binary Encoding (Bitwise)

### 1.1 Representation Model

Each **bit** represents one boolean or bounded state.

Example (2 layers):

```
bit 1 | bit 0
 new  | old
```

| Integer | Binary | old | new |
|--------|--------|-----|-----|
| 0 | 00 | 0 | 0 |
| 1 | 01 | 1 | 0 |
| 2 | 10 | 0 | 1 |
| 3 | 11 | 1 | 1 |

---

### 1.2 Encoding (Set a Layer)

```java
x |= (v << k);
```

---

### 1.3 Decoding (Extract a Layer)

```java
layer = (x >> k) & 1;
```

---

### 1.4 Commit (Collapse Layers)

```java
x >>= 1;
```

---

## 2️⃣ Decimal Encoding (Positional)

### 2.1 Representation Model

```
tens | units
 new | old
```

---

### 2.2 Encoding

```java
x = new * 10 + old;
```

---

### 2.3 Decoding

```java
old = x % 10;
new = x / 10;
```

---

## 3️⃣ Binary vs Decimal Comparison

| Aspect | Binary | Decimal |
|------|--------|---------|
| Scalability | High | Low |
| Safety | High | Risky |
| Use case | Production | Learning |

---

## 4️⃣ Common Failure Modes

- Reading without masking
- Overwriting instead of merging
- Overflow in decimal encoding
- Mixed base decoding

---

## 5️⃣ Encoding Checklist

- Number of layers?
- Max value per layer?
- Overflow bound?
- Read/write discipline?
