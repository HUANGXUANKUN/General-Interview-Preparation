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