# Introduction

## TCP

- Tcp is a protocol to allow end systems to communicate effectively.

- It ensure data reaching desired destination and is not corrupted.



## UDP

- The User Datagram Protocol (UDP)
- It does not ensure that data reaches the destination and that remain incorrupt.



## HTTP

- Hypertext Transfer Protocol (HTTP) 
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

**URL**

A Universal Resource Locator (URL), is used to locate files that exist on servers. URLs consist of the following parts:

- **Protocol** in use
- The **hostname** of the server
- The **location of the file**
- **Arguments** to the file 

**HTTP Requires Lower Layer Reliability**

- Application layer protocols rely on underlying transport layer protocols called UDP (User Datagram Protocol) and TCP (Transmission Control Protocol).
- **TCP ensures that messages are always delivered.** Messages get delivered in the order that they are sent.
- **UDP does not ensure that messages get delivered**. This means some messages may get dropped and so never be received.
- **HTTP uses TCP** as its underlying transport protocol so that messages are guaranteed to get delivered in order. This allows the application to function without having to build any extra reliability as it would've had to with UDP.

- TCP is connection-oriented, meaning a connection has to be initiated with servers using a series of starting messages.
- Once the connection has been made, the client exchanges messages with the server until the connection is officially closed by sending a few ending messages.

### Types of HTTP connections

There are two kinds of HTTP connections:

- **Non-persistent HTTP connections**
- **Persistent HTTP connections**

#### Non-persistent HTTP

Non-persistent HTTP connections use **one TCP connection per request.** Assume a client requests the base HTML file of a web page. Here is what happens:

1. The client initiates a TCP connection with a server
2. The client sends an HTTP request to the server
3. The server retrieves the requested object from its storage and sends it
4. The client receives the object which in this case in an HTML file. If that file has references to more objects, step 1-4 are repeated for each of those.
5. The server closes the TCP connection

For each HTTP request, more requests tend to follow, as well to fetch images, JavaScript files, CSS files, and other objects.

The underlying TCP connection requires three TCP messages are sent between the client and server. Similarly, when the connection is closed, three TCP messages are sent back and forth between the client and server.

#### Persistent HTTP

An HTTP session typically involves multiple HTTP request-response pairs, for which separate TCP connections are established and then torn down between the same client and server. This is inefficient. Later on, **Persistent HTTP** was developed, which used a single client-server TCP connection for all the HTTP request-responses for a session.

Typically, if there have been no requests for a while, the server closes the connection. The duration of time before the server closes the connection is configurable.

## HTTP Request Message

Let’s look at request messages first. Here is an example of a typical HTTP message:

```http
GET /path/to/file/index.html HTTP/1.1 
Host: www.educative.io
Connection: close
User-agent: Mozilla/5.0
Accept-language: fr
Accept: text/html
```

It should be noted that,

- HTTP messages are in **plain ASCII text**
- The first line is called the **request line** while the rest are called **header lines**.
- **Each line** of the message **ends with** two control characters: a **carriage return and a line feed `\r\n`**
  - The last line of the message also ends with a carriage return and a line feed!
- This particular message has 6 lines, but HTTP messages can have **one or as many lines as needed**.

**The Anatomy of an HTTP Request Line** [#](https://www.educative.io/courses/grokking-computer-networking/xoNPoDWnjpl#the-anatomy-of-an-http-request-line)

The HTTP request line is followed by an HTTP header. We’ll look at the request line first. The request line consists of three parts:

- **Method**
- **URL**
- **Version**

### HTTP Methods

HTTP methods tell the server what to do. There are a lot of HTTP methods but we’ll study the most common ones: `GET`, `POST`, `HEAD`, `PUT`, or `DELETE`.

- **`GET`** is the most common and **requests data**.
- **`POST`** **puts an object on the server**.
  - This method is generally used when the client is not sure where the new data would reside. The server responds with the location of the object.
  - The data posted can be a message for a bulletin board, newsgroup, mailing list, a command, a web form, or an item to add to a database.
  - The POST method technically requests a page but that depends on what was entered.
- **`HEAD`** is similar to the `GET` method except that **the resource requested does not get sent in response. Only the HTTP headers are sent** instead.
  - This is useful for quickly retrieving meta-information written in response headers, without having to transport the entire content. In other words, it’s useful to check with minimal traffic if a certain object still exists. This includes its meta-data, like the last modified date. The latter can be useful for caching.
  - This is also useful for testing and debugging.
- **`PUT`** **uploads an enclosed entity under a supplied URI**. In other words, it **puts** data at a specific location. If the URI refers to an already existing resource, it’s replaced with the new one. If the URI does not point to an existing resource, then the server creates the resource with that URI.
- **`DELETE`** **deletes an object** at a given URL.

However, sending forms with a POST request is generally better because:

1. The amount of data that can be sent via a post request is unlimited.
2. The form’s fields are not shown in the URL.

**URL**

This is the location that any HTTP method is referring to.

**Version**

The HTTP version is also specified in the request line. The latest version of HTTP is [HTTP/2](https://en.wikipedia.org/wiki/HTTP/2).

**The Anatomy of HTTP Header Lines**

The HTTP request line is followed by an HTTP header. A lot of HTTP headers exist! We’ll be covering the most important ones in this lesson. However, if you’re interested, you can [read further](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields) about all of them.

- The first header line specifies the `Host` that the request is for.
- The second one defines the type of HTTP `Connection`. It’s Non-persistent in the case of the following drawing as the connection is specified to be closed.
- The `user-agent` line specifies the client. This is useful when the server has different web pages that exist for different devices and browsers.
- The `Accept-language` header specifies the language that is preferred. The server checks if a web page in that language exists and sends it if it does, otherwise the server sends the default page.
- The `Accept` header defines the sort of response to accept. It can be anything like HTML files, images, and audio/video.

## HTTP Response Message

Let’s start with a typical example of an HTTP response message:

```http
HTTP/1.1 200 OK
Connection: close
Date: Tue, 18 Aug 2015 15: 44 : 04 GMT
Server: Apache/2.2.3 (CentOS)
Last-Modified: Tue, 18 Aug 2015 15:11:03 GMT 
Content-Length: 6821
Content-Type: text/html

[The object that was requested]
```

- It has 3 parts: an initial **status line**, some **header lines** and an **entity body**.

### Status Line

- HTTP response status lines start with the **HTTP version**.

- The **status code** comes next which tells the client if the request succeeded or failed.
- There are a lot of status codes:
  - 1xx codes fall in the informational category
  - 2xx codes fall in the success category
  - 3xx codes are for redirection
  - 4xx is client error
  - 5xx is server error

Here is a list of some common status codes and their meanings:

- **`200 OK`**: the request was successful, and the result is appended with the response message.
- **`404 File Not Found`**: the requested object doesn’t exist on the server.
- **`400 Bad Request`**: generic error code that indicates that the request was in a format that the server could not comprehend.
- **`500 HTTP Internal Server Error`**: the request could not be completed because the server encountered some unexpected error.
- **`505 HTTP Version Not Supported`**: the requested HTTP version is not supported by the server.

### Header Lines

Let’s study the header lines.

- **Connection type**. In this case, indicates that the server will `close` the TCP connection after it sends the response.
- **Date**. The date at which the response was generated.
- **Server**. Gives server software specification of the server that generated the message. Apache in this case.
- **Last-Modified.** The date on which the object being sent was last modified.
- **Content-Length.** The length of the object being sent in 8-bit bytes.
- **Content-Type.** The type of content. The type of the file is not determined by the file extension of the object, but by this header.

The **response** body contains the file requested.

**How HTTP Headers Are Chosen**

Lastly, you must be wondering how browsers decide which HTTP headers to include in requests and how servers decide which headers to return in the response. That **depends on a complex mix of factors such as the browser, the user configurations and products**.



## Cookies

HTTP is a stateless protocol, but we often see websites where session state is needed. For instance, imagine you are browsing for products on an e-commerce website. How does the server know if you are logged in or not, or if the protocol is stateless? How does the server know what’s in your shopping cart when checking out if the protocol is stateless? Cookies allow the server to keep track of this sort of information.

- Cookies are **unique string identifiers** that can be stored on the client's browser.
- These identifiers are **set by the server through HTTP headers** when the client first navigates to the website.
- After the cookie is set, it's sent along with subsequent HTTP requests to the same server. This **allows the server to know who is contacting it** and hence serve content accordingly.

So the HTTP request, the HTTP response, the cookie file on the client's browser, and a database of cookie-user values on the server end are all involved in the process of setting and using cookies.

**`Set-cookie` Header**

Let’s look at how cookies work in a bit more detail. When a server wants to set a cookie on the client-side, it includes the header `Set-cookie: value` in the HTTP response. This value is then **appended to a special cookie file stored on your browser**. The cookie file contains:

- The website’s domain
- The string value of the cookie
- The date that the cookie expires (yes, much like actual cookies, they do expire)

## Email: SMTP

There are many protocols associated with email. One popular choice is a combination of `POP3` and `SMTP`. One is used to send emails that are stored in a user’s inbox and the other is used to retrieve emails sent to you. However, the very core of electronic mail is the **Simple Mail Transfer Protocol (SMTP)**.

- SMTP uses **TCP**, which means that transfers are **reliable**. The connection is established at `port 25`.

**How SMTP Works**

Let’s look at how this ubiquitous protocol works.

1. When an email is sent, its sent to the sender’s **SMTP server** using the SMTP protocol.
   - The SMTP server is configured in your email client. The general format of the domain of the SMTP server is `smtp.example.com` where the main email address of the sender is `user@example.com`. But it’s not mandatory to adhere to this format. We could set up, say, `zeus.example.com` to serve as our SMTP server, if we wanted. From a security point of view, it is probably a good idea, since people are unlikely to guess it as easily.
2. The **email is now placed on a message queue in the sending SMTP server**.
3. Then, the SMTP server initiates a connection with the recipient server and will conduct an initial SMTP handshake.
   - If the recipient is on the same SMTP server as the sender (for instance [alice@gmail.com](mailto:alice@gmail.com) sending to [bob@gmail.com](mailto:bob@gmail.com)), then the SMTP server doesn’t need to connect to the recipient’s server.
4. The SMTP server will finally send the message to the recipient’s email server.
5. The **email is then downloaded from the recipient’s SMTP server** using other protocols when the recipient logs in **to their email account or 'user agent**.’ In other words, the recipient’s SMTP server copies the email to the recipient’s mail-box.



### **Error Handling**

There are many scenarios where sending an email may fail. Here are a few.

- **If the email is not sent** for any reason such as a misspelled recipient address, the email is returned to the sender with a **“not delivered” message**.
- **If the recipient SMTP server is offline**, the sending SMTP server **keeps trying to send** the email, say, every 30 minutes or so. It **stops trying after a few days and alerts the sender about the mail not delivered error**.
- Also, SMTP does not use any intermediate servers. So, even if an attempt to send an email fails because of any reason such as the receiving server being down, the email won’t be stored on an intermediary server. It will be stored on the sending SMTP server.



## Email: POP & IMAP

POP and IMAP are used to retrieve email from an email server. Either one can be used. Let’s discuss both.

**POP**

The most commonly used version of the **Post Office Protocol (POP)** is version 3, or **POP3**. This is how it works:

**POP Phases**

Emails are simply downloaded from the server in **4 phases: connect, authorize, transaction, update**.

1. **Connect:** The user agent first connects to the POP3 server on TCP using `port 110`.

2. **Authorize:** The user agent authenticates the user with a username and a password.

3. **Transaction:** The user can now retrieve emails and mark emails for deletion.

4. Update:

    

   After the user agent quits and closes the POP3 session, the server makes updates based on the user’s commands. So if the user marked an email for deletion, it will delete it. No copy of a deleted email is kept on the server.

   - Note that only what’s in the user’s inbox is downloaded. Other folders such as sent items, outbox, or drafts are not synced. So `POP3` does not synchronize the folders.

**POP Modes** 

POP works in two modes.

- **Download and delete**: Once emails are downloaded from the server to the user agent, they are all deleted from there.
- **Download and keep**: Emails are not deleted from the server once they are downloaded onto the user agent.

However, with the download and delete model, you can only use one client to check your emails. If you use multiple devices to check your email, this method is not appropriate because emails will not look the same across devices at different times. Also, users won’t be able to reread emails from different devices.

Have a look at the following slides for an example of how emails might not be in sync on multiple devices with POP.

**IMAP**

The **Internet Message Access Protocol (IMAP)**, like POP, is also a mail access protocol used for retrieving email. It is a bit more complex than POP and hence allows you to view your email from multiple devices. With IMAP, though:

- Emails are **kept** on the server and **not deleted**.
- Local copies of the emails are cached on each device.
- It **syncs** up all of the **user’s folders** including custom folders.
- The **inbox** would look exactly the **same on all clients**.
- If an email is deleted from one user agent, it will be **deleted off the server**.
- Deleted emails **won’t be visible** from other devices either.

# DNS (**Domain Name System**)

**Domain Name System**

It is a client-server application layer protocol that translates hostnames on the Internet to IP addresses.

Generally, using one of the following ways:

- **Addresses** or **locations** that specify where something is.
- **Names**. In particular, domain names, or the unique name that identifies a websites, are mapped into IP addresses based on lookup service that uses a database. The most well-known lookup service is the **Domain Name System (DNS)**. So when you enter the URL ‘[educative.io](http://educative.io/)’ into your browser, it uses DNS to find the actual IP address of the server that hosts it.
- **[Content-based addressing](https://en.wikipedia.org/wiki/Content-addressable_storage)**.
  - The content itself is used to look up its location.

## **Distributed Hierarchical Database**

One single database on one single server does not scale for reasons such as:

- Single point of failure. If the server that has the database crashes, DNS would stop working entirely, which is too risky.
- Massive amounts of traffic. Everyone would be querying that one server. It will not be able to handle that amount of load.
- Maintenance. Maintaining the server would become critical to the operation of DNS.
- Location. Where would the server be located?

This is why DNS employs several servers, each with part of the database. Also, the servers exist in a hierarchy. To understand this hierarchy better, you need to understand how URLs are broken down into their hierarchies. Have a look at the following example:

```
discuss.educative.io
```

- `discuss`: sub-domain
- `educative`: second-level domain
- `io`: top-level domain

## **DNS Namespaces**

The parts of the URL above roughly map to DNS servers. These servers manage the abstract space of domains. The servers all exist in a *hierarchy*. Have a look at the following diagram.

## Root DNS Servers

Root DNS servers are the first point of contact for a DNS query (after the client’s local cache of names and IP addresses). They exist at the top of the hierarchy and point to the appropriate TLD(Top Level Domain) server in reply to the query. So a query for `educative.io` would return the IP address of a server for the top-level domain, `io`.

As of the writing of this course, there are 1017 instances of root servers operated by 12 different organizations. To get a full list and an interactive map, have a look at [root-servers.org](http://root-servers.org/).

## Top-level Servers [#](https://www.educative.io/courses/grokking-computer-networking/39LZ8PvPLnR#top-level-servers)

Servers in the top-level domain hold mappings to DNS servers for certain domains. Each domain is meant to be used by specific organizations only. Here are some common domains:

- **com**
- **edu**
- **gov**
- **mil**
- **net**: It was initially intended for use by organizations working in network technology such as ISPs, but it is now a general purpose domain like com.

## Authoritative Servers

Every organization with a public website or email server provides DNS records. These records have **hostname** **to** **IP address** mappings stored for that organization. These records can either be stored on a dedicated DNS server for that organization or they can pay for a service provider to store the records on their server.

This is the next link in the chain. If this server has the answer that we are looking for, the IP address that it has is finally returned to the client. However, this server may not have the sought after answer if the domain has a **sub-domain**. In that case, this server *may* point to a server that has records of the subdomain.

For instance, if the DNS record for [cs.stanford.edu](http://cs.stanford.edu/) is being looked for, a DNS server separate from ‘[stanford.edu](http://stanford.edu/)’ may hold records for the sub-domain `cs`

## Local DNS 

- DNS mappings are often also cached locally on the client end-system to avoid repetitive lookups and saves time for often visited websites.
- This is done via an entity called the **local resolver library**, which is part of the OS. The application makes an API call to this library. This library manages the local DNS cache.
- If the local resolver library does not have a cached answer for the client, it will contact the organization's local DNS server.
- This local DNS server is typically configured on the client machine either using a protocol called **DHCP** or can be configured statically.
- **So, if it's configured manually, any local DNS server of the client's choice can be chosen,. A few open DNS servers are incredibly popular, such as the ones by Google.**

## Local DNS Servers

There is one type of server that we ignored, the **local DNS Server**.

Local DNS servers are usually the first point of contact after a client checks its local cache. These servers are generally hosted at the ISP and contain some mappings based on what websites users visit.

![image-20200220010705286](C:\Users\xk\Desktop\Interview preparation\imgs\image-20200220010705286.png)



## Resource Records

The DNS distributed database consists of entities called **RR**s, or **Resource Records**.



**Format **

RRs are 4-tuples with the following entries:

```shell
(name, value, type, ttl)
```

Every resource record has a `type` and a `TTL` along with a `name-value` pair. The TTL specifies **how long an RR entry can be cached by the client**. The remaining fields are described for each RR type below.

**Types of RRs**

- Address
  - Type `A` addresses are used to map IPv4 addresses to hostnames.
  - `name` is the hostname in question.
  - `value` is the IP address of the hostname.
  - **Example:** `educative.io. 299 IN A 104.20.7.183` where 299 is the TTL, [educative.io](http://educative.io/) is the name, `A` is the type, and `104.20.7.183` is the value.
- Canonical name
  - Type `CNAME` records are records of alias hostnames against actual hostnames. For example if, `ibm.com` is really `servereast.backup2.com`, then the latter is the canonical name of `ibm.com`.
  - `name` is the alias name for the real or ‘canonical’ name of the server.
  - `value` is the canonical name of the server.
  - **Example:** `bar.example.com. CNAME foo.example.com.`
- Mail Exchanger
  - We have seen this one before! Type `MX` records are records of the server that accepts email on behalf of a certain domain.
  - The `name` is the name of the host.
  - `value` is the name of the mail server associated with the host.
  - **Example:** `educative.io mail exchanger = 10 aspmx2.googlemail.com.`

These resource records are stored in text form in special files called **zone files**.



## DNS Messages

There are a few kinds of DNS messages, out of which the most common are **query** and **reply**, and both have the same format. Study the following slides for a detailed overview of a DNS message.![image-20200220013225543](C:\Users\xk\Desktop\Interview preparation\imgs\image-20200220013225543.png)

# Transport Layer

![image-20200220013507228](C:\Users\xk\Desktop\Interview preparation\imgs\image-20200220013507228.png)

**Key Responsibilities of the Transport Layer **

- **Extends network to the applications**: the transport layer takes messages from the network to applications. In other words, while the network layer (directly below the transport layer) transports messages from one end-system to another, the transport layer delivers the message to and from the relevant application *on* an end-system.

- **Logical application-to-application delivery**, the transport layer makes it so that applications can address other applications on other end-systems directly. This is true even if it exists halfway across the world. So it provides a layer of **abstraction**.
- **Segments data**. The transport layer also divides the data into manageable pieces called ‘segments’ or ‘datagrams.’
- **Can allow multiple conversations**. Tracks each application to application connection or ‘conversation’ separately, which can allow multiple conversations to occur at once.
- **Multiplexes & demultiplexes data**. It ensures that the data reaches the relevant application *within* an end-system. So if multiple packets get sent to one host, each will end up at the correct application.

**Transport Layer Protocols [#](https://www.educative.io/courses/grokking-computer-networking/gxQl90g1279#transport-layer-protocols)**

The transport layer has two prominent protocols: the **transmission control protocol** and the **user datagram protocol**. 

## TCP vs UDP

**TCP**

- Delivers messages that we call ‘segments’ reliably and in order.
- Detects any modifications that may have been introduced in the packets during delivery and corrects them.
- Handles the volumes of traffic at one time within the network core by sending only an appropriate amount of data at one time.
- Examples of applications/application protocols that use TCP are: HTTP, E-mail, File Transfers.

**UDP**

- Does not ensure in-order delivery of messages that we call ‘datagrams.’
- Detects any modifications that may have been introduced in the packets during delivery but does not correct them by default.
- Does not ensure reliable delivery.
- Generally faster than TCP because of the reduced overhead of ensuring uncorrupted delivery of packets in order.
- Applications that use UDP include: Domain Name System (DNS), live video streaming, and Voice over IP (VoIP).

## Multiplexing & Demultiplexing

**Multiplexing** allows messages to be sent to more than one destination host via a single medium.

**Demultiplexing** is the process of delivering the correct packets to the correct applications from one stream.

**How Do They Work in the Transport Layer?**

The transport layer **labels** **packets with the port number** of the application a message is from and the one it is addressed to. This is what allows the layer to multiplex and demultiplex data.

## Multiplexing & Demultiplexing in UDP [#](https://www.educative.io/courses/grokking-computer-networking/JE4wpnQYl2K#multiplexing-demultiplexing-in-udp)

- When a datagram is sent out from an application, the port number of the associated **source** and **destination** application is appended to it in the UDP protocol header.
- When the datagram is received at the receiving host, it sends the datagram off to the relevant application’s socket based on **destination port number**.
- If the source port and source IP address of two datagrams are different but the destination port and IP address are the same, the datagrams will still get sent to the same application.

![image-20200220133731474](C:\Users\xk\Desktop\Interview preparation\imgs\image-20200220133731474.png)

## Congestion

When more packets than the network has bandwidth for are sent through, some of them start getting dropped and others get delayed. This phenomenon leads to an overall drop in performance and is called **congestion**.

This is analogous to vehicle traffic congestion when too many vehicles drive on the same road at the same time. This slows the overall traffic down.

**Congestion Control**

Congestion control is really just congestion avoidance. Here’s how the transport layer controls congestion:

1. It sends packets at a slower rate in response to congestion,
2. The ‘slower rate’ is still fast enough to make efficient use of the available capacity,
3. Changes in the traffic are also kept track of.

Congestion control algorithms are based on these general ideas and are built into transport layer protocols like **TCP**. Let’s also look at a few principles of bandwidth allocation before moving on.

### Max-min Fairness

Furthermore, the congestion control scheme should be **fair**. Most congestion schemes aim at achieving **max-min fairness**. An allocation of transmission rates to sources is said to be **max-min fair** if:

1. No link in the network is congested
2. The rate allocated to a source *j* cannot be increased without decreasing the rate allocated to another source *i*, whose allocation is smaller than the rate allocated to the source *j*.

### Convergence 

Additionally, bandwidth should be allocated such that it converges to a fair (a host does not hog all of it), and efficient value. This means it should not oscillate and most of it will be used. Furthermore, it should also change in response to changes in the demands of the network over time. Here are some slides that demonstrate this convergence.

![image-20200220142306257](C:\Users\xk\Desktop\Interview preparation\imgs\image-20200220142306257.png)



## Principles of Reliable Data Transfer

**Network Layer Imperfections [#](https://www.educative.io/courses/grokking-computer-networking/397GLvojwMA#network-layer-imperfections)**

The transport layer must deal with the imperfections of the network layer service. There are three types of imperfections that must be considered by the transport layer:

1. Segments can be **corrupted** by transmission errors
2. Segments can be **lost**
3. Segments can be **reordered** or **duplicated**

### Checksums

The first imperfection of the network layer is that segments **may be corrupted by transmission errors**. The simplest error detection scheme is the **checksum**.

A checksum can be based on a number of schemes. One possible scheme is an arithmetic sum of all the bytes of a segment. Checksums are computed by the sender and attached with the segment. The receiver verifies it upon reception and can choose what to do in case it is not valid. Quite often, the segments received with an invalid checksum are **discarded**.

### Retransmission Timers  (**stop and wait**)

The second imperfection of the network layer is that **segments may be lost**. Since the receiver sends an acknowledgment segment after having received each data segment, the simplest solution to deal with losses is to use a **retransmission timer**.

A retransmission timer starts when the sender sends a segment. The value of this retransmission timer should be *greater* than the **round-trip-time**, for example, the delay between the transmission of a data segment and the reception of the corresponding acknowledgment. Note that TCP sends an acknowledgment for almost every segment! We’ll look at this in more detail in later lessons. When the retransmission timer expires, the sender assumes that the data segment has been lost and retransmits it.

### Sequence Number

To identify duplicates, transport protocols associate an *identification number* with each segment called the **sequence number**. This sequence number is prepended to the segments and sent. This way, the end entity can identify duplicates.

## Reliable Data Transfer: Sliding Window

The sliding window is the set of consecutive sequence numbers that the sender can use when transmitting segments without being forced to wait for an acknowledgment. At the beginning of a session, the sender and receiver agree on a sliding window size.

The figure below illustrates the operation of the sliding window. The sliding window shown contains three segments. The sender can thus transmit three segments before being forced to wait for an acknowledgment. The sliding window moves to the higher sequence numbers upon reception of acknowledgments. When the first acknowledgment (of segment 0) is received, it allows the sender to move its sliding window to the right, and sequence number 3 becomes available.

In practice, as the segment header encodes the sequence number in a n*n* bits string, only the sequence numbers between 00 and 2^{n}2*n* − 11 can be used. There’s a problem here. What happens when an application has transmitted 2^n -12*n*−1 messages and still has more data to send? Well, the same sequence number can be used for different segments and that the sliding window can wrap. This is illustrated in the figure below assuming that 22 bits are used to encode the sequence number in the segment header. Note that upon reception of the acknowledgment for the first segment, the sender slides its window and can use sequence number 00 again.

Unfortunately, **segment losses** do not disappear because a transport protocol is using a sliding window. To recover from segment losses, a sliding window protocol must define:

- **A heuristic to detect segment losses.**
- **A retransmission strategy to retransmit the lost segments.**

### Go-back-n

In the last lesson, we discovered that a sending sliding window alone is not enough to ensure **detection and retransmission of lost packets**. In order to do that, we will look at two protocols:

1. **Go-back-n**
2. **Selective Repeat**

### 

The simplest sliding window protocol uses **go-back-n** recovery.

**Go-back-n Receiver**

Intuitively, go-back-n receiver operates as follows:

1. It only accepts the segments that arrive in-sequence.
2. It discards any out-of-sequence segment that it receives.
3. When it receives a data segment, it always returns an acknowledgment containing the sequence number of the **last in-sequence segment** that it has received.

**Cumulative Acknowledgements**

This acknowledgment is said to be **cumulative**. When a go-back-receiver sends an acknowledgment for sequence number x*x*, it implicitly acknowledges the reception of all segments whose sequence number is smaller than or equal to x*x*.

A **key advantage** of these cumulative acknowledgments is that it’s **easy to recover from the loss of an acknowledgment**.

Consider for example a go-back-n receiver that received segments 1, 2 and 3.

1. It sent acknowledgments for all three segments.
2. Unfortunately, acknowledgments of the first two were lost.
3. Thanks to the cumulative acknowledgments, the receiver receives the acknowledgment for the last segment and so it knows that all three have been correctly received.

**Go-back-n Sender**

A go-back-n sender uses a sending buffer that can store an entire sliding window of segments.

- The segments are sent with a sending sliding window that we looked at in the last lesson.
- The sender must wait for an acknowledgment once its sending buffer is full.
- When a go-back-n sender receives an acknowledgment, it removes all the acknowledged segments from the sending buffer.

**Retransmission Timer**

A go-back-n sender uses a **retransmission timer** to detect segment losses. It maintains one retransmission timer per connection. Here’s how it works:

1. This timer is started when the first segment is sent.
2. When the go-back-n sender receives an acknowledgment, it restarts the retransmission timer, but only if any unacknowledged segments exist in its sending window.
3. When the retransmission timer expires, the go-back-n sender assumes that all of the unacknowledged segments currently stored in its sending buffer have been lost. It thus retransmits all the unacknowledged segments in the buffer and restarts its retransmission timer.

![image-20200220144653169](C:\Users\xk\Desktop\Interview preparation\imgs\image-20200220144653169.png)

Pros and Cons of Go-back-n [#](https://www.educative.io/courses/grokking-computer-networking/N811q0JmwAz#advantages-of-go-back-n)

**Pros:**

The main advantage of go-back-n is that it can be easily implemented, and it can also provide good performance when only a few segments are lost. 

**Cons:**

But when there are many losses, the performance of go-back-n quickly drops for two reasons:

- The go-back-n receiver does not accept out-of-sequence segments.
- The go-back-n sender retransmits all unacknowledged segments once it has detected a loss.

Since the go-back-n protocol does not accept out of order segments, it can waste a lot of bandwidth if segments are frequently lost.

### Selective Repeat

- Uses a sliding window protocol just like go-back-n.
- The window size should be less than or equal to half the sequence numbers available. This avoids packets being identified incorrectly. Here’s an **example**: suppose the window size is greater than half the buffer size.
  - Segment ‘1’ is lost, hence the receiver expects a segment with sequence number 1 to be retransmitted.
  - Meanwhile, the window wraps around back to sequence number ‘1.’
  - The sender sends a new packet with sequence number 1 and the receiver perceives it to be the original one that it was expecting.

- Senders retransmit unacknowledged packets after a timeout or if a *NAK* (*negative acknowledgment/not acknowledged*) is received.
- The receiver acknowledges all correct packets.
- The receiver stores correct packets until they can be delivered in order to the upper application layer.

### Go-Back-N vs Selective Repeat

In comparison to the go-back-n protocol, selective repeat conserves bandwidth, which is, the rate of data transfer across a path.



## UDP in Detail

### Header

UDP prepends **four** 2-byte header fields to the data it receives from the application layer. So in total, a UDP header is **8 bytes** long. The fields are:

1. **Source** port number
2. **Destination** port number
3. **Length** of the datagram (header and data in bytes)
4. **Checksum** to detect if errors have been introduced into the message. We’ll study this in detail in the [next lesson](https://www.educative.io/collection/page/10370001/6105520698032128/6036281094045696)!



### Data

Other than the headers, a UDP datagram contains a body of data which can be up to **65,527** bytes long. Since the maximum possible length of a UDP datagram is 65,535 bytes which includes the 88-byte header, we are left with 65,527 bytes available. The nature of the data depends on the overlying application. So if the application is querying a DNS server, it would contain bytes of a zone file.

Here’s what a UDP message looks like:

![image-20200220151842850](C:\Users\xk\Desktop\Interview preparation\imgs\image-20200220151842850.png)

### UDP Check Sum Calculation

UDP detects if any changes were introduced into a message while it traveled over the network. To do so, it appends a ‘checksum’ to the packet as a field that can be checked against the message itself to see if it was corrupted. It’s calculated the same way as in TCP. Here’s a refresher with some extra information:

1. The payload and some of the headers (including some IP headers) are all divided into 16-bit words.
2. These words are then added together, wrapping any overflow around.
3. Lastly, the one’s complement of the resultant sum is taken and appended to the message as the checksum.

### Why UDP?

You might be wondering why would anyone use UDP when it has so many apparent drawbacks and doesn’t really do anything? Well, there are actually a number of reasons why UDP would be a good choice for certain applications.

1. UDP can be **faster**. Some applications cannot tolerate the load of the retransmission mechanism of TCP, the other [transport layer protocol](https://www.educative.io/collection/page/10370001/6105520698032128/4608034330378240).
2. **Reliability can be built on top** of UDP. TCP ensures that every message is sent by resending it if necessary. However, this reliability can be built in the application itself.
3. UDP gives **finer control** over what message is sent and when it is sent. This can allow the application developer to decide what messages are important and which do not need concrete reliability.
4. Going on points 3 and 4, **UDP allows custom protocols to be built on top of it**.
   - In fact, Google’s transport layer protocol, **Quick UDP Internet Connections (QUIC)**, pronounced *quick*, is an experimental transport layer network protocol built on top of UDP and designed by Google. The overall goal is to reduce latency compared to that of TCP. It’s used by most connections from the Chrome web browser to Google’s servers!
5. With the significantly smaller header gives UDP an edge over TCP in terms of reduced transmission overhead and quicker transmission times.



## TCP in Detail

Introduction to TCP [#](https://www.educative.io/courses/grokking-computer-networking/7Xw8KoRLYqQ#introduction-to-tcp)

TCP, or the **transmission control protocol**, is one of the two key protocols of the transport layer. TCP is what makes most modern applications as enjoyable and reliable as they are. HTTP’s implementation, for example, would be very complex, if it weren’t for TCP.

TCP is a robust protocol meant to adapt to a diverse range of network topologies, bandwidths, delays, message sizes, and other varying factors that exist in the network layer.



Here are some key responsibilities of the protocol.

1. **Send data** at an appropriate transmission rate. It should be a fast enough rate to make full use of the available capacity but it shouldn’t be so fast as to cause congestion.
2. **Segment data.** The application layer sends the transport layer a continuous and unsegmented stream of data so that there’s no limit to how much data the application layer can give to the transport layer at once. Hence, the transport layer divides it into appropriately sized **segments**. Note that a segment is a collection of bytes. Furthermore, when a TCP segment is too big, the network layer may break it into multiple network layer messages, so the receiving TCP entity would have to re-assemble the network layer messages.
3. **End to end flow control**. Flow control means **not overwhelming the receiver**. It’s not the same as congestion control. Congestion control tries not to choke the network. However, if the receiving machine is slow, it might drown in data even if the network is not choked. Avoiding drowning the receiver in data is end to end flow control. There is also hop by hop flow control, which is done at the data link layer.
4. **Identify and retransmit messages** that do not get delivered. The network layer cannot be relied upon to deliver messages.
5. **Identify when messages are received out of order and reassemble** them. The network layer can also *not* be relied upon to transmit messages in order.

## Well-Known Applications That Use TCP

- **File Transfer** [#](https://www.educative.io/courses/grokking-computer-networking/7Xw8KoRLYqQ#file-transfer)
  - FTP or **File Transfer Protocol** is built on top of TCP. It uses ports **20** and **21**. When transferring files, we wouldn’t want some bytes of the file completely missing, or some chunks in the file re-ordered or some byte values changed during transfer. That’s why TCP is a natural choice for FTP. In other words, it uses TCP for its **reliability**, which is a key part of file transfer.
- **Secure Shell** `SSH` [#](https://www.educative.io/courses/grokking-computer-networking/7Xw8KoRLYqQ#secure-shell-ssh)
  - SSH or **Secure Shell** is a protocol to allow a secure connection to a remote host over an unsecured network. It’s widely popular and most programmers use it to date to execute operating system shell commands on remote servers. The reasons that this application uses TCP is similar to FTP, and that’s reliable delivery.
- **Email** [#](https://www.educative.io/courses/grokking-computer-networking/7Xw8KoRLYqQ#email)
  - All email protocols, SMTP, IMAP, and POP use TCP to ensure complete and reliable message delivery similar to the reasons that FTP uses TCP.
- **Web Browsing** [#](https://www.educative.io/courses/grokking-computer-networking/7Xw8KoRLYqQ#web-browsing)
  - Web browsing on both HTTP and HTTPS is done on TCP as well for the same reasons as FTP.

## TCP Segment Header

TCP headers play a crucial role in the implementation of the protocol. In fact, TCP segments without actual data and with headers are completely valid. They’re actually used quite often!

The size of the headers range from **20 - 60 bytes**. Let’s discuss the header field by field.

The source and destination port numbers are self-explanatory. They are exactly like the source and destination ports in UDP. Just for a refresher though, the source port is the port of the socket of the application that is **sending** the segment and the **destination port** is the port of the socket of the **receiving** application. The size of each field is **two bytes**.



**Content in Segment Heads:**

- **Sequence Number**: Every byte of the TCP segment’s data is labeled with a number
- **Acknowledgement Number**: a 4-byte field that represents the sequence number of the next expected segment that the sender will receive
- **Header Length**: The length of the TCP header is specified here. This helps the receiving end to identify where the header ends and the data starts from.
- **Reserved Field:** The header has a 4-bit field that is reserved and is always set to 00. This field aligns the total header size to be in multiples of 4

## TCP Connection Establishment: Three-way Handshake

A TCP connection is established by using a **three-way handshake**, which we briefly touched upon in a previous lesson. The connection establishment phase uses the **sequence number**, the **acknowledgment number**, and the **SYN flag**.

### **1. Initiating a Connection**

When a client host wants to **open a TCP connection** with a server host, it creates and sends a TCP segment with:

- **The *SYN* flag set**
- **The sequence number set to a random initial value**. So the sequence numbers **do not start with 0**.
  - To prevent **Sequence Prediction Attack**. Attacker must guess the sequence number

### **2. Responding to an Initial Connection Message**

Upon reception of this segment (which is often called a *SYN* segment), the server host replies with a segment containing:

- the ***SYN* flag** set
- the *sequence number* set to a random number.
- The ***ACK* flag** set
- The **acknowledgment number** set to the (sequence number + 1) mod 2^32. 2^32 is the limit of ACK header field.

### 3. Acknowledging The Response

Upon reception of the *SYN+ACK* segment, the client host replies with a segment containing:

- The ***ACK* flag** set
- The **acknowledgment number** set to the sequence number of the received *SYN+ACK* segment incremented by 1. The modulus of the number by 2^{32}232 is obviously taken. At this point, the TCP connection is open and both the client and the server are allowed to send TCP segments containing data. This is illustrated in the figure below

![image-20200220161609872](C:\Users\xk\Desktop\Interview preparation\imgs\image-20200220161609872.png)

## When TCP Connection Establishment Fails: Syn Floods & Retransmission

**Hosts Can Refuse Connection Requests** 

A host could refuse to open a TCP connection upon reception of a *SYN* segment. This refusal may be due to various reasons, for example:

1. There may be **no server process** that’s listening on the destination port of the *SYN* segment.
2. The server could always refuse connection establishments from *a* particular client (e.g., due to **security reasons**).
3. The server may **not have enough resources** to accept a new TCP connection at that time.

There are other scenarios in which a connection may be refused but these are the common ones. If a process is listening on a port, but the connection is to be refused, the server sends a SYN segment with the following properties:

- Has its *RST* flag set
- Contains the sequence number of the received *SYN* segment as its acknowledgment number.

### Syn Flood Attacks

When a TCP entity opens a TCP connection, it creates a **Transmission Control Block (TCB)** that contains the entire state of the connection, including the local sequence number and sequence number sent by the remote client. Until the mid-1990s, TCP implementations had a limit on the number of ‘half-open’ TCP connections (TCP connections in the **SYN RCVD** state) which was most commonly at 100100. So a machine could only have a 100100 ‘half-open’ TCP connections. This was meant to avoid overflowing the entity’s memory with TCBs. When the limit was reached, the TCP entity would stop accepting any *new* *SYN* segments.

**Syn Cookies**

However, this allowed attackers to carry out an attack where they could render a resource unavailable in the network by sending it valid messages. Such attacks are called **Denial of Service (DoS)** attacks because they deny the user(s) a service. Here’s how this one was carried out:

1. The attacker would send a few 100100 SYN segments every second to a server
2. The attacker would not reply to any received *SYN+ACK* segments
3. To avoid being caught, the attacker would send these *SYN* segments with a different IP address from their own IP address.
4. Once a server entered the **SYN RCVD** state, it would remain in that state for several seconds, waiting for an ACK and not accepting any new, possibly genuine connections, thus being rendered unavailable.

To avoid these **SYN flood attacks**, newer TCP implementations reply directly with SYN+ACK segments and wait until the reception of a valid ACK to create a TCB.

The goal is to not store connection state on the server immediately upon reception of a *SYN* packet. But, without this information, the server cannot tell if a subsequent *ACK* it receives is from a legitimate client that had sent a benign *SYN* packet. One way to do it is to verify that if the acknowledgement number contained in the *ACK* packet is y*y*, then the server had sent a sequence number y - 1*y*−1 in the *SYN+ACK* packet. But, again, if we are remembering the initial sequence number for each *SYN* packet, we are back to square one - remembering connection state. The way **SYN Cookie** solves this problem is to use a function that uses some information from the client’s *SYN* packet and some information from the server side to calculate a random initial sequence number. This number, say, y-1*y*−1 is sent to the client in a *SYN + ACK* message. If an *ACK* packet is later received with a sequence number y*y*, using some packet header fields and some server side information, a reverse function can verify that the acknowledgement number is valid. If not, the connection is refused, otherwise a TCB is created and a connection is established.

The advantage of *SYN* cookies is that the server would not need to create and store a TCB upon reception of the *SYN* segment.

### Retransmitting Lost Segments [#](https://www.educative.io/courses/grokking-computer-networking/xl7G66JQ8yn#retransmitting-lost-segments)

Since the underlying Internet protocol provides an unreliable service, the *SYN* and *SYN+ACK* segments sent to open a TCP connection could be lost. Current TCP implementations start a **retransmission timer** when the first *SYN* segment is sent. This timer is often set to three seconds for the first retransmission, and then doubles after each retransmission ([RFC 2988](https://tools.ietf.org/html/rfc2988)). When the timer expires, the segment is retransmitted. TCP implementations also enforce a maximum number of retransmissions for the initial *SYN* segment. Note that the same technique is used for all TCP segments, not just connection establishment segments.

We’ll look at this in detail when we study TCP reliability in an upcoming lesson!

## TCP Connection Release

TCP, like most connection-oriented transport protocols, supports two types of connection releases:

1. **Graceful** connection release, where the connection is not closed until both parties have closed their sides of the connection.
2. **Abrupt** connection release, where either one user closes both directions of data transfer or one TCP entity is forced to close the connection.

### Abrupt Connection Release

An abrupt release is executed when a *RST* segment is sent. A *RST* can be sent for the following reasons:

- A non-*SYN* segment was received for a **non-existing TCP connection** ([RFC 793](https://tools.ietf.org/html/rfc793.html)).
- Some implementations send a *RST* segment when a segment with an **invalid header** is received on an open connection ([RFC 3360](http://tools.ietf.org/html/rfc3360.html)). This causes the corresponding connection to be closed and has prevented attacks ([RFC 4953](https://tools.ietf.org/html/rfc4953)).
- Some implementations send an *RST* segment when they need **to close an existing TCP connection** for any reason such as:
  - There are **not enough resources** to support this connection
  - The remote **host has stopped responding and is now unreachable**.

When a *RST* segment is sent by a TCP entity, it should contain the current value of the sequence number for the connection (or 00 if it does not belong to any existing connection), and the acknowledgment number should be set to the next expected in-sequence sequence number on this connection.

### Graceful Connection Release [#](https://www.educative.io/courses/grokking-computer-networking/qVqgKxrgxmG#graceful-connection-release)

The normal way of terminating a TCP connection is by using the *FIN* flag of the TCP header. This ‘graceful mechanism’ allows each host to release its own side of the connection individually. The utilization of the ***FIN* flag** in the TCP header consumes one sequence number.

**FSM [#](https://www.educative.io/courses/grokking-computer-networking/qVqgKxrgxmG#fsm)**

The following figure shows an FSM that depicts the various ‘graceful’ ways that a TCP connection can be released.

Don’t feel overwhelmed if you don’t understand it yet, we’ll study each possible path individually.

![image-20200220164825552](C:\Users\xk\Desktop\Interview preparation\imgs\image-20200220164825552.png)



## Efficient data transmission with TCP

In a transport protocol such as TCP that offers a byte stream, a practical issue that was left as an implementation choice in [RFC 793](https://tools.ietf.org/html/rfc793.html) was *when* a new TCP segment should be sent.

There are two simple and extreme implementation choices:

1. Send a TCP segment as soon as the application has requested the transmission of some data.
   - **Advantage**: This allows TCP to provide a **low delay service**.
   - **Disadvantage**: If the application is writing data one byte at a time, TCP would place each byte in a segment containing 20 bytes of the TCP header. This is a **huge overhead** that is not acceptable in wide area networks.
2. Transmit a new TCP segment once the application has produced MSS (Maximum size) bytes of data. 
   - **Advantage:** **Reduced overhead**
   - **Disadvantage:** Potentially at the cost of a **very high delay**, which may be unacceptable for interactive applications.

### Nagle’s Algorithm [#](https://www.educative.io/courses/grokking-computer-networking/q2Qk8QW43P2#nagles-algorithm)

An elegant solution to this problem was proposed by John Nagle in [RFC 896](http://tools.ietf.org/html/rfc896.html) called **Nagle’s Algorithm**.

In essence, as long as there are unacknowledged packets, Nagle’s algorithm keeps collecting application layer data up to maximum segment size, to be sent together in a single packet. This helps reduce the packetization overhead by reducing small packets.

**Here’s how it works:** 

it sends data if it is at least the size of one MSS and the window size is appropriate. Otherwise, it checks if any unacknowledged segments exist. If so, it **buffers the data** and doesn’t send it. There is no timer on this condition, and it will keep buffering data until previous segments are acknowledged. If an *ACK* segment comes in, it sends the data.

Nagle’s has a few limitations:

1. Nagle’s algorithm is only supported by TCP and no other protocols like UDP.
2. TCP applications that require **low latency** and **fast response times** such as internet phone calls or real-time online video games, do not work well when Nagle’s is enabled. The delay caused by the algorithm triggers a noticeable lag. These applications usually *disable* Nagle’s with an interface called the `TCP_NODELAY` option.
3. The algorithm was originally developed at a time when computer networks supported much less bandwidth than they do today. It saved bandwidth and made a lot of sense at the time, however, the algorithm is much less frequently used today.
4. The algorithm also works poorly with **delayed ACKS**, a TCP feature that is used now. With both algorithms enabled, applications experience a consistent delay because Nagle’s algorithm doesn’t send data until an ACK is received and delayed ACKs feature doesn’t send an ACK until after a certain delay.

## TCP Window Scaling

**Problem: Small Windows Result in Inefficient Use of Bandwidth**

### Round Trip Time vs. Bandwidth vs. Throughput 

- **Round Trip Time** is the amount of time it takes to send a packet and receive its acknowledgment.
- **Bandwidth** is the rate at which the network can transport the bits.
- **Throughput** is the amount of data that is *actually* transferred from one end-system to another.

### Solution: Larger Windows [#](https://www.educative.io/courses/grokking-computer-networking/3jW4n0WMNqx#solution-larger-windows)

To solve this problem, a backward-compatible extension that allows TCP to use larger receive windows was proposed in [RFC 1323](http://tools.ietf.org/html/rfc1323.html).

**Basic idea:** instead of storing the size of the sending window and receiving window as 16-bit integers in the TCB, we keep the 16-bit window size, but introduce a multiplicative scaling factor.

#### Scaling Factor [#](https://www.educative.io/courses/grokking-computer-networking/3jW4n0WMNqx#scaling-factor)

As the TCP segment header only contains 16 bits to place the window field, it is impossible to copy the size of the sending window in each sent TCP segment. Instead, the header contains:

`Sending window << S`

where S is the scaling factor (0≤S≤14) negotiated during connection establishment and is specified in the header options field. ‘<<<<’ is the bitwise shift operator. This operation pads zeros at the right and discards the bits on the left essentially multiplying by 22 for each shift.

### Deciding a Scaling Factor [#](https://www.educative.io/courses/grokking-computer-networking/3jW4n0WMNqx#deciding-a-scaling-factor)

The client adds its proposed scaling factor as a TCP option in the SYN segment. If the server supports scaling windows, it sends the scaling factor in the *SYN+ACK* segment when advertising its own receive window. The local and remote scaling factors are included in the TCB. If the server does not support scaling windows, it ignores the received option and no scaling is applied. So scaling only applies when both parties support it.

Note that the protection mechanism of not maintaining state from the *SYN* packet via *SYN* cookies has the disadvantage that **the server wouldn’t remember the proposed scaling factor**.



## TCP Congestion Control

### **Slow Start**

The objective of TCP slow-start is to quickly reach an acceptable value for the congestion window.

During slow-start:

1. The congestion window is **doubled every round-trip time**.
2. The slow-start algorithm uses an additional variable in the TCB to maintain the **slow-start threshold**
   - The slow-start threshold is an estimation of the last value of the congestion window that did *not* cause congestion.
   - It is initialized at the sending window and is updated after each congestion event.



The TCP congestion control scheme distinguishes between two types of congestion:

1. **Severe Congestion**
2. **Mild Congestion**

### Severe Congestion [#](https://www.educative.io/courses/grokking-computer-networking/7DV98Mnpzg1#severe-congestion)

TCP considers that the network is severely congested when its retransmission timer expires. The following process is followed accordingly:

1. The sender performs slow-start until the first segments are lost and the retransmission timer expires.
2. At this time, TCP retransmits the first segment and the slow start threshold is set to half of the current congestion window. Then the congestion window is reset at one segment.
3. The lost segments are retransmitted as the sender again performs slow-start until the congestion window reaches the slow start threshold.
4. It then switches to congestion avoidance and the congestion window increases linearly until segments are lost and the retransmission timer expires.

### Mild Congestion [#](https://www.educative.io/courses/grokking-computer-networking/7DV98Mnpzg1#mild-congestion)

TCP considers that the network is lightly congested if it receives three duplicate acknowledgments.

1. The sender begins with a slow-start
2. If 3 duplicate ACKs arrive, the sender performs a **fast retransmit** (retransmits without waiting for the retransmission timer to expire).
   - Have a look at the following slides to see when 3 duplicate acknowledgments could arrive and when a fast retransmit happens.
3. If the fast retransmit is successful, this implies that only one segment has been lost.
   - In this case, TCP performs multiplicative decrease and the congestion window is divided by 22.
   - The slow-start threshold is set to the new value of the congestion window.
4. The sender immediately enters congestion avoidance as this was mild congestion.



