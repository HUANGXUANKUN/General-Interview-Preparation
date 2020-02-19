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

The main jobs of the application layer is to enable **end users** to access the Internet via a number of applications.

This involves:

- **Writing data off to the network** in a format that is compliant with the protocol in use.
- **Reading data** from the end user
- **Providing useful applications** to end users
- Some application also ensure data from the end-user is in the correct format
- Some applications provide error handling and recovery



## Program vs Process

- A **program** is simply an executable file(application).
- A **process** is any currently running instance of a program. A program can have several copies of it running at once.
- A **thread** is lightweight process. One process can have multiple running threads. The difference between threads and processes is that threads do lightweight singular jobs. 



## Sockets

Processes on different machines send messages to each other through the computer network. The interface between a process (a running instance of application) and the computer network is called a **socket**. 



## Addressing

Messages have to be addressed to a certain application on certain end system. 

Via addressing constructs like IP addresses and ports. 



## Ports

Since every end system may have number of applications running, ports are used to address the packet to specific applications. 



## **Ephemeral Ports**

It is used for several instances(processes) of an application are running at once.

Different port numbers are dynamically generated for each instance of an application. The port is freed once the application is done using it.

Furthermore, server processes need to have well defined and fixed port numbers so that clients can connect to them in a systematic and predictable way. However, clients don't need to have reserved ports. They can use ephemeral ports. Servers can also use ephemeral ports in addition to the reserved ones. For instance, a client makes the initial connection to the server on a well-known port and the rest of the communication is carried out by connecting to an ephemeral port on the server.



# HTTP 

## The basics

The Internet was an obscure set of methods for file transfer and email used by academics and researchers. The World Wide Web was invented to allow the European research organization CERN to present documents linked by hypertexts. All of that changed though when it caught the public’s eye and popularized the Internet. The web was different from other services such as cable television, because it served content based on demand. People could watch what they wanted. **HTTP** or **HyperText Transfer Protocol** is the protocol at the core of the web.

Objects:

- Web pages are objects that consist of other objects.
- An object is simply a file like an HTML file, PNG file, MP3 file, etc.
- Each object has a URL
- The base object of a web page if often an HTML file that has references to other objects by making request for them via their URL.

### URL

A Universal Resource Locator (URL), is used to locate files that exist on servers. URLs consist of the following parts:

- **Protocol** in use
- The **hostname** of the server
- The **location of the file**
- **Arguments** to the file 

### HTTP Requires Lower Layer Reliability

- Application layer protocols rely on underlying transport layer protocols called UDP (User Datagram Protocol) and TCP (Transmission Control Protocol).
- **TCP ensures that messages are always delivered.** Messages get delivered in the order that they are sent.
- **UDP does not ensure that messages get delivered**.
- 

