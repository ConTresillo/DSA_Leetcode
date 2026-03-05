# Name: S Tharun Parykshyt
# Roll No: 24BCE1119

# Experiment 1 — CRC Error Detection

## Aim
To implement Cyclic Redundancy Check (CRC) using CRC-8, CRC-16, and CRC-32 generator polynomials for a given binary data stream and analyze their effectiveness in detecting transmission errors.

---

## Theory

Cyclic Redundancy Check (CRC) is an error detection mechanism used in communication networks and storage systems.

The sender and receiver agree on a **generator polynomial**.  
The binary data stream is treated as a polynomial and divided using **modulo-2 division**.

The remainder obtained from the division becomes the **CRC bits**, which are appended to the data to form the transmitted codeword.

### CRC Principle

$$
\large
\text{Codeword } C(x) = D(x)\cdot x^r + R(x)
$$

Where

$$
\large
D(x) \rightarrow \text{Data polynomial}
$$

$$
\large
G(x) \rightarrow \text{Generator polynomial}
$$

$$
\large
r = \deg(G(x))
$$

$$
\large
R(x) \rightarrow \text{Remainder obtained from modulo-2 division}
$$

At the receiver:

$$
\large
C(x) \div G(x)
$$

If the remainder equals zero, the frame is considered correct.

---

### Generator Polynomials

CRC-8

$$
\large
G(x) = x^8 + x^2 + x + 1
$$

CRC-16

$$
\large
G(x) = x^{16} + x^{12} + x^5 + 1
$$

CRC-32

$$
\large
G(x)=x^{32}+x^{26}+x^{23}+x^{22}+x^{16}+x^{12}+x^{11}+x^{10}+x^8+x^7+x^5+x^4+x^2+x+1
$$

---

### Given Data Stream

```
D = 1101011010110101
```

---

## Procedure

### Part (i) Codeword Generation

1. Take the data stream  
2. Append **r zeros** where r is the generator polynomial degree  
3. Perform **modulo-2 division**  
4. Obtain the **remainder (CRC bits)**  
5. Append the remainder to the data to obtain the **codeword**

---

### Part (ii) Error Simulation

1. Introduce a **single-bit error at position 5 from the left**
2. Divide the received frame by the same generator polynomial
3. Check the remainder

If

$$
\large
R(x)=0
$$

No error detected

If

$$
\large
R(x)\neq0
$$

Error detected

---

### Part (iii) Comparison

CRC-8  
- Lower computational cost  
- Detects most simple errors  

CRC-16  
- Higher reliability  
- Widely used in communication protocols  

CRC-32  
- Very high reliability  
- Used in Ethernet and storage systems  

Trade-off:

$$
\large
\text{Higher polynomial degree } \Rightarrow \text{Better reliability but higher computation}
$$

---

## Code

~~~python
# CRC implementation for CRC-8, CRC-16 and CRC-32

def xor(a, b):
    result = []
    for i in range(1, len(b)):
        result.append('0' if a[i] == b[i] else '1')
    return ''.join(result)

def mod2div(dividend, divisor):
    pick = len(divisor)
    tmp = dividend[0:pick]

    while pick < len(dividend):
        if tmp[0] == '1':
            tmp = xor(divisor, tmp) + dividend[pick]
        else:
            tmp = xor('0'*pick, tmp) + dividend[pick]
        pick += 1

    if tmp[0] == '1':
        tmp = xor(divisor, tmp)
    else:
        tmp = xor('0'*pick, tmp)

    return tmp

def encode_data(data, key):
    l_key = len(key)
    appended_data = data + '0'*(l_key-1)
    remainder = mod2div(appended_data, key)
    codeword = data + remainder
    return remainder, codeword

def introduce_error(frame, position):
    frame = list(frame)
    frame[position] = '1' if frame[position] == '0' else '0'
    return ''.join(frame)

data = "1101011010110101"

crc8  = "100000111"
crc16 = "10001000000100001"
crc32 = "100000100110000010001110110110111"

for name, poly in [("CRC-8", crc8), ("CRC-16", crc16), ("CRC-32", crc32)]:
    rem, codeword = encode_data(data, poly)
    print(name)
    print("Remainder:", rem)
    print("Codeword:", codeword)

    error_frame = introduce_error(codeword, 4)
    check = mod2div(error_frame, poly)

    print("Received Frame:", error_frame)
    print("Receiver Remainder:", check)
    print()
~~~

---

## Output

![[Pasted image 20260304140037.png]]

---

## Result

CRC-8, CRC-16, and CRC-32 were implemented for the given data stream.  
The codewords were generated and a single-bit error was introduced.  
Higher degree CRC polynomials showed greater reliability in detecting errors.

---

# Experiment 2 — Checksum Error Detection

## Aim
To compute checksums for block sizes of **8 bits, 16 bits, and 32 bits** and analyze their ability to detect transmission errors.

---

## Theory

Checksum is a simple error detection technique used in networking protocols such as TCP and UDP.

The data is divided into fixed-size blocks and added using **1’s complement addition**.

The complement of the sum is transmitted as the checksum.

### Checksum Formula

$$
\large
\text{Checksum} = \overline{\sum \text{Data Blocks}}
$$

Where

$$
\large
\sum \text{Data Blocks}
$$

represents addition using **1’s complement arithmetic**.

At the receiver

$$
\large
\text{Sum(Data + Checksum)} = 111...111
$$

If true → No error

Otherwise → Error detected

---

### Given Data Stream

```
D = 11001010111100011010101100101101
```

---

## Procedure

### Part (i) Checksum Generation

1. Divide the data into blocks of size **8 bits**
2. Perform **1’s complement addition**
3. Compute the complement of the result
4. Append the checksum to the frame
5. Repeat for **16-bit and 32-bit blocks**

---

### Part (ii) Error Detection

1. Introduce a **single-bit error at the 10th bit**
2. At the receiver recompute the checksum
3. Verify whether the final result equals all ones

---

## Code

~~~python
def ones_complement_addition(a, b):
    result = bin(int(a,2) + int(b,2))[2:]

    if len(result) > len(a):
        carry = result[0]
        result = result[1:]
        result = bin(int(result,2) + int(carry,2))[2:]

    return result.zfill(len(a))

def checksum(data, block_size):
    blocks = [data[i:i+block_size] for i in range(0,len(data),block_size)]
    total = blocks[0]

    for block in blocks[1:]:
        total = ones_complement_addition(total, block)

    checksum = ''.join('1' if b=='0' else '0' for b in total)
    return checksum

def introduce_error(data, pos):
    data = list(data)
    data[pos] = '1' if data[pos]=='0' else '0'
    return ''.join(data)

data = "11001010111100011010101100101101"

for size in [8,16,32]:
    cs = checksum(data, size)
    print("Block Size:", size)
    print("Checksum:", cs)

    transmitted = data + cs

    received = introduce_error(transmitted,9)

    print("Received Frame:", received)
    print()
~~~

---

## Output

![[Pasted image 20260304140113.png]]

---

## Result

Checksums were generated for 8-bit, 16-bit, and 32-bit block sizes.  
A single-bit error was introduced and the checksum was recomputed at the receiver to verify error detection capability.
