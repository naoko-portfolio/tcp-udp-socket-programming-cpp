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
### code

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

The server successfully received each request and returned the closest matching word to the client using UDP communication.

### UDP Request-Response Flow

<img width="160" height="377" alt="7udp" src="https://github.com/user-attachments/assets/bb433b85-3ce2-40d0-a6df-608b4a9d0aca" />


## Question I had
**What information allows the server to know where to send its reply?**
The server knows where to send the response because recvfrom() receives the client's IP address and port number along with the message.

## TCP Communication
##### Converting UDP to TCP

To create the TCP version, I copied the UDP client-server project into a new project folder and renamed the UDP classes to TCP classes. This allowed me to keep the same project structure while modifying the socket type and communication flow for TCP.

### Modify the Client Code
**Replace:** 
- socket(AF_INET, SOCK_DGRAM, 0); -> socket(AF_INET, SOCK_STREAM, 0);
<img width="197" height="77" alt="8tcp" src="https://github.com/user-attachments/assets/bac8ea34-2f08-4d47-9ea9-8f2db424e00f" />


- sendto → send
- recvfrom → recv
<img width="170" height="108" alt="10tcp" src="https://github.com/user-attachments/assets/e9dbbafb-8ccb-49c4-8009-dcab38e09740" />

**Add:**
connect(sock, (sockaddr*:&serverAddr, sizeof(serverAddr));

<img width="251" height="72" alt="9tcp" src="https://github.com/user-attachments/assets/215d8d91-de0e-443a-a897-84567c37b554" />

### Modify the Server Code
- Create a TCP socket (SOCK_STREAM)
- bind()
- listen()

<img width="284" height="209" alt="11tcp" src="https://github.com/user-attachments/assets/e8f13ea9-e26f-4095-ad6e-c98c6a49e2de" />

In the run() function:
create
- accept() 
- recv()
- send()

<img width="289" height="345" alt="12tcp" src="https://github.com/user-attachments/assets/871d5872-75f8-4650-aee3-0f26474f74e2" />


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



## Wireshark Analysis
koko



## Questions I Had

Q: Why is TCP slower but more reliable than UDP? Does this relate to how they communicate?

A: Yes. The communication method is one of the main reasons.

UDP is connectionless. It sends data without establishing a connection or checking whether the data arrives successfully. This makes UDP fast, but packets may be lost or arrive out of order.
TCP is connection-oriented. It first establishes a connection using connect(), then exchanges data through the established connection. TCP also confirms that data has been received correctly before continuing, making it slower but much more reliable.
UDP prioritizes speed, while TCP prioritizes reliability.

## Reflection

What I learned


## Error occrned

## Skills Demonstrated

ls doent'work dir

the last
