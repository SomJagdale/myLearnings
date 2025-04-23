 The Select/Poll/Epoll Showdown:

select:
Mechanism: Uses file descriptor sets (bitmasks) to monitor multiple file descriptors for readability, writability, or exceptions.
Limitations:
Limited number of file descriptors (typically up to FD_SETSIZE, often 1024).
Inefficient for a large number of inactive connections because it iterates through all monitored descriptors on each call.
Requires rebuilding the file descriptor sets on each call.
Advantages: Widely portable and available on most POSIX systems.
Use Cases: Suitable for a small to moderate number of connections or when portability is paramount.


poll:
Mechanism: Uses an array of pollfd structures, providing more information about the events to monitor.
Limitations: Still iterates through all monitored descriptors to check for events, so performance degrades with a large number of inactive connections.
Advantages: Overcomes the FD_SETSIZE limit of select. Provides more detailed event information.
Use Cases: Better than select for a larger number of connections or when the file descriptor limit is a concern.


epoll (Linux-Specific):
Mechanism: Event-driven interface that uses an event list to return only the file descriptors that have triggered an event. Uses epoll_create, epoll_ctl, and epoll_wait system calls.
Limitations: Linux-specific, not portable to all POSIX systems.
Advantages: Highly efficient for a large number of connections, even if most are inactive, because it only returns active descriptors. Uses a constant time complexity for event notification. Supports edge-triggered (ET) and level-triggered (LT) modes.
Use Cases: Ideal for high-performance network servers handling a massive number of concurrent connections.


 Write C code to switch a socket to non-blocking mode. What happens to recv() and accept() in this case?
 
🧠 Use fcntl() to set O_NONBLOCK. recv() or accept() will return -1 with errno == EAGAIN.

13. Describe how TCP’s congestion window (CWND) works. Can you simulate it in code or data structure?
14. 
🧠 A sliding window. Can simulate with circular buffer or two pointers over an array.



### ⚙️ **TCP (Transmission Control Protocol)**  
Think of TCP like a **phone call** — you get a reliable connection before talking.


#### ✅ Key Features:
| Feature        | Description |
|----------------|-------------|
| **Connection-oriented** | Establishes connection (3-way handshake) before sending data |
| **Reliable** | Guarantees delivery, with acknowledgments (ACKs) |
| **Ordered** | Data arrives in the same order it was sent |
| **Error-checked** | Includes checksums and retransmission on errors |
| **Slower** | Due to overhead of reliability features |
| **Use cases** | Web (HTTP/HTTPS), FTP, Emails (SMTP, IMAP) |

---

### ⚡ **UDP (User Datagram Protocol)**  
Think of UDP like a **postcard** — it’s sent and forgotten.

#### 🚫 Key Features:
| Feature        | Description |
|----------------|-------------|
| **Connectionless** | No connection setup — fire and forget |
| **Unreliable** | No guarantee of delivery, order, or error correction |
| **Fast** | Minimal overhead — best for speed-sensitive apps |
| **No retransmission** | If a packet is lost, it’s just gone |
| **Use cases** | VoIP, live video, DNS, online gaming, TFTP |

---

### 🔍 Comparison Table:

| Feature             | **TCP**                     | **UDP**                    |
|---------------------|-----------------------------|----------------------------|
| Connection setup    | Yes (Handshake)              | No                         |
| Reliability         | Reliable (ACK, retry)        | Unreliable                 |
| Order of packets    | Maintained                   | Not guaranteed             |
| Speed               | Slower                       | Faster                     |
| Error checking      | Yes                          | Yes (but no retransmit)    |
| Common Uses         | Web, FTP, Email              | VoIP, DNS, Streaming       |

---

### 🎯 When to use what?

- **Use TCP** when **data integrity and order** matter (web, downloads).
- **Use UDP** when **speed is critical**, and **occasional loss** is acceptable (calls, games, streams).

---

Let me know if you'd like a packet-level comparison or real code examples for TCP vs UDP in C or Python!


Sure! Here's a **side-by-side difference between TCP and UDP in C**, focusing on simple client examples to demonstrate how each works programmatically.

---

### ⚙️ TCP Client Example (C)

```c
// tcp_client.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>

int main() {
    int sock = socket(AF_INET, SOCK_STREAM, 0);  // TCP
    if (sock < 0) {
        perror("Socket creation failed");
        exit(EXIT_FAILURE);
    }

    struct sockaddr_in server_addr = {
        .sin_family = AF_INET,
        .sin_port = htons(8080),  // Server port
        .sin_addr.s_addr = inet_addr("127.0.0.1")  // Server IP
    };

    if (connect(sock, (struct sockaddr *)&server_addr, sizeof(server_addr)) < 0) {
        perror("Connection failed");
        exit(EXIT_FAILURE);
    }

    char *msg = "Hello via TCP!";
    send(sock, msg, strlen(msg), 0);

    char buffer[1024] = {0};
    recv(sock, buffer, sizeof(buffer), 0);
    printf("Server replied: %s\n", buffer);

    close(sock);
    return 0;
}
```

---

### ⚡ UDP Client Example (C)

```c
// udp_client.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>

int main() {
    int sock = socket(AF_INET, SOCK_DGRAM, 0);  // UDP
    if (sock < 0) {
        perror("Socket creation failed");
        exit(EXIT_FAILURE);
    }

    struct sockaddr_in server_addr = {
        .sin_family = AF_INET,
        .sin_port = htons(8080),  // Server port
        .sin_addr.s_addr = inet_addr("127.0.0.1")  // Server IP
    };

    char *msg = "Hello via UDP!";
    sendto(sock, msg, strlen(msg), 0,
           (struct sockaddr *)&server_addr, sizeof(server_addr));

    char buffer[1024] = {0};
    socklen_t len = sizeof(server_addr);
    recvfrom(sock, buffer, sizeof(buffer), 0,
             (struct sockaddr *)&server_addr, &len);

    printf("Server replied: %s\n", buffer);

    close(sock);
    return 0;
}
```

---

### 🔍 Code Differences Summary

| Feature         | TCP Code                        | UDP Code                        |
|-----------------|----------------------------------|----------------------------------|
| Socket type     | `SOCK_STREAM`                   | `SOCK_DGRAM`                     |
| Connection      | `connect()` needed              | No connection, uses `sendto()`   |
| Send/Receive    | `send()`, `recv()`              | `sendto()`, `recvfrom()`         |
| Reliable?       | Yes (ACK/retry)                 | No (best-effort)                 |
| Use case        | Data-sensitive apps             | Real-time or lossy-tolerant apps |


Great question! Let’s break it down simply and clearly.

---

### 🔒 **Blocking Mode** (default behavior)

In **blocking mode**, network system calls like `accept()`, `recv()`, `send()`, `connect()` **wait (block)** until the operation completes.

#### Example Behavior:
- `recv()` blocks until data is available to read.
- `accept()` blocks until a new connection is made.
- `connect()` blocks until connection is established or fails.

🧠 **Think of it like this**:  
“Hey system, I’m ready to receive data. I’ll wait here doing nothing until you give it to me.”

#### ✅ Pros:
- Easier to write and understand.
- Great for simple, sequential applications.

#### ❌ Cons:
- Can block your whole program if you're waiting on a slow client or network.
- Not suitable for real-time or high-concurrency apps.

---

### ⚡ **Non-Blocking Mode**

In **non-blocking mode**, the system call returns **immediately**, even if it can’t complete the requested action right away.

#### Example Behavior:
- `recv()` returns `-1` and sets `errno` to `EAGAIN` or `EWOULDBLOCK` if no data is available.
- `accept()` returns `-1` if no incoming connection yet.

🧠 **Think of it like this**:  
“Hey system, try to give me data now, but don’t make me wait. If you don’t have it, I’ll come back later.”

#### ✅ Pros:
- Allows single-threaded event loops (like in `select()`, `poll()`, `epoll()`).
- Better for high-performance servers, chat apps, game servers.

#### ❌ Cons:
- Code is more complex.
- You need to handle retries and check for readiness.

---

### 🧪 Example in C: Switching a socket to non-blocking

```c
#include <fcntl.h>

int sockfd = socket(AF_INET, SOCK_STREAM, 0);

// Set non-blocking mode
int flags = fcntl(sockfd, F_GETFL, 0);
fcntl(sockfd, F_SETFL, flags | O_NONBLOCK);
```

---

### 🔄 Real-Life Analogy

| Mode            | Analogy                             |
|-----------------|--------------------------------------|
| Blocking        | Waiting in line until it's your turn |
| Non-blocking    | Checking the line, going elsewhere, and coming back |

---

Let me know if you want a live demo app using non-blocking `select()` or `epoll()` to visualize this better!
