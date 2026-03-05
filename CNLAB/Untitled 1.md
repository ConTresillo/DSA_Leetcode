## 🧩 Atomic Capabilities — Socket Programming (Building Blocks Only)

### 1️⃣ Socket Lifecycle

- Create socket
    
- Bind address
    
- Listen (TCP)
    
- Accept (TCP)
    
- Connect (TCP)
    
- Close socket
    

---

### 2️⃣ Data Transmission

- Send bytes (TCP)
    
- Receive bytes (TCP)
    
- Send datagram (UDP)
    
- Receive datagram (UDP)
    
- Encode string → bytes
    
- Decode bytes → string
    

---

### 3️⃣ Connection Management (TCP)

- Maintain listening socket
    
- Maintain per-client socket
    
- Detect disconnect (`recv() == 0`)
    
- Handle connection backlog
    
- Graceful shutdown
    

---

### 4️⃣ Client State Management

- Store client address
    
- Store client name
    
- Maintain client list
    
- Add client
    
- Remove client
    
- Lookup client
    
- Broadcast to all
    
- Send to specific client
    

---

### 5️⃣ Concurrency

- Create thread
    
- Start thread
    
- Join thread
    
- Handle blocking `recv()`
    
- Thread per client model
    
- Shared resource access
    

---

### 6️⃣ Message Framing / Protocol Design

- Define first packet rule
    
- Send metadata
    
- Send file size
    
- Chunked transfer
    
- Accumulate bytes
    
- Termination condition
    
- Custom application protocol
    

---

### 7️⃣ File Handling

- Open file (read binary)
    
- Open file (write binary)
    
- Read chunk
    
- Write chunk
    
- Get file size
    
- Extract file name
    

---

### 8️⃣ Error Handling

- Try/except
    
- Detect malformed input
    
- Validate numeric data
    
- Handle abrupt disconnect
    
- Handle invalid expression
    

---

### 9️⃣ Application Logic Variants

- Echo
    
- Broadcast chat
    
- One-to-one messaging
    
- File transfer
    
- Calculator service
    
- Request-response model
    
- Continuous session model
    

---

### 🔟 UDP-Specific

- Stateless communication
    
- Track sender via address
    
- Register client via first message
    
- Forward datagrams
    

---

### 1️⃣1️⃣ Advanced Extensions

- Multi-client support
    
- Timeout handling
    
- Buffer sizing
    
- Port reuse
    
- Firewall allowance
    
- Logging
    
- Progress tracking
    
- Resume transfer
    

---

If you want, next I can group them into **true minimal primitives vs derived capabilities** (clean abstraction hierarchy).
# 🧩 Group 1 — Socket Lifecycle (Core Transport Primitives)

We focus only on:

- Create
    
- Bind
    
- Listen
    
- Accept
    
- Connect
    
- Close
    

No application logic. Only lifecycle.

---

# 1️⃣ Create Socket

## What It Really Means

You are asking the OS:

> “Give me a network endpoint.”

Python:

```python
import socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
```

### Parameters

- `AF_INET` → IPv4
    
- `SOCK_STREAM` → TCP
    
- `SOCK_DGRAM` → UDP
    

### Important Reality

This does **not**:

- Bind to a port
    
- Connect anywhere
    
- Listen
    

It only allocates a socket descriptor.

---

# 2️⃣ Bind

## What It Means

Attach socket to:

```
(IP, PORT)
```

Example:

```python
s.bind(("0.0.0.0", 5000))
```

### Atomic Effect

- Reserves port 5000
    
- OS routes traffic for that port to your process
    

### Key Rule

Only one process can bind to the same port (unless special reuse flags).

---

# 3️⃣ Listen (TCP Only)

```python
s.listen(5)
```

### What It Does

Switches socket to **passive mode**.

The number `5` is:

```
backlog queue size
```

Meaning:

- Up to 5 pending connection requests can wait.
    

### Important

UDP does NOT use `listen()`.

---

# 4️⃣ Accept (TCP Only)

```python
conn, addr = s.accept()
```

### What Happens Internally

When a client connects:

- OS completes 3-way handshake
    
- `accept()` returns a NEW socket
    

You now have:

|Socket|Purpose|
|---|---|
|`s`|Listening socket|
|`conn`|Client communication socket|

### Critical Concept

Server always keeps:

- 1 listening socket
    
- N client sockets
    

---

# 5️⃣ Connect (Client Side, TCP)

```python
s.connect(("192.168.1.10", 5000))
```

### What It Does

- Sends SYN
    
- Completes 3-way handshake
    
- Establishes virtual connection
    

After this:

- `send()` and `recv()` are allowed.
    

UDP does not require this.

---

# 6️⃣ Close

```python
s.close()
```

### Effect

- Releases OS resources
    
- Frees port
    
- Terminates connection
    

Important:  
If you forget `close()`:

- Port may remain in `TIME_WAIT`
    
- You cannot bind again immediately
    

---

# 🧠 Complete Minimal Lifecycle — TCP Server

```python
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind(("0.0.0.0", 5000))
s.listen(1)

conn, addr = s.accept()

conn.close()
s.close()
```

No send/recv. Just lifecycle.

---

# 🧠 Complete Minimal Lifecycle — TCP Client

```python
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect(("SERVER_IP", 5000))
s.close()
```

---

# 🧠 Minimal UDP Lifecycle

Server:

```python
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
s.bind(("0.0.0.0", 6000))
s.close()
```

Client:

```python
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
s.close()
```

No listen. No accept. No connect required.

---

# 🔎 Mental Model (Most Important)

## TCP Server Lifecycle

```
Create
Bind
Listen
Accept
Communicate
Close
```

## TCP Client Lifecycle

```
Create
Connect
Communicate
Close
```

## UDP Lifecycle

```
Create
Bind (server only)
Send/Receive
Close
```

---

# ⚠️ Common Mistakes

- Forgetting `bind()` before `listen()`
    
- Calling `accept()` without `listen()`
    
- Trying `listen()` on UDP
    
- Forgetting to close
    
- Binding to wrong IP
    

---

If you want next:

- 🔬 Deep internal OS-level explanation  
    or
    
- 🧠 Visualization of TCP handshake  
    or
    
- 🛠 Edge cases (TIME_WAIT, port reuse, multiple binds)  
    or
    
- Move to Group 2 (Data Transmission)
# 🧩 Group 2 — Data Transmission (Core Communication Primitives)

Focus only on:

- send
    
- recv
    
- sendto
    
- recvfrom
    
- encode
    
- decode
    

No threading. No protocol logic. Just raw transfer.

---

# 1️⃣ TCP — `send()`

```python
conn.send(b"Hello")
```

or

```python
conn.send("Hello".encode())
```

### Atomic Effect

- Pushes bytes into OS transmit buffer
    
- Returns number of bytes accepted
    

---

# 2️⃣ TCP — `recv()`

```python
data = conn.recv(1024)
```

### Atomic Effect

- Pulls up to 1024 bytes from receive buffer
    
- Returns `b""` if connection closed
    

Important:

- Not guaranteed to return full message
    
- Returns whatever is available
    

---

# 3️⃣ UDP — `sendto()`

```python
socket.sendto(b"Hello", ("192.168.1.10", 6000))
```

### Atomic Effect

- Sends one datagram
    
- No connection required
    

---

# 4️⃣ UDP — `recvfrom()`

```python
data, addr = socket.recvfrom(1024)
```

### Atomic Effect

- Receives one datagram
    
- Returns sender address
    

---

# 5️⃣ Encoding

```python
message.encode()
```

String → bytes

---

# 6️⃣ Decoding

```python
data.decode()
```

Bytes → string

---

# 🧠 Minimal TCP Transfer Example

Server:

```python
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind(("0.0.0.0", 5000))
s.listen(1)

conn, addr = s.accept()

data = conn.recv(1024)
conn.send(b"OK")

conn.close()
s.close()
```

Client:

```python
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect(("SERVER_IP", 5000))

s.send(b"Hello")
reply = s.recv(1024)

s.close()
```

---

# 🧠 Minimal UDP Transfer Example

Server:

```python
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
s.bind(("0.0.0.0", 6000))

data, addr = s.recvfrom(1024)
s.sendto(b"OK", addr)

s.close()
```

Client:

```python
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

s.sendto(b"Hello", ("SERVER_IP", 6000))
data, addr = s.recvfrom(1024)

s.close()
```

---

# 🔎 Critical Realities

TCP:

- Stream-based
    
- No message boundaries
    
- May split or merge sends
    

UDP:

- Message-based
    
- Preserves boundaries
    
- Entire datagram delivered or dropped
    

---

Next group:

- 🧠 Group 3 — Connection Management (Disconnect detection, buffering behavior, blocking, shutdown)
# 🧩 Group 3 — Connection Management (Control-Level Primitives)

This group is about **managing the life of a connection**, not sending data.

Atomic capabilities only.

---

# 1️⃣ Blocking vs Non-Blocking Mode

### Default (Blocking)

```python
socket.setblocking(True)
```

### Non-Blocking

```python
socket.setblocking(False)
```

Effect:

- Blocking → `recv()` waits
    
- Non-blocking → `recv()` raises exception if no data
    

---

# 2️⃣ Timeout Control

```python
socket.settimeout(seconds)
```

Examples:

```python
conn.settimeout(30)
```

Atomic effect:

- `recv()` waits up to 30 seconds
    
- Raises `socket.timeout` if no data
    

---

# 3️⃣ Detect Client Disconnect

```python
data = conn.recv(1024)

if not data:
    # client closed connection
```

Atomic rule:

- Empty bytes (`b""`) = TCP FIN received
    

---

# 4️⃣ Graceful Shutdown (Half-Close)

```python
socket.shutdown(socket.SHUT_WR)
```

Modes:

|Mode|Meaning|
|---|---|
|SHUT_RD|Stop receiving|
|SHUT_WR|Stop sending|
|SHUT_RDWR|Stop both|

Used when:

- You want to signal end of sending but still receive
    

---

# 5️⃣ Handle Abrupt Disconnect

Use exception handling:

```python
try:
    data = conn.recv(1024)
except ConnectionResetError:
    # client crashed
```

Atomic capability:

- Detect RST (reset)
    

---

# 6️⃣ Backlog Queue Control

```python
server_socket.listen(5)
```

Atomic meaning:

- Up to 5 pending connection attempts stored by OS
    

---

# 7️⃣ Port Reuse

```python
server_socket.setsockopt(
    socket.SOL_SOCKET,
    socket.SO_REUSEADDR,
    1
)
```

Atomic capability:

- Rebind to port even if in TIME_WAIT
    

---

# 8️⃣ Connection State Awareness

Possible states (conceptual):

- LISTEN
    
- SYN_RECEIVED
    
- ESTABLISHED
    
- FIN_WAIT
    
- TIME_WAIT
    
- CLOSED
    

Atomic capability:

- Understand lifecycle progression
    

---

# 9️⃣ Keep Connection Alive (Optional)

```python
socket.setsockopt(
    socket.SOL_SOCKET,
    socket.SO_KEEPALIVE,
    1
)
```

Atomic meaning:

- OS periodically checks if peer is alive
    

---

# 🔟 Buffer Size Awareness

```python
socket.recv(buffer_size)
```

Atomic capability:

- Control maximum bytes per read
    
- Avoid partial-message assumptions
    

---

# 1️⃣1️⃣ Graceful Server Shutdown

Close in correct order:

```python
conn.close()
server_socket.close()
```

Atomic capability:

- Release client first
    
- Then stop listening
    

---

# 🔎 Minimal Managed Server Example

```python
import socket

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
server.bind(("0.0.0.0", 5006))
server.listen(5)

conn, addr = server.accept()
conn.settimeout(60)

try:
    while True:
        data = conn.recv(1024)
        if not data:
            break
        conn.send(data)
except socket.timeout:
    print("Client inactive")
except ConnectionResetError:
    print("Client crashed")

conn.close()
server.close()
```

---

# 🧠 What Group 3 Really Gives You

You now control:

- How long to wait
    
- When to terminate
    
- How to detect idle
    
- How to detect crash
    
- How to reuse ports
    
- How to close safely
    
- How many clients can queue
    

---

Next group:

- 🧵 Group 4 — Client State Management (multi-client structures)  
    or
    
- ⚙ Group 5 — Concurrency Models (threads, select, async)