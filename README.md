# TCP and UDP Socket Programming in C++

## Overview
Using instructor-provided starter code, I implemented UDP and TCP client-server communication in C++, converted the UDP application to TCP, and analyzed network traffic using Wireshark.

## Goal
- Implement UDP and TCP client-server communication in C++.
- Understand the differences between UDP and TCP.
- Analyze network traffic using Wireshark.


## Why I Started This Project
This project was originally completed as part of my Data Communication Programming course. Although I found the topic interesting, I didn't have enough time during the course to fully understand how everything worked or explore it in depth. I decided to revisit the project, deepen my understanding, and document what I learned on GitHub so I can review it in the future. I had also learned how to use Wireshark in a previous project, so this was a great opportunity to compare UDP and TCP traffic and better understand how each protocol handles communication.


## Environment

- Operating System: Windows 11
- Programming Language: C++
- Compiler: g++ (MinGW)
- IDE: Visual Studio Code
- Networking Library: Winsock2
- Packet Analyzer: Wireshark 4.6
- Diagram Tool: Figma
- Protocols: UDP, TCP

## Structure

client/

├── client.cpp

├── UDPClient.cpp

├── UDPClient.h

├── SocketSystem.*

└── SocketUtils.*

server/

├── server.cpp

├── UDPServer.cpp

└── UDPServer.h



## UDP Communication Flow

### UDP Request-Response Flow

<img width="160" height="377" alt="7udp" src="https://github.com/user-attachments/assets/bb433b85-3ce2-40d0-a6df-608b4a9d0aca" />


### Implementation Steps

1. Set up the server address

<img alt="1" src="https://github.com/user-attachments/assets/b79b4a8a-894f-4716-9f8f-9da44539913b" width="500" />

2. Send the user message using `sendto()`

<img alt="2" src="https://github.com/user-attachments/assets/9e0dfc2c-fea2-4a47-988b-8e0ca2ee3df4" width="500" />

3. Receive the server response using `recvfrom()`

<img alt="3" src="https://github.com/user-attachments/assets/98579348-cab7-4e67-9c96-73f623b88349" width="500" />

4. Display the server response

<img alt="4" src="https://github.com/user-attachments/assets/839d44b8-11c7-45d0-bf26-b4e1dfc1e4da" width="500" />



### Client Output

- Sent multiple messages (`aa`, `az`, `ha`, `hz`, `ma`, `mz`)
- Received the closest matching word from the server.

<img width="391" height="154" alt="5udp" src="https://github.com/user-attachments/assets/e48ffc98-6f83-4cbe-9e9a-48d77fbb744b" />


### Server Output
- Received each request from the client.
- Displayed the client's IP address, port number, and received message.

<img width="392" height="106" alt="6udp" src="https://github.com/user-attachments/assets/40e5f9c5-cea7-4f15-a1a9-383f0e5dfae3" />



## Questions I had

**What information allows the server to know where to send its reply?**

`recvfrom()` provides the client's IP address, port number, and message. The server uses the IP address and port number to send the response back to the correct client.

## TCP Communication
### Converting UDP to TCP

To create the TCP version, I copied the UDP client-server project into a new folder and renamed the UDP classes to TCP classes. I then modified the socket type and communication flow to use TCP instead of UDP.

### Modify the Client Code
**Changes:**
- socket(AF_INET, SOCK_DGRAM, 0); -> socket(AF_INET, SOCK_STREAM, 0);
<img width="197" height="77" alt="8tcp" src="https://github.com/user-attachments/assets/bac8ea34-2f08-4d47-9ea9-8f2db424e00f" />


- sendto → send
- recvfrom → recv
<img width="170" height="108" alt="10tcp" src="https://github.com/user-attachments/assets/e9dbbafb-8ccb-49c4-8009-dcab38e09740" />

**Add:**
connect(sock, (sockaddr*)&serverAddr, sizeof(serverAddr));

<img width="251" height="72" alt="9tcp" src="https://github.com/user-attachments/assets/215d8d91-de0e-443a-a897-84567c37b554" />

### Modify the Server Code
The server performs the following steps:

- Create a TCP socket (`SOCK_STREAM`)
- Bind the socket to port 8080
- Listen for incoming connections

<img width="284" height="209" alt="11tcp" src="https://github.com/user-attachments/assets/e8f13ea9-e26f-4095-ad6e-c98c6a49e2de" />

In the `run()` function:

- accept()
- recv()
- send()

<img width="289" height="345" alt="12tcp" src="https://github.com/user-attachments/assets/871d5872-75f8-4650-aee3-0f26474f74e2" />

### Establishing a TCP Connection

Unlike UDP, TCP requires a connection before data can be exchanged. The client establishes the connection using `connect()`, and the server accepts it using `accept()`. Once the connection is established, both sides communicate using `send()` and `recv()`.



## Wireshark Analysis

### UDP
1. Open Wireshark.
2. Select the loopback network adapter.
3. Apply the display filter: `udp.port == 8080`.
4. Start capturing packets.
5. Run the UDP server.
6. Run the UDP client.
7. Send the following messages: `aa`, `az`, `ha`, `hz`, `ma`, `mz`.
8. Enter `q` to terminate the client.
9. Stop packet capture.

## UDP Traffic Analysis
Client

<img width="478" height="221" alt="wireshark2" src="https://github.com/user-attachments/assets/07e5ee61-746c-4f7c-8c27-93deda89c358" />

Server
<img width="481" height="149" alt="wireshark3" src="https://github.com/user-attachments/assets/7c6e16ef-531d-499a-b953-b45b4b65f8d4" />
Client port: 65243
Server port: 8080

### RTT

Request Time: 656.341608500

Response Time: 656.343351800

RTT = 656.343351800 − 656.341608500

= 0.001743300 sec

= 1.743 ms

### Observations

- Each client request is immediately followed by a server response.
- The server listens on port 8080, while the client uses a temporary port.
- Request packets are 2 bytes because the client sends two-character messages.
- Response packet sizes vary because the matched words have different lengths.
- All traffic uses 127.0.0.1 because the client and server run on the same computer.
- No retransmissions were observed because UDP does not guarantee reliable delivery.


## Questions I Had

### What is RTT?

RTT (Round-Trip Time) is the time it takes for a request to travel from the client to the server and for the response to return to the client. A shorter RTT generally indicates faster network communication.


### What do `Len=29`, `Len=31`, and `Len=26` mean?

`Len` represents the size of the UDP payload (data) in bytes.

- Request packets have `Len=2` because the client sends two-character messages such as `"aa"` or `"az"`.
- Response packets have different lengths (`Len=29`, `Len=31`, `Len=26`) because the server returns words of different lengths.


### Why is TCP slower but more reliable than UDP?

UDP is **connectionless**. It sends data without establishing a connection or confirming that the data has been delivered. This makes UDP fast, but packets may be lost or arrive out of order.

TCP is **connection-oriented**. It first establishes a connection using `connect()`, then exchanges data through the established connection. TCP also acknowledges received data, making communication more reliable but slightly slower.

**In short:**
- **UDP prioritizes speed.**
- **TCP prioritizes reliability.**


## Summary

Using Wireshark, I observed the differences between UDP and TCP communication.

- UDP sends packets without establishing a connection.
- TCP establishes a connection using the three-way handshake before exchanging data.
- Both protocols showed very low RTT because the client and server were running on the same computer (`127.0.0.1`).

### TCP

1. Open Wireshark.
2. Select the loopback network adapter.
3. Apply the display filter: `tcp.port == 8080`.
4. Start capturing packets.
5. Run the TCP server.
6. Run the TCP client.
7. Send the following messages: `aa`, `az`, `ha`, `hz`, `ma`, `mz`.
8. Enter `q` to terminate the client.
9. Stop packet capture.

---

## TCP Traffic Analysis

### Client

<img width="483" height="187" alt="wireshark5" src="https://github.com/user-attachments/assets/31e5019c-77fc-455f-bddf-bb7a39b8114f" />

### Server

<img width="562" height="230" alt="wireshark6" src="https://github.com/user-attachments/assets/706e8333-203a-42dd-9acd-38bfa237d8e5" />

### RTT

- **Client Port:** 55865
- **Server Port:** 8080

Request Time: **160.733430500**

Response Time: **160.733626900**

RTT = 160.733626900 − 160.733430500

= 0.0001964 sec

= **0.196 ms**

### Observations

- TCP establishes a connection using the three-way handshake (`SYN` → `SYN, ACK` → `ACK`).
- After the connection is established, multiple messages are exchanged without repeating the handshake.
- Data transfer uses `PSH, ACK` packets, and each transmission is acknowledged with `ACK`.
- Both the client and server communicate through the established connection.
- The RTT was very small because the client and server were running on the same computer (`127.0.0.1`).


## Question I Had

**Why do I only see one `SYN` and one `SYN, ACK` packet even though I sent six messages?**

At first, I thought TCP would check the connection every time I sent a message. However, I learned that TCP performs the three-way handshake only once at the beginning. After the connection is established, the client and server can continue exchanging multiple messages without repeating the handshake.


## TCP vs UDP


### TCP Connection
<img width="281" height="373" alt="TCP connection" src="https://github.com/user-attachments/assets/7d65f434-63ca-4ab4-a8d2-a406cb441cbb" />

After connect(), the client uses send() and recv() without specifying IP address and port. The connection stays until it is closed.

### UDP Connection
<img width="282" height="329" alt="UDP connection" src="https://github.com/user-attachments/assets/0b315d35-5e0b-4534-885f-4e902a419121" />

Each time data is sent, the destination IP address and port number must be specified

### RTT Observation

I measured the round-trip time (RTT) by calculating the time difference between a client request and the corresponding server response in Wireshark.

**Observation**

- The RTT values were very small because both the client and server were running on the same computer (`127.0.0.1`).
- UDP and TCP both showed low RTT values in this local environment.
- Therefore, RTT was useful for observing the request-response timing, but it was not suitable for comparing the real-world performance of UDP and TCP.

The table below summarizes the key differences between UDP and TCP observed during this project.

| Protocol       | UDP                      | TCP                             |
| -------------- | ------------------------ | ------------------------------- |
| Connection     | Connectionless           | Connection-oriented             |
| Handshake      | None                     | Three-way handshake             |
| Reliability    | No guarantee             | Reliable delivery               |
| ACK            | No                       | Yes                             |
| Retransmission | No                       | Yes (if needed)                 |
| Functions      | `sendto()`, `recvfrom()` | `connect()`, `send()`, `recv()` |



## Question I Had

**If UDP has to specify the server's IP address and port number every time, why is it faster than TCP?**

At first, I thought TCP would be faster because it remembers the server's address after `connect()`, while UDP specifies the destination address every time with `sendto()`.

However, I learned that specifying the destination address takes very little time. The main reason TCP is slower is that it establishes a connection, acknowledges received data (ACK), retransmits lost packets when necessary, and guarantees reliable, ordered delivery. UDP skips these reliability features, making it much faster.
**Key takeaway:** UDP prioritizes speed, while TCP prioritizes reliability.


## Reflection

Before this project, I knew that UDP is faster than TCP and that TCP is more reliable, but it was only theoretical knowledge. By implementing both protocols, capturing packets with Wireshark, and analyzing the communication, I gained a much deeper understanding of how UDP and TCP actually work.

This hands-on experience made networking much more interesting to me. I especially enjoyed drawing communication diagrams and visualizing how data flows between the client and the server. It helped me connect networking concepts with real packet behavior and strengthened my understanding of socket programming.


## What I Learned

At the beginning of this project, I was confused about why `ls` did not work and why I had to use `dir`. I used to use a Mac and learned Linux at that time, so I was used to typing `ls`. Since I am now using Windows Command Prompt, I needed to use `dir` instead. After looking into the reason, I realized that `ls` is a Linux/macOS command, while `dir` is the equivalent command in Windows.

## Skills Demonstrated

## Skills Demonstrated

- C++
- Socket Programming (Winsock2)
- UDP & TCP Client-Server Development
- TCP Three-Way Handshake
- Packet Analysis with Wireshark
- Round-Trip Time (RTT) Analysis
- Network Troubleshooting
- Protocol Comparison (UDP vs TCP)
- Command-Line Development (Windows Command Prompt)

## Future Improvements

This project focused on the fundamentals of networking. As I continue learning, I want to take on more challenging networking projects, deepen my understanding, and gradually improve my technical skills through hands-on experience.

