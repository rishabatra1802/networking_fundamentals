# 🛰️ Protocols

Network protocols are the set of standardized rules that define how data is transmitted, routed, and received over a network. They ensure smooth communication between devices and are essential for everything from browsing websites to sending emails. Below is a breakdown of commonly used protocols along with their working, ports, methods, and security implications.

---

## 📧 1. SMTP – Simple Mail Transfer Protocol

**Purpose:** Sending Emails

### What is SMTP?

SMTP is the protocol responsible for sending email messages from a client (like Gmail) to a mail server or from one mail server to another. Think of it like the digital equivalent of a postman.

### Working:

- Operates on **Port 25** (standard), **Port 587** (for secured transmission with STARTTLS), and **Port 465** (SMTPS - SMTP over SSL/TLS).
- Uses **Push-based communication** — the sender initiates communication.
- Key SMTP commands:
  - `HELO` / `EHLO`: Identify the sending server
  - `MAIL FROM`: Sender’s address
  - `RCPT TO`: Recipient’s address
  - `DATA`: Begin the body of the email

### Real-Time Example:
When you send an email through Gmail, SMTP handles the outbound delivery behind the scenes.

### Security:
- **SMTPS** (Port 465): Ensures end-to-end encryption using SSL/TLS.
- **ESMTP** (Extended SMTP): Adds authentication and attachment support.
- If the receiving server is unavailable, SMTP retries based on a queue system with timeout and backoff strategies.

### Challenge:
If a mail server is down, how does SMTP respond? — It queues the message and retries delivery after a time interval until it either succeeds or fails with a bounce message.

---

## 📂 2. FTP – File Transfer Protocol

**Purpose:** Uploading and Downloading Files

### What is FTP?

FTP allows you to transfer files over a network, similar to copying files using a USB drive—but over the internet.

### Working:

- Operates on **Port 21** for control and **Port 20** for data.
- Two modes:
  - **Active Mode**: Server initiates data connection back to client.
  - **Passive Mode**: Client initiates both control and data connections (used behind firewalls).

### Real-Time Example:
When you upload images to a web server using **FileZilla**, FTP transfers files securely in the background.

### Security:
- **FTPS** (FTP Secure): Adds SSL/TLS for encrypted transmission.
- **SFTP** (SSH File Transfer Protocol): Not related to FTP; uses **SSH on Port 22** for secure file transfer.
- For sensitive environments, SFTP is preferred.

### Challenge:
How to speed up file transfers in low-bandwidth? — Use compression, increase TCP buffer size, and prefer SFTP over plain FTP for security.

---

## 🌐 3. HTTP – HyperText Transfer Protocol

**Purpose:** Web Browsing and Web App Communication

### What is HTTP?

HTTP is the foundation of any data exchange on the Web. It's the protocol that loads web pages in your browser.

### Working:

- Works over **Port 80**
- Stateless protocol – each HTTP request is independent.
- Common HTTP Methods:
  - `GET`: Retrieve data (like webpages)
  - `POST`: Submit data (like login forms)
  - `PUT`: Update resources
  - `DELETE`: Remove resources

### Real-Time Example:
When you browse Google or watch YouTube, the content is fetched using HTTP.

### Security:
- By default, HTTP does **not encrypt** traffic. Vulnerable to:
  - Man-in-the-middle (MITM) attacks
  - Session hijacking
  - Cookie theft

---

## 🔐 4. HTTPS – HTTP Secure

**Purpose:** Secure Web Browsing

### What is HTTPS?

HTTPS is HTTP combined with **SSL/TLS** encryption, ensuring privacy, data integrity, and authenticity between client and server.

### Working:

- Operates over **Port 443**
- Uses **SSL/TLS certificates** to establish secure connections
- During handshake:
  - Server shares certificate signed by Certificate Authority (CA)
  - Key exchange happens for symmetric encryption
- Ensures encrypted data transmission (TLS 1.2 or TLS 1.3)

### Real-Time Example:
Banking apps, online shopping, and login forms all use HTTPS to encrypt sensitive information.

### Security:
- TLS 1.3 provides:
  - **Forward secrecy**
  - **0-RTT** for faster reconnections
- Common providers: DigiCert, Let’s Encrypt
- Tools like `openssl` or browser dev tools can inspect HTTPS certs

### Challenge:
If HTTPS certificate is expired or invalid, browsers will block the site. Solution: Renew certificate and configure expiration monitoring.

---

## 🌍 5. DNS – Domain Name System

**Purpose:** Converts domain names into IP addresses

### What is DNS?

DNS works like the phonebook of the internet. It resolves human-readable domain names like `www.google.com` to IP addresses like `142.250.190.14`.

### Working:

- Operates on **Port 53**
- Uses both **UDP** (faster, default) and **TCP** (for large responses)
- Resolution Process:
  - User sends query to **Resolver**
  - Resolver checks local cache → Root Server → TLD → Authoritative Server
  - Returns IP address to client

### Real-Time Example:
When you open Facebook, DNS translates the name to an IP, allowing your browser to connect.

### Security:
- **DNSSEC**: Adds data integrity and authentication via digital signatures.
- **DoH (DNS over HTTPS)** and **DoT (DNS over TLS)** encrypt DNS queries to prevent snooping.

### Challenge:
How to protect DNS against DDoS? — Use **Anycast DNS**, **Rate Limiting**, and **CDNs (Cloudflare, Akamai)**.

---

## 📨 6. POP3 and IMAP – Email Retrieval Protocols

**Purpose:** Retrieving Emails from Mail Server

### What is POP3?

**POP3 (Post Office Protocol v3)** is used to retrieve emails from the server and download them locally. Works best for single-device use.

- Operates on **Port 110** (plaintext) or **Port 995** (SSL)
- Once downloaded, emails are typically deleted from the server.

### What is IMAP?

**IMAP (Internet Message Access Protocol)** allows access to emails stored on a mail server and syncs across multiple devices.

- Operates on **Port 143** (plaintext) or **Port 993** (SSL)
- Emails remain on the server; changes reflect across devices

### Real-Time Example:
Using Outlook or Gmail app on phone or desktop, IMAP ensures all emails are synced.

### Security:
- Use **SSL/TLS** encryption (IMAPS or POP3S)
- IMAP IDLE allows real-time mail notifications

### Challenge:
To optimize IMAP on mobile networks, use **selective sync** and restrict folder subscriptions to save bandwidth.

---

## 📊 Protocol Comparison Table

| Protocol      | Purpose                | Port(s)        | Secure Version | Example Use Case                  |
|---------------|------------------------|----------------|----------------|-----------------------------------|
| SMTP          | Email Sending          | 25, 587, 465   | SMTPS          | Gmail email delivery              |
| FTP           | File Transfer          | 21 (Ctrl), 20 (Data) | FTPS/SFTP     | Uploading website files           |
| HTTP          | Web Browsing           | 80             | HTTPS (443)    | Loading web pages                 |
| HTTPS         | Secure Web Access      | 443            | TLS/SSL        | Online banking, shopping          |
| DNS           | Domain Resolution      | 53 (UDP/TCP)   | DNSSEC, DoH    | Resolving `google.com` to IP      |
| POP3          | Email Retrieval (Local)| 110, 995       | POP3S          | Downloading emails to one device  |
| IMAP          | Email Sync             | 143, 993       | IMAPS          | Accessing mail on multiple devices|

---

## 🔁 Data Flow Visualization

```mermaid
graph LR
    Client -->|Request| Server
    Server -->|Response| Client
    Client --> SMTP --> MailServer
    FileTransfer --> FTP --> WebServer
    Client --> DNS --> DNSResolver
    Client --> HTTP/HTTPS --> WebServer
    Client --> POP3/IMAP --> MailServer
