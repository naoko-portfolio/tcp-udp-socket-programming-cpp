# TCP and UDP Socket Programming in C++

## Overview
This project was completed as part of my Data Communication Programming course. Using instructor-provided starter code, I implemented UDP and TCP client-server communication in C++, converted the UDP application to TCP, and analyzed network traffic using Wireshark.

## Goal
Implement UDP and TCP client-server communication in C++.
Understand the differences between UDP and TCP.
Analyze network traffic using Wireshark.

## Why I Started This Project
This project was originally completed as part of my Data Communication Programming course. Although I found the topic interesting, I didn't have enough time during the course to fully understand how everything worked or explore it in depth. I decided to revisit the project, deepen my understanding, and document what I learned on GitHub so I can review it in the future. I had also learned how to use Wireshark in a previous project, so this was a great opportunity to compare UDP and TCP traffic and better understand how they handle data communication differently.



Features

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

1. Set up the server address
<img width="265" height="90" alt="1" src="https://github.com/user-attachments/assets/b79b4a8a-894f-4716-9f8f-9da44539913b" />

2. Send the user message using `sendto()`

<img width="351" height="251" alt="2" src="https://github.com/user-attachments/assets/9e0dfc2c-fea2-4a47-988b-8e0ca2ee3df4" />

3. Receive the server response using `recvfrom()`

<img width="160" height="139" alt="3" src="https://github.com/user-attachments/assets/98579348-cab7-4e67-9c96-73f623b88349" />

4. Display the server response

<img width="330" height="227" alt="4" src="https://github.com/user-attachments/assets/839d44b8-11c7-45d0-bf26-b4e1dfc1e4da" />


## TCP Communication

## TCP vs UDP

Wireshark Analysis

## Questions I Had

## Reflection

What I learned


##Error occrned

ls doent'work dir

the last
