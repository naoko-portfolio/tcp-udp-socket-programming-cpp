# TCP and UDP Socket Programming in C++

## Overview
This project was completed as part of my Data Communication Programming course. Using instructor-provided starter code, I implemented UDP and TCP client-server communication in C++, converted the UDP application to TCP, and analyzed network traffic using Wireshark.

## Goal
Implement UDP and TCP client-server communication in C++.
Understand the differences between UDP and TCP.
Analyze network traffic using Wireshark.

## Why I Started This Project
This project was originally completed as part of my Data Communication Programming course. Although I found the topic interesting, I didn't have enough time during the course to fully understand how everything worked or explore it in depth. I decided to revisit the project, deepen my understanding, and document what I learned on GitHub so I can review it in the future. I had also learned how to use Wireshark in a previous project, so this was a great opportunity to compare UDP and TCP traffic and better understand how they handle data communication differently.


## Environment

- Operating System: Windows 11
- Programming Language: C++
- Compiler: g++ (MinGW)
- IDE: Visual Studio Code
- Networking Library: Winsock2
- Packet Analyzer: Wireshark 4.6
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
### Key Code Steps

1. Set up the server address
<img width="265" height="90" alt="1" src="https://github.com/user-attachments/assets/b79b4a8a-894f-4716-9f8f-9da44539913b" />

2. Send the user message using `sendto()`

<img width="351" height="251" alt="2" src="https://github.com/user-attachments/assets/9e0dfc2c-fea2-4a47-988b-8e0ca2ee3df4" />

3. Receive the server response using `recvfrom()`

<img width="160" height="139" alt="3" src="https://github.com/user-attachments/assets/98579348-cab7-4e67-9c96-73f623b88349" />

4. Display the server response

<img width="330" height="227" alt="4" src="https://github.com/user-attachments/assets/839d44b8-11c7-45d0-bf26-b4e1dfc1e4da" />


### Client Output

- Sent multiple messages (`aa`, `az`, `ha`, `hz`, `ma`, `mz`)
- Received the closest matching word from the server.

<img width="391" height="154" alt="5udp" src="https://github.com/user-attachments/assets/e48ffc98-6f83-4cbe-9e9a-48d77fbb744b" />


### Server Output
- Received each request from the client.
- Displayed the client's IP address, port number, and received message.

<img width="392" height="106" alt="6udp" src="https://github.com/user-attachments/assets/40e5f9c5-cea7-4f15-a1a9-383f0e5dfae3" />

The server successfully received each request and returned the closest matching word using UDP communication.

### UDP Request-Response Flow
The diagram below shows how the client and server exchange a request and response using UDP.
<img width="160" height="377" alt="7udp" src="https://github.com/user-attachments/assets/bb433b85-3ce2-40d0-a6df-608b4a9d0aca" />


## Question I had

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


ここまで正解

## Wireshark Analysis
### UDP
1. Open Wireshark
2. Chose network
3. Apply udp.port == 8080
4. Run UDP server
5. Run UDP client
6. Apply "aa, ah, ha, hz, ma, mz" on client
7. Apply "q" on client
8. Stop wireshark netwark
9. Start captering

## Anlyze UDP networking
Client

<img width="478" height="221" alt="wireshark2" src="https://github.com/user-attachments/assets/07e5ee61-746c-4f7c-8c27-93deda89c358" />

Server
<img width="481" height="149" alt="wireshark3" src="https://github.com/user-attachments/assets/7c6e16ef-531d-499a-b953-b45b4b65f8d4" />
Client port: 65243
Server port: 8080
RTT
Request: 656.341608500
Response: 656.343351800
656.343351800 - 656.341608500=1.743 ms
RTT= 1.743 ms

### Observations

- Each client request is immediately followed by a server response.
- The server listens on port 8080, while the client uses a temporary port.
- Request packets are 2 bytes because the client sends two-character messages.
- Response packet sizes vary depending on the matched word.
- All traffic uses 127.0.0.1 because the client and server run on the same computer.
- No retransmissions were observed because UDP does not guarantee reliable delivery.





## Questions I Had
What is RTT?
RTT(Round-Trip Time) 
Time from sending a request to receiving the response. Shorter represents faster networking.

What do Len=29, Len=31, and Len=26 mean?
Len represents the size of the UDP payload (data) in bytes. The request packets have Len=2 because the client sends two-character messages such as "aa" or "az". The response packets have different lengths (Len=29, Len=31, Len=26) because the server returns words of different lengths, resulting in different payload sizes.


Q: Why is TCP slower but more reliable than UDP? Does this relate to how they communicate?

A: Yes. The communication method is one of the main reasons.

UDP is connectionless. It sends data without establishing a connection or checking whether the data arrives successfully. This makes UDP fast, but packets may be lost or arrive out of order.
TCP is connection-oriented. It first establishes a connection using connect(), then exchanges data through the established connection. TCP also confirms that data has been received correctly before continuing, making it slower but much more reliable.
UDP prioritizes speed, while TCP prioritizes reliability.

### TCP
1. Open Wireshark
2. Chose network
3. Apply tcp.port == 8080
4. Run TCP server
5. Run TCP client
6. Apply "aa, ah, ha, hz, ma, mz" on client
7. Apply "q" on client
8. Stop wireshark netwark
9. Start captering


## Anlyze UDP networking
Client

<img width="483" height="187" alt="wireshark5" src="https://github.com/user-attachments/assets/31e5019c-77fc-455f-bddf-bb7a39b8114f" /> 

Server

<img width="562" height="230" alt="wireshark6" src="https://github.com/user-attachments/assets/706e8333-203a-42dd-9acd-38bfa237d8e5" />


Client port: 55865
Server port: 8080
RTT
Request: 656.341608500
Response: 656.343351800
656.343351800 - 656.341608500=1.743 ms
RTT= 1.743 ms




### Observations



## Question I Had

**Why do I only see one `SYN` and one `SYN, ACK` packet even though I sent six messages?**

At first, I thought TCP would check the connection every time I sent a message. However, I learned that TCP performs the three-way handshake only once at the beginning. After the connection is established, the client and server can continue exchanging messages without repeating the handshake.


## TCP vs UDP
put dwor and compare

TCP Connection

Unlike UDP, TCP is connection-oriented. The client establishes a connection with the server using connect() only once. After the connection is established, send() and recv() use the existing connection, so the client does not need to specify the server's IP address and port number for every message. The connection remains active until it is closed.

<img width="1169" height="1345" alt="ChatGPT Image Jul 4, 2026, 12_48_54 PM" src="https://github.com/user-attachments/assets/84859691-db7a-42f3-9b67-808dffed1e3b" />


connect(IP, Port)
send()
↓

recv()
↓

send()
↓

recv()
↓

close()

UDP
sendto(IP, Port)
↓

sendto(IP, Port)
↓

sendto(IP, Port)



## Reflection

What I learned


## Error occrned

## Skills Demonstrated

ls doent'work dir

the last
