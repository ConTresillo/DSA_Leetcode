# 🧩 PART A — Basic LAN Communication Using Socket Programming

We start with the **simplest possible message exchange between two computers in the same LAN**.

We will implement:

1. TCP message exchange
    
2. UDP message exchange
    

Language used: **Python**  
Goal: **Minimal, exam-friendly, logically structured code**

---

# 🔵 A1 — TCP Message Exchange

## 🔷 Concept 1: What TCP Does

TCP is:

- Connection-oriented
    
- Reliable (guaranteed delivery)
    
- Ordered (no packet disorder)
    

Flow:

```
Server:
    Create socket
    Bind to IP + Port
    Listen
    Accept connection
    Receive
    Send
    Close

Client:
    Create socket
    Connect
    Send
    Receive
    Close
```

---

## 🖥️ TCP SERVER (Minimal)

```python
import socket

# 1. Create socket
server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# 2. Bind to IP and Port
server_socket.bind(("0.0.0.0", 5000))

# 3. Listen for connections
server_socket.listen(1)

print("Server waiting for connection...")

# 4. Accept connection
conn, addr = server_socket.accept()
print("Connected from:", addr)

# 5. Receive message
data = conn.recv(1024).decode()
print("Client says:", data)

# 6. Send reply
reply = input("Enter reply: ")
conn.send(reply.encode())

# 7. Close connection
conn.close()
server_socket.close()
```

---

## 🖥️ TCP CLIENT (Minimal)

```python
import socket

# 1. Create socket
client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# 2. Connect to server IP
client_socket.connect(("SERVER_IP_HERE", 5000))

# 3. Send message
msg = input("Enter message: ")
client_socket.send(msg.encode())

# 4. Receive reply
reply = client_socket.recv(1024).decode()
print("Server replied:", reply)

# 5. Close
client_socket.close()
```

Replace `"SERVER_IP_HERE"` with the actual LAN IP of the server machine.

To find server IP:

```
ipconfig   (Windows)
ifconfig   (Linux/Mac)
```

---

## 🧠 TCP Concepts (Grouped Explanation)

### 1️⃣ Socket Creation

```
socket(AF_INET, SOCK_STREAM)
```

- AF_INET → IPv4
    
- SOCK_STREAM → TCP
    

---

### 2️⃣ Binding

Server binds to:

```
(IP, PORT)
```

Port = logical door number.

---

### 3️⃣ Listening & Accepting

- `listen()` → allow incoming connections
    
- `accept()` → returns new socket for communication
    

Important:  
Server uses **two sockets**:

- Listening socket
    
- Connected socket
    

---

### 4️⃣ Sending & Receiving

```
send()   → send bytes
recv()   → receive bytes
```

We use:

```
.encode()
.decode()
```

Because sockets transmit bytes, not strings.

---

# 🟢 A2 — UDP Message Exchange

## 🔷 Concept 1: What UDP Does

UDP is:

- Connectionless
    
- No guarantee of delivery
    
- Faster
    
- No handshake
    

Flow:

```
Server:
    Create socket
    Bind
    Receive
    Send back

Client:
    Create socket
    Send
    Receive
```

No `connect()` needed.

---

## 🖥️ UDP SERVER (Minimal)

```python
import socket

# 1. Create socket
server_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

# 2. Bind
server_socket.bind(("0.0.0.0", 6000))

print("UDP Server waiting...")

# 3. Receive
data, addr = server_socket.recvfrom(1024)
print("Received from", addr, ":", data.decode())

# 4. Send reply
reply = input("Enter reply: ")
server_socket.sendto(reply.encode(), addr)

server_socket.close()
```

---

## 🖥️ UDP CLIENT (Minimal)

```python
import socket

# 1. Create socket
client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

server_address = ("SERVER_IP_HERE", 6000)

# 2. Send
msg = input("Enter message: ")
client_socket.sendto(msg.encode(), server_address)

# 3. Receive
data, addr = client_socket.recvfrom(1024)
print("Server replied:", data.decode())

client_socket.close()
```

---

## 🧠 UDP Concepts (Grouped Explanation)

### 1️⃣ SOCK_DGRAM

Indicates UDP protocol.

---

### 2️⃣ No Connection

There is:

- No `listen()`
    
- No `accept()`
    
- No `connect()`
    

Because UDP sends packets directly.

---

### 3️⃣ recvfrom() vs recv()

TCP:

```
recv()
```

UDP:

```
recvfrom()
```

Returns:

```
(data, sender_address)
```

---

## ⚖️ TCP vs UDP (Exam Comparison)

|Feature|TCP|UDP|
|---|---|---|
|Connection|Yes|No|
|Reliable|Yes|No|
|Ordered|Yes|No|
|Speed|Slower|Faster|
|Used in|File transfer, HTTP|Streaming, DNS|

---

## ✅ How To Run in LAN

1. Run server first
    
2. Find server IP
    
3. Replace IP in client code
    
4. Run client on same or different LAN machine
    
5. Ensure firewall allows port
    

---

# 🧩 PART B1 — TCP Echo Server

Now we implement a proper **Echo Server using TCP**.

Goal:

- Client sends a message
    
- Server sends the same message back
    
- While displaying, print:
    
    - Client IP
        
    - Client Port
        
    - Client Name
        

Keep it **minimal and exam-friendly**.

---

## 🔷 Concept 1 — What is an Echo Server?

An echo server:

```
Client → sends message
Server → receives
Server → sends SAME message back
```

It does not modify data.  
It just mirrors it.

---

## 🔷 Concept 2 — Handling One Client Cleanly

Flow:

```
Create socket
Bind
Listen
Accept
Receive in loop
Send back same data
Close when client exits
```

We ask the client to send their **name first**, so the server can display it.

---

## 🖥️ TCP ECHO SERVER (Minimal + Clear)

```python
import socket

server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.bind(("0.0.0.0", 5001))
server_socket.listen(1)

print("Echo Server waiting...")

conn, addr = server_socket.accept()

client_ip = addr[0]
client_port = addr[1]

# First message = client name
client_name = conn.recv(1024).decode()
print("Connected:", client_name, client_ip, client_port)

while True:
    data = conn.recv(1024).decode()

    if not data:
        break

    print("Message from", client_name, client_ip, client_port, ":", data)

    # Echo back same message
    conn.send(data.encode())

conn.close()
server_socket.close()
```

---

## 🖥️ TCP ECHO CLIENT

```python
import socket

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect(("SERVER_IP_HERE", 5001))

name = input("Enter your name: ")
client_socket.send(name.encode())

while True:
    msg = input("Enter message (type exit to quit): ")

    if msg == "exit":
        break

    client_socket.send(msg.encode())

    reply = client_socket.recv(1024).decode()
    print("Echo from server:", reply)

client_socket.close()
```

---

## 🧠 Concept Groups (Important Understanding)

### 1️⃣ Why Send Name First?

Because TCP has no built-in identity.

We manually define a simple protocol:

```
First packet = name
Rest packets = messages
```

---

### 2️⃣ Why `if not data:` ?

When client disconnects:

```
recv() returns empty bytes
```

So we break the loop.

---

### 3️⃣ Why Echo Works in Loop?

Because TCP keeps the connection open until:

```
client closes
or
server closes
```

---

## 🔎 Sample Execution

### Server Terminal

```
Echo Server waiting...
Connected: Ravi 192.168.1.10 54231
Message from Ravi 192.168.1.10 54231 : Hello
Message from Ravi 192.168.1.10 54231 : How are you
```

### Client Terminal

```
Enter your name: Ravi
Enter message: Hello
Echo from server: Hello
Enter message: How are you
Echo from server: How are you
```

---

## ⚠️ Important Limitation

- Handles only **one client**
    
- Second client must wait
    

---

# 🧩 PART B2 — Multi-Client Chat Server (TCP + Multithreading)

Now we implement a **multi-user chat server**.

Goal:

- Multiple clients connect simultaneously
    
- Messages from one client are sent to all other clients
    
- While displaying, print:
    
    - Client Name
        
    - Client IP
        
    - Client Port
        

Keep it **minimal, reproducible, exam-safe**.

---

## 🔷 Concept 1 — Why Multithreading?

Problem:

Blocking `recv()` allows only one client at a time.

Solution:

```
Main thread → accepts connections
New thread → handles each client
```

So:

```
Client1 → Thread1
Client2 → Thread2
Client3 → Thread3
```

Each thread waits independently.

---

## 🔷 Concept 2 — Shared Client List

We maintain:

```
clients = []
```

When a client connects:

- Add it to list
    
- Broadcast messages to everyone except sender
    

---

## 🖥️ TCP MULTI-CLIENT CHAT SERVER

```python
import socket
import threading

clients = []   # List of (connection, name, address)

def broadcast(message, sender_conn):
    for conn, name, addr in clients:
        if conn != sender_conn:
            conn.send(message.encode())

def handle_client(conn, addr):
    client_ip = addr[0]
    client_port = addr[1]

    # First receive name
    name = conn.recv(1024).decode()
    clients.append((conn, name, addr))

    print("Connected:", name, client_ip, client_port)

    while True:
        try:
            data = conn.recv(1024).decode()
            if not data:
                break

            print("Message from", name, client_ip, client_port, ":", data)

            full_message = name + ": " + data
            broadcast(full_message, conn)

        except:
            break

    print(name, "disconnected")
    clients.remove((conn, name, addr))
    conn.close()

# ---- Main Server ----

server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.bind(("0.0.0.0", 5002))
server_socket.listen(5)

print("Chat Server running...")

while True:
    conn, addr = server_socket.accept()
    thread = threading.Thread(target=handle_client, args=(conn, addr))
    thread.start()
```

---

## 🖥️ TCP CHAT CLIENT

```python
import socket
import threading

def receive_messages(sock):
    while True:
        try:
            message = sock.recv(1024).decode()
            print(message)
        except:
            break

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect(("SERVER_IP_HERE", 5002))

name = input("Enter your name: ")
client_socket.send(name.encode())

thread = threading.Thread(target=receive_messages, args=(client_socket,))
thread.start()

while True:
    msg = input()
    if msg == "exit":
        break
    client_socket.send(msg.encode())

client_socket.close()
```

---

## 🧠 Concept Groups

### 1️⃣ Thread per Client

Each client runs:

```
handle_client()
```

independently.

Blocking `recv()` is no longer a limitation.

---

### 2️⃣ Broadcasting Logic

We send message to:

```
All clients except sender
```

Controlled by:

```python
if conn != sender_conn:
```

---

### 3️⃣ Why Try/Except?

If client disconnects suddenly:

- `recv()` raises error
    
- We catch and exit cleanly
    

---

## 🔎 Sample Execution

### Server Output

```
Chat Server running...
Connected: Ravi 192.168.1.5 54321
Connected: Arun 192.168.1.6 54330
Message from Ravi 192.168.1.5 54321 : Hello
```

### Client (Arun)

```
Ravi: Hello
```

---

## ⚠️ Exam Notes

You must remember:

- `threading.Thread(target=..., args=...)`
    
- Maintain client list
    
- Broadcast inside loop
    
- Remove client on disconnect
    

Everything else is conceptual.

---
# 🧩 PART B3 — File Transfer Application (TCP)

Now we implement a proper **File Transfer Application using TCP**.

Goal:

- Client sends a file to server  
- Server receives and stores it  
- While displaying, print:
  - Client Name  
  - Client IP  
  - Client Port  

Keep it **minimal and exam-friendly**.

---

## 🔷 Concept 1 — What is a File Transfer Application?

A file transfer system:

```
Client → sends file metadata
Client → sends file content (bytes)
Server → receives and writes to file
```

Since files may be:

- PDF  
- Image  
- Audio  
- Video  

We must handle **binary data**.

---

## 🔷 Concept 2 — Transfer Flow Design

We define a simple protocol:

```
Step 1 → Client sends name
Step 2 → Client sends file name
Step 3 → Client sends file size
Step 4 → Client sends file data (in chunks)
```

Server:

```
Receive name
Receive file name
Receive file size
Receive file data until size matched
Save file
```

---

## 🖥️ TCP FILE TRANSFER SERVER (Minimal + Clear)

```python
import socket

server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.bind(("0.0.0.0", 5003))
server_socket.listen(1)

print("File Transfer Server running...")

conn, addr = server_socket.accept()

client_ip = addr[0]
client_port = addr[1]

# 1️⃣ Receive client name
client_name = conn.recv(1024).decode()
print("Connected:", client_name, client_ip, client_port)

# 2️⃣ Receive file name
file_name = conn.recv(1024).decode()

# 3️⃣ Receive file size
file_size = int(conn.recv(1024).decode())

print("Receiving file:", file_name)
print("File size:", file_size, "bytes")

# 4️⃣ Receive file data
received_bytes = 0

with open("received_" + file_name, "wb") as f:
    while received_bytes < file_size:
        data = conn.recv(1024)
        f.write(data)
        received_bytes += len(data)

print("File received successfully.")

conn.close()
server_socket.close()
```

---

## 🖥️ TCP FILE TRANSFER CLIENT

```python
import socket
import os

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect(("SERVER_IP_HERE", 5003))

# 1️⃣ Send name
name = input("Enter your name: ")
client_socket.send(name.encode())

# 2️⃣ Send file name
file_path = input("Enter file path: ")
file_name = os.path.basename(file_path)
client_socket.send(file_name.encode())

# 3️⃣ Send file size
file_size = os.path.getsize(file_path)
client_socket.send(str(file_size).encode())

# 4️⃣ Send file data
with open(file_path, "rb") as f:
    while True:
        data = f.read(1024)
        if not data:
            break
        client_socket.send(data)

print("File sent successfully.")

client_socket.close()
```

---

## 🧠 Concept Groups (Important Understanding)

### 1️⃣ Why Send File Size?

TCP is a continuous stream.

Without file size, server does not know:

```
Where file ends
```

So we stop receiving when:

```
received_bytes >= file_size
```

---

### 2️⃣ Why Use Binary Mode?

Files are not plain text.

We must use:

```
"rb" → read binary
"wb" → write binary
```

Otherwise file may get corrupted.

---

### 3️⃣ Why Receive in Loop?

`recv(1024)` does NOT guarantee:

- Full file
- Fixed-size chunks

So we accumulate:

```
received_bytes += len(data)
```

Until total size matches.

---

## 🔎 Sample Execution

### Server Terminal

```
File Transfer Server running...
Connected: Ravi 192.168.1.5 54321
Receiving file: sample.pdf
File size: 20480 bytes
File received successfully.
```

---

### Client Terminal

```
Enter your name: Ravi
Enter file path: sample.pdf
File sent successfully.
```

---

## ⚠️ Important Limitation

- Handles only **one client**
- No progress tracking
- No error recovery
- No resume support

---

# 🧩 PART B4 — Calculator Server (TCP)

Now we implement a **Calculator Server using TCP**.

Goal:

- Client sends a mathematical expression  
- Server evaluates it  
- Server sends result back  
- While displaying, print:
  - Client Name  
  - Client IP  
  - Client Port  

Keep it **minimal and exam-friendly**.

---

## 🔷 Concept 1 — What is a Calculator Server?

A calculator server:

```
Client → sends expression
Server → evaluates expression
Server → sends result back
```

It processes input instead of echoing it.

---

## 🔷 Concept 2 — Handling One Client Cleanly

Flow:

```
Create socket
Bind
Listen
Accept
Receive name
Receive expression in loop
Evaluate
Send result
Close when client exits
```

We ask the client to send their **name first**, so the server can display it.

---

## 🖥️ TCP CALCULATOR SERVER (Minimal + Clear)

```python
import socket

server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.bind(("0.0.0.0", 5004))
server_socket.listen(1)

print("Calculator Server running...")

conn, addr = server_socket.accept()

client_ip = addr[0]
client_port = addr[1]

# First message = client name
client_name = conn.recv(1024).decode()
print("Connected:", client_name, client_ip, client_port)

while True:
    data = conn.recv(1024).decode()

    if not data:
        break

    print("Expression from", client_name, client_ip, client_port, ":", data)

    try:
        result = str(eval(data))
    except:
        result = "Invalid Expression"

    conn.send(result.encode())

conn.close()
server_socket.close()
```

---

## 🖥️ TCP CALCULATOR CLIENT

```python
import socket

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect(("SERVER_IP_HERE", 5004))

name = input("Enter your name: ")
client_socket.send(name.encode())

while True:
    expression = input("Enter expression (type exit to quit): ")

    if expression == "exit":
        break

    client_socket.send(expression.encode())

    reply = client_socket.recv(1024).decode()
    print("Result from server:", reply)

client_socket.close()
```

---

## 🧠 Concept Groups (Important Understanding)

### 1️⃣ Why Send Name First?

Because TCP has no built-in identity.

We define a simple protocol:

```
First packet = name
Rest packets = expressions
```

---

### 2️⃣ Why `eval()`?

```
eval(expression)
```

Converts string mathematical expression into result.

⚠ In real systems, `eval()` is unsafe.  
For lab/exam purposes, it is acceptable.

---

### 3️⃣ Why `try/except`?

If client sends:

```
10 +
```

`eval()` raises an error.

We catch it and return:

```
Invalid Expression
```

---

### 4️⃣ Why Loop?

Because TCP connection remains open:

```
while True:
    receive → evaluate → send
```

Multiple calculations allowed per connection.

---

## 🔎 Sample Execution

### Server Terminal

```
Calculator Server running...
Connected: Ravi 192.168.1.5 54321
Expression from Ravi 192.168.1.5 54321 : 10+20
Expression from Ravi 192.168.1.5 54321 : 5*6
```

### Client Terminal

```
Enter your name: Ravi
Enter expression: 10+20
Result from server: 30
Enter expression: 5*6
Result from server: 30
```

---

## ⚠️ Important Limitation

- Handles only **one client**
- Second client must wait
- `eval()` should not be used in production systems

---

Next:

👉 UDP implementations  
or  
👉 Error Detection (LRC, VRC, CRC, Checksum)  
or  
👉 Hamming Code problem  
or  
👉 Binary conversion TCP problem  

# 🧩 PART C — UDP Socket Programming Applications

We now implement applications using **UDP Socket Programming**.

We will implement:

1. Centralized Hub  
2. Chatroom  
3. File Transfer Application  

Language used: **Python**  
Goal: **Minimal, exam-friendly, logically structured code**

---

# 🟢 C1 — Centralized Hub (UDP)

## 🔷 Concept 1: What a Centralized Hub Does

A centralized hub:

- Receives messages from clients  
- Forwards messages to other clients  
- Does not create connections  

UDP is:

- Connectionless  
- No reliability guarantee  
- No ordering guarantee  

Flow:

```
Server:
    Create socket
    Bind
    Receive message
    Store client address
    Forward message

Client:
    Create socket
    Send name
    Send messages
    Receive forwarded messages
```

---

## 🖥️ UDP HUB SERVER (Minimal)

```python
import socket

server_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
server_socket.bind(("0.0.0.0", 6001))

print("UDP Hub Server running...")

clients = {}   # address → name

while True:
    data, addr = server_socket.recvfrom(1024)
    message = data.decode()

    # First message from client = name
    if addr not in clients:
        clients[addr] = message
        print("Registered:", message, addr[0], addr[1])
        continue

    sender_name = clients[addr]

    print("Message from", sender_name, addr[0], addr[1], ":", message)

    # Forward to other clients
    for client_addr in clients:
        if client_addr != addr:
            forward_msg = sender_name + ": " + message
            server_socket.sendto(forward_msg.encode(), client_addr)
```

---

## 🖥️ UDP HUB CLIENT (Minimal)

```python
import socket
import threading

def receive_messages(sock):
    while True:
        data, addr = sock.recvfrom(1024)
        print(data.decode())

client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
server_address = ("SERVER_IP_HERE", 6001)

name = input("Enter your name: ")
client_socket.sendto(name.encode(), server_address)

thread = threading.Thread(target=receive_messages, args=(client_socket,))
thread.start()

while True:
    msg = input()
    if msg == "exit":
        break
    client_socket.sendto(msg.encode(), server_address)

client_socket.close()
```

---

## 🧠 UDP Concepts (Grouped Explanation)

### 1️⃣ No Connection Phase

UDP does NOT use:

- `listen()`  
- `accept()`  
- `connect()`  

Server only uses:

```
recvfrom()
sendto()
```

---

### 2️⃣ Client Identification

`recvfrom()` returns:

```
(data, sender_address)
```

We store:

```
clients[address] = name
```

So server remembers who is who.

---

### 3️⃣ Manual Broadcasting

UDP does not broadcast automatically.

Server must manually forward:

```
for each client except sender:
    sendto()
```

---

## 🔎 Sample Execution

### Server Terminal

```
UDP Hub Server running...
Registered: Ravi 192.168.1.5 54021
Registered: Arun 192.168.1.6 54030
Message from Ravi 192.168.1.5 54021 : Hello
```

---

### Client (Arun)

```
Ravi: Hello
```

---

## ⚠️ Important Notes

- No guaranteed delivery  
- No guaranteed ordering  
- Server forgets clients if restarted  
- Suitable for lightweight communication  

---

# 🟢 C2 — UDP Chatroom

Now we implement a proper **UDP Chatroom**.

Goal:

- Multiple clients communicate through a UDP server  
- Server forwards messages to all other clients  
- While displaying, print:
  - Client Name  
  - Client IP  
  - Client Port  

Keep it **minimal and exam-friendly**.

---

## 🔷 Concept 1 — What is a UDP Chatroom?

A UDP chatroom:

```
Client → sends message to server
Server → forwards message to all other clients
Clients → receive messages
```

Unlike TCP:

- No connection setup
- No session state
- Server identifies clients using address

---

## 🔷 Concept 2 — How It Differs from C1 (Centralized Hub)

Centralized Hub:
- First message registers client
- Then forwards

UDP Chatroom:
- Same logic
- But supports continuous chatting
- Uses thread on client side for receiving

Flow:

```
Server:
    recvfrom()
    identify sender
    forward to others

Client:
    sendto()
    recvfrom() in separate thread
```

---

## 🖥️ UDP CHATROOM SERVER (Minimal + Clear)

```python
import socket

server_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
server_socket.bind(("0.0.0.0", 6002))

print("UDP Chatroom Server running...")

clients = {}   # address → name

while True:
    data, addr = server_socket.recvfrom(1024)
    message = data.decode()

    # First message = name registration
    if addr not in clients:
        clients[addr] = message
        print("Registered:", message, addr[0], addr[1])
        continue

    sender_name = clients[addr]

    print("Message from", sender_name, addr[0], addr[1], ":", message)

    # Broadcast to others
    for client_addr in clients:
        if client_addr != addr:
            full_message = sender_name + ": " + message
            server_socket.sendto(full_message.encode(), client_addr)
```

---

## 🖥️ UDP CHATROOM CLIENT

```python
import socket
import threading

def receive_messages(sock):
    while True:
        data, addr = sock.recvfrom(1024)
        print(data.decode())

client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
server_address = ("SERVER_IP_HERE", 6002)

name = input("Enter your name: ")
client_socket.sendto(name.encode(), server_address)

thread = threading.Thread(target=receive_messages, args=(client_socket,))
thread.start()

while True:
    msg = input()
    if msg == "exit":
        break
    client_socket.sendto(msg.encode(), server_address)

client_socket.close()
```

---

## 🧠 Concept Groups (Important Understanding)

### 1️⃣ Why Use Thread in Client?

Because:

```
input() blocks
recvfrom() blocks
```

Without thread:
- Client cannot send and receive simultaneously

---

### 2️⃣ How Server Identifies Clients?

UDP provides:

```
(data, sender_address)
```

We store:

```
clients[address] = name
```

---

### 3️⃣ Why No Connection Management?

UDP is stateless.

Server does not:

- Accept connections
- Maintain sessions
- Detect disconnections

---

## 🔎 Sample Execution

### Server Terminal

```
UDP Chatroom Server running...
Registered: Ravi 192.168.1.5 55001
Registered: Arun 192.168.1.6 55010
Message from Ravi 192.168.1.5 55001 : Hello
```

---

### Client (Arun)

```
Ravi: Hello
```

---

## ⚠️ Important Limitation

- No delivery guarantee  
- No ordering guarantee  
- No disconnection detection  
- If server restarts → client list lost  

---

# 🟢 C3 — UDP File Transfer Application

Now we implement a proper **UDP File Transfer Application**.

Goal:

- Client sends a file to server  
- Server receives and stores it  
- While displaying, print:
  - Client Name  
  - Client IP  
  - Client Port  

Keep it **minimal and exam-friendly**.

---

## 🔷 Concept 1 — What is a UDP File Transfer?

In UDP:

- There is no connection
- No reliability
- No ordering guarantee

So file transfer works like:

```
Client:
    Send name
    Send file metadata
    Send file content (chunks)

Server:
    Receive metadata
    Receive file chunks
    Write to file
```

We assume:

- Small file
- No packet loss
- Clean LAN environment

---

## 🔷 Concept 2 — Transfer Flow Design

Protocol:

```
Step 1 → Client sends name
Step 2 → Client sends file name
Step 3 → Client sends file size
Step 4 → Client sends file data in chunks
Step 5 → Server receives until size matches
```

---

## 🖥️ UDP FILE TRANSFER SERVER (Minimal + Clear)

```python
import socket

server_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
server_socket.bind(("0.0.0.0", 6003))

print("UDP File Transfer Server running...")

# 1️⃣ Receive client name
data, addr = server_socket.recvfrom(1024)
client_name = data.decode()
print("Connected:", client_name, addr[0], addr[1])

# 2️⃣ Receive file name
data, addr = server_socket.recvfrom(1024)
file_name = data.decode()

# 3️⃣ Receive file size
data, addr = server_socket.recvfrom(1024)
file_size = int(data.decode())

print("Receiving file:", file_name)
print("File size:", file_size, "bytes")

# 4️⃣ Receive file data
received_bytes = 0

with open("received_" + file_name, "wb") as f:
    while received_bytes < file_size:
        data, addr = server_socket.recvfrom(1024)
        f.write(data)
        received_bytes += len(data)

print("File received successfully.")
server_socket.close()
```

---

## 🖥️ UDP FILE TRANSFER CLIENT (Minimal)

```python
import socket
import os

client_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
server_address = ("SERVER_IP_HERE", 6003)

# 1️⃣ Send name
name = input("Enter your name: ")
client_socket.sendto(name.encode(), server_address)

# 2️⃣ Send file name
file_path = input("Enter file path: ")
file_name = os.path.basename(file_path)
client_socket.sendto(file_name.encode(), server_address)

# 3️⃣ Send file size
file_size = os.path.getsize(file_path)
client_socket.sendto(str(file_size).encode(), server_address)

# 4️⃣ Send file data
with open(file_path, "rb") as f:
    while True:
        data = f.read(1024)
        if not data:
            break
        client_socket.sendto(data, server_address)

print("File sent successfully.")
client_socket.close()
```

---

## 🧠 Concept Groups (Important Understanding)

### 1️⃣ Why No `listen()` or `accept()`?

UDP is connectionless.

Server simply:

```
recvfrom()
sendto()
```

No session setup.

---

### 2️⃣ Why Send File Size?

UDP does not indicate end of transmission.

So we stop when:

```
received_bytes >= file_size
```

---

### 3️⃣ Why Use Binary Mode?

Files may be:

- Image
- PDF
- Audio
- Video

So we use:

```
"rb" → read binary
"wb" → write binary
```

---

### 4️⃣ Limitations of UDP File Transfer

- No reliability  
- No retransmission  
- No ordering guarantee  
- Suitable only for small files in LAN  

---

## 🔎 Sample Execution

### Server Terminal

```
UDP File Transfer Server running...
Connected: Ravi 192.168.1.5 55100
Receiving file: image.jpg
File size: 15000 bytes
File received successfully.
```

---

### Client Terminal

```
Enter your name: Ravi
Enter file path: image.jpg
File sent successfully.
```

---

# 🧩 PART D — Complete Error Detection Suite (Sender + Receiver Logic)

We now implement **all major error detection techniques** properly.

We will implement:

1. VRC (Vertical Redundancy Check)  
2. LRC (Longitudinal Redundancy Check)  
3. Checksum (1’s Complement)  
4. CRC (Cyclic Redundancy Check)  

All implementations will include:

- Sender logic  
- Receiver verification logic  
- Clean reusable functions  

Assume your bit utility functions are available.

---

# 🔵 D1 — VRC (Vertical Redundancy Check)

## 🔷 Concept

Add **one parity bit** to each data word.

Even parity rule:

```
Total number of 1s must be even
```

---

## 🖥️ VRC Implementation

### Sender

```python
def vrc_generate(data):
    ones = data.count('1')
    parity = '0' if ones % 2 == 0 else '1'
    return data + parity
```

---

### Receiver

```python
def vrc_check(received):
    return received.count('1') % 2 == 0
```

---

# 🟢 D2 — LRC (Longitudinal Redundancy Check)

## 🔷 Concept

Parity calculated **column-wise** over multiple words.

---

## 🖥️ LRC Implementation

### Sender

```python
def lrc_generate(data_words):
    length = len(data_words[0])
    lrc = ""

    for i in range(length):
        column_sum = 0
        for word in data_words:
            column_sum += int(word[i])
        lrc += str(column_sum % 2)

    return lrc
```

---

### Receiver

```python
def lrc_check(data_words, received_lrc):
    computed_lrc = lrc_generate(data_words)
    return computed_lrc == received_lrc
```

---

# 🟡 D3 — Checksum (1’s Complement Method)

## 🔷 Concept

Steps:

```
1. Divide data into fixed-size blocks
2. Add using binary addition
3. Take 1’s complement
4. Append checksum
```

Receiver:

```
Add all blocks including checksum
If result = all 1s → No error
```

---

## 🖥️ Checksum Implementation

### Helper: Binary Addition with Carry Wrap

```python
def binary_add_wrap(a, b):
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
        result = binary_add_wrap(result, "1")

    return result[-max_len:]
```

---

### Sender

```python
def checksum_generate(blocks):
    total = blocks[0]
    for block in blocks[1:]:
        total = binary_add_wrap(total, block)

    checksum = ones_complement(total)
    return checksum
```

---

### Receiver

```python
def checksum_check(blocks, received_checksum):
    total = blocks[0]
    for block in blocks[1:]:
        total = binary_add_wrap(total, block)

    total = binary_add_wrap(total, received_checksum)

    return total.count('0') == 0
```

---

# 🔴 D4 — CRC (Cyclic Redundancy Check)

## 🔷 Concept

CRC uses:

- Generator polynomial
- Mod-2 division

Sender:

```
Append zeros (degree of generator)
Divide using XOR
Remainder = CRC
Append remainder
```

Receiver:

```
Divide full codeword
If remainder = 0 → No error
```

---

## 🖥️ CRC Implementation

### XOR Helper

```python
def xor(a, b):
    result = ""
    for i in range(len(b)):
        if a[i] == b[i]:
            result += '0'
        else:
            result += '1'
    return result
```

---

### Mod-2 Division

```python
def mod2_division(dividend, divisor):
    pick = len(divisor)
    temp = dividend[:pick]

    while pick < len(dividend):
        if temp[0] == '1':
            temp = xor(divisor, temp) + dividend[pick]
        else:
            temp = xor('0'*pick, temp) + dividend[pick]
        pick += 1

    if temp[0] == '1':
        temp = xor(divisor, temp)
    else:
        temp = xor('0'*pick, temp)

    return temp
```

---

### Sender

```python
def crc_generate(data, key):
    appended_data = data + '0'*(len(key)-1)
    remainder = mod2_division(appended_data, key)
    return data + remainder
```

---

### Receiver

```python
def crc_check(received, key):
    remainder = mod2_division(received, key)
    return remainder.count('1') == 0
```

---

## ⚖️ Final Comparison

| Method   | Detects Single | Detects Burst | Corrects | Complexity |
| -------- | -------------- | ------------- | -------- | ---------- |
| VRC      | Yes            | No            | No       | Very Low   |
| LRC      | Some           | Limited       | No       | Low        |
| Checksum | Moderate       | Moderate      | No       | Medium     |
| CRC      | Strong         | Strong        | No       | High       |

---

## 🧠 What You Now Have

- Full sender logic
- Full receiver verification
- Reusable functions
- Pure bit-level operations
- Ready for socket integration

---
# 🧩 PART E — Error Correction (Hamming Code)

Now we implement **Single-Bit Error Correction using Hamming Code**.

Assumption:

- Your bit utility functions (binary_add, xor, etc.) are available.
- We focus only on Hamming logic.

Goal:

- Generate Hamming code (even parity)
- Detect error using syndrome
- Correct single-bit error
- Extract original data

---

## Concept 1: How Many Parity Bits?

Hamming condition:

```
2^r ≥ m + r + 1
```

Where:

- m = number of data bits
- r = number of parity bits

Example:

If m = 10

Try:

```
r = 4
2^4 = 16
10 + 4 + 1 = 15
16 ≥ 15 → valid
```

So we need **4 parity bits**.

---

## Concept 2: Parity Bit Positions

Parity bits are placed at positions:

```
1, 2, 4, 8, 16, ...
```

These are powers of 2.

All other positions are data bits.

---

# 🔵 E1 — Construct Hamming Code (Even Parity)

### 🖥️ Implementation

```python
def calculate_parity_bits(m):
    r = 0
    while (2 ** r) < (m + r + 1):
        r += 1
    return r
```

---

```python
def generate_hamming_code(data):
    m = len(data)
    r = calculate_parity_bits(m)
    n = m + r

    hamming = ['0'] * n

    # Place data bits
    j = 0
    for i in range(1, n + 1):
        if i & (i - 1) != 0:  # not power of 2
            hamming[i - 1] = data[j]
            j += 1

    # Calculate parity bits
    for i in range(r):
        position = 2 ** i
        parity = 0

        for j in range(1, n + 1):
            if j & position:
                parity ^= int(hamming[j - 1])

        hamming[position - 1] = str(parity)

    return ''.join(hamming)
```

---

# 🔵 E2 — Error Detection (Syndrome Calculation)

Concept:

- Recalculate parity
- If parity fails → add position
- Syndrome gives error location

---

### 🖥️ Implementation

```python
def detect_error(codeword):
    n = len(codeword)
    r = 0
    while (2 ** r) < n:
        r += 1

    error_position = 0

    for i in range(r):
        position = 2 ** i
        parity = 0

        for j in range(1, n + 1):
            if j & position:
                parity ^= int(codeword[j - 1])

        if parity != 0:
            error_position += position

    return error_position
```

---

# 🔵 E3 — Correct the Error

```python
def correct_error(codeword):
    error_pos = detect_error(codeword)

    if error_pos == 0:
        return codeword, 0

    corrected = list(codeword)

    if corrected[error_pos - 1] == '0':
        corrected[error_pos - 1] = '1'
    else:
        corrected[error_pos - 1] = '0'

    return ''.join(corrected), error_pos
```

---

# 🔵 E4 — Extract Original Data

```python
def extract_data(codeword):
    data = ""
    n = len(codeword)

    for i in range(1, n + 1):
        if i & (i - 1) != 0:
            data += codeword[i - 1]

    return data
```

---

## 🧪 Full Example

```python
data = "1011010110"

code = generate_hamming_code(data)
print("Generated Code:", code)

# Introduce error at position 9
corrupted = list(code)
corrupted[8] = '1' if corrupted[8] == '0' else '0'
corrupted = ''.join(corrupted)

print("Corrupted Code:", corrupted)

corrected, pos = correct_error(corrupted)

print("Error at position:", pos)
print("Corrected Code:", corrected)
print("Extracted Data:", extract_data(corrected))
```

---

## 🧠 What This Implementation Supports

| Feature                    | Supported |
| -------------------------- | --------- |
| Single-bit error detection | ✅         |
| Single-bit correction      | ✅         |
| Multi-bit detection        | ❌         |
| SECDED                     | ❌         |

---

## ⚠️ Important Concepts

- Parity positions = powers of 2
- Syndrome = binary index of error
- Only corrects single-bit error
- Used in memory systems

---

