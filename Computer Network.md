# Introduction

## TCP

- Tcp is a protocol to allow end systems to communicate effectively.

- It ensure data reaching desired destination and is not corrupted.



## UDP

- The User Datagram Protocol (UDP)
- It does not ensure that data reaches the destination and that remain incorrupt.



## HTTP

- HyperText Transfer Protocol (HTTP) 
- It defines the format of data exchanges between web clients. 



## Packets

Message of bits in zero and one is being broken down to a smaller unit.

- Why smaller sizes:
  - In case of corruption, resend smaller package
  - Spend less time in sending one packet so that not occupy the pipe for too long
  - Some intermediate devices have limited buffer space, smaller package is easier to fit in.

## IP Address

- IP addresses are 32 bit numbers (in IP version 4)
- It is separated by 8 bits at a time. Those octets are read out in decimals, then separated by dots.
- Each octet (8 bits in decimals) can be from 0 to 255. 



## Port

Any host connected to the internet can be running many network applications. **Port number is used to distinguish these applications bounded to the same ip address.**  

Each endpoint in a communication session is identified with a unique IP address and port combination. This combination in known as **Socket.**

Port helps **address the packet to specific applications** on hosts.

- IP addresses identify end systems but ports identify an application on the end system.

- Every application has a 16-bit port number. It range from 0 to 2^16 = 65535
- The ports 0 to 1023 are reserved for specific applications and **called well-known ports**
  - Port 80 is reserved for HTTP traffic

- The ports 1024 - 49152 are known as **registered ports**, and they are used by specific applications that are known but not system defined

  - SQL server uses port 1433
  - Best practice is not to use these ports for any user defined applications, but there is no technical restriction on using them

  

## OSI Model & Layers

There are several models along which computer networks are organized. The two most common ones are the **Open Systems Interconnection (OSI)** model and the **Transmission Control Protocol/Internet Protocol (TCP/IP)** model.

The model splits up a communication system into **7 abstract layers**, stacked upon each other.

1. Application
2. Presenation
3. Session
4. Transport
5. Network
6. Data Link
7. Physical



## TCP/IP Model

The **Internet Protocol (IP)** and the **Transmission Control Protocol (TCP)**.

5 Layers:

1. Application
2. Transport
3. Network
4. Data Link
5. Physical



# Application Layer

### 