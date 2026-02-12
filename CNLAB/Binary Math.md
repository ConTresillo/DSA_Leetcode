# 🧩 Bit-Level Utilities in Python (Reusable for CRC, Checksum, etc.)

Goal:

Build **clean reusable binary helper functions** so you can:

- Do binary addition manually
- Do subtraction
- Compute 1’s & 2’s complement
- Perform XOR
- Perform left/right shift
- Perform mod-2 division (CRC base)

Everything implemented at **bit-string level**  
(no relying on Python’s built-in integer arithmetic).

---

# 🔵 1️⃣ Binary Addition (Manual Bit-by-Bit)

### 🔷 Concept

Algorithm:

```
Start from right
Add bit1 + bit2 + carry
result bit = sum % 2
carry = sum // 2
Move left
```

---

### 🖥️ Implementation

```python
def binary_add(a, b):
    # Make equal length
    max_len = max(len(a), len(b))
    a = a.zfill(max_len)
    b = b.zfill(max_len)

    carry = 0
    result = ""

    for i in range(max_len - 1, -1, -1):
        total = int(a[i]) + int(b[i]) + carry
        result = str(total % 2) + result
        carry = total // 2

    if carry:
        result = "1" + result

    return result
```

---

# 🟢 2️⃣ Binary Subtraction (Using Borrow)

### 🔷 Concept

Manual borrow logic:

```
If a[i] < b[i]:
    borrow from left
```

---

### 🖥️ Implementation

```python
def binary_subtract(a, b):
    max_len = max(len(a), len(b))
    a = a.zfill(max_len)
    b = b.zfill(max_len)

    result = ""
    borrow = 0

    for i in range(max_len - 1, -1, -1):
        diff = int(a[i]) - int(b[i]) - borrow

        if diff >= 0:
            result = str(diff) + result
            borrow = 0
        else:
            result = str(diff + 2) + result
            borrow = 1

    return result.lstrip('0') or "0"
```

---

# 🟡 3️⃣ 1’s Complement

### 🔷 Concept

Flip every bit.

---

### 🖥️ Implementation

```python
def ones_complement(binary):
    result = ""
    for bit in binary:
        if bit == '0':
            result += '1'
        else:
            result += '0'
    return result
```

---

# 🔴 4️⃣ 2’s Complement

### 🔷 Concept

```
2’s complement = 1’s complement + 1
```

---

### 🖥️ Implementation

```python
def twos_complement(binary):
    ones = ones_complement(binary)
    return binary_add(ones, "1")
```

---

# 🟣 5️⃣ XOR (Used in CRC)

### 🔷 Concept

```
0 XOR 0 = 0
1 XOR 1 = 0
1 XOR 0 = 1
0 XOR 1 = 1
```

---

### 🖥️ Implementation

```python
def xor(a, b):
    result = ""
    for i in range(len(a)):
        if a[i] == b[i]:
            result += '0'
        else:
            result += '1'
    return result
```

---

# 🟤 6️⃣ Left Shift & Right Shift

### 🔷 Left Shift

Append zeros to right.

```python
def left_shift(binary, n):
    return binary + "0" * n
```

---

### 🔷 Right Shift

Remove bits from right.

```python
def right_shift(binary, n):
    return binary[:-n] if n < len(binary) else "0"
```

---

# ⚫ 7️⃣ Mod-2 Division (Core of CRC)

### 🔷 Concept

Binary division using XOR (no subtraction).

Algorithm:

```
Take first n bits (length of divisor)
If first bit is 1 → XOR with divisor
Else → XOR with 0...0
Shift left
Repeat
```

---

### 🖥️ Implementation

```python
def mod2_division(dividend, divisor):
    pick = len(divisor)
    temp = dividend[:pick]

    while pick < len(dividend):
        if temp[0] == '1':
            temp = xor(divisor, temp) + dividend[pick]
        else:
            temp = xor('0' * pick, temp) + dividend[pick]
        pick += 1

    if temp[0] == '1':
        temp = xor(divisor, temp)
    else:
        temp = xor('0' * pick, temp)

    return temp
```

---

# 🟡 8️⃣ Binary to Integer (If Needed)

```python
def binary_to_int(binary):
    value = 0
    power = 0
    for bit in reversed(binary):
        if bit == '1':
            value += 2 ** power
        power += 1
    return value
```

---

# 🟢 9️⃣ Integer to Binary (Manual)

```python
def int_to_binary(n):
    if n == 0:
        return "0"

    result = ""
    while n > 0:
        result = str(n % 2) + result
        n = n // 2
    return result
```

---

# 🧠 Recommended Structure for Your Notebook

Create a file:

```
bit_utils.py
```

Put all these functions inside.

Then in CRC, Checksum, etc:

```python
from bit_utils import *
```

Reuse directly.

---

# 🧪 Quick Test

```python
print(binary_add("1011", "1101"))
print(binary_subtract("1010", "0011"))
print(twos_complement("0101"))
print(xor("1101", "1011"))
```

---

Now you have:

- Full manual bit arithmetic
- No dependency on Python integers
- Ready for CRC
- Ready for Checksum
- Ready for Hamming

---
