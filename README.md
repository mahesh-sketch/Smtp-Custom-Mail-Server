# 📬 Custom Mail Server

## 1. What is a Mail Server? 📧

A mail server is a computerized system that sends, receives, and stores emails. It follows the Simple Mail Transfer Protocol (SMTP) for sending emails and either Post Office Protocol 3 (POP3) or Internet Message Access Protocol (IMAP) for receiving and storing emails. Mail servers play a crucial role in facilitating email communication across the internet.

## 2. How Sending and Receiving Works? 🚀

### Receiving an Email 📥

1. **Sender's SMTP Server:** The sender's SMTP server initiates the communication by connecting to the recipient's mail server.

2. **Receiving SMTP Server:** The recipient's SMTP server accepts the connection, verifies the sender's authenticity, and stores the email in the recipient's mailbox.

3. **Email Client Access:** The recipient uses an email client (e.g., Outlook, Gmail) to connect to the receiving server and fetch the email.

### Sending an Email 📤

1. **Sender's Email Client:** The sender uses an email client to compose an email.

2. **Sender's SMTP Server:** The sender's SMTP server is responsible for sending the email. It connects to the recipient's SMTP server using the recipient's domain's MX records.

3. **Receiving SMTP Server:** The recipient's SMTP server accepts the connection, verifies the sender's authenticity, and stores the email in the recipient's mailbox.

## 3. DNS Records Explained 🌐

### MX - Mail Exchange 📮

MX records specify the mail servers responsible for receiving emails on behalf of a domain. These records point to the server's hostname and priority.

Example MX record:

```plaintext
Priority: 10
Mail Server: mail.example.com
```

### SPF - Sender Policy Framework 🛡️

SPF records define which IP addresses are authorized to send emails on behalf of a domain. It helps prevent email spoofing.

Example SPF record:

```plaintext
v=spf1 ip4:192.168.0.1 include:_spf.example.com ~all
```

### DKIM - DomainKeys Identified Mail 🔐

DKIM adds a digital signature to outgoing emails, allowing the recipient to verify the email's authenticity.

Example DKIM record:
```plaintext
Selector: default
Public Key: "MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCx5oG0VLvJkG8bUxryl4AqHz2
            K+W/BoQIDAQAB"
```
### DMARC - Domain-based Message Authentication, Reporting, and Conformance 📊

DMARC is an email authentication and reporting protocol that builds on SPF and DKIM. It allows senders to specify policy and provides reporting on email authentication failures.

Example DMARC record:
```plaintext
v=DMARC1; p=quarantine; rua=mailto:dmarc@example.com; ruf=mailto:dmarc@example.com
```
**Possible Policy:**

**none:** Take no action.

**quarantine:** Place the email in the recipient's spam or quarantine folder.

**reject:** Reject the email outright.

## 4. Create our own SMTP Server 🛠️

**SMTP Communication 📨**

SMTP communication involves several steps:

### HELO / EHLO 👋

The client (sender's SMTP server) initiates the conversation by sending a greeting command (HELO or EHLO) to the recipient's SMTP server.

```plaintext
EHLO sender.example.com
```

### MAIL FROM ✉️

The sender's server identifies the sender's email address and announces the beginning of a new mail transaction.

```plaintext
MAIL FROM: <sender@example.com>
```

### RCPT TO 🎯

The sender's server identifies the recipient's email address and announces the recipient for the mail transaction.

```plaintext
RCPT TO: <recipient@example.com>
```

### DATA 📤
The client sends the actual email message after negotiating the recipient.

```plaintext
DATA
Subject: Example Subject
Hello, this is the email body.
```

These steps are a simplified overview of SMTP communication, and a complete implementation involves error handling, security measures, and compliance with SMTP standards.

## 5. Workflow: Authentication to DNS Lookup 🔄

## Sender Authentication:
 - The sender's server uses its private key to sign the outgoing email.
 - The recipient's server, using the sender's public key from DNS records, verifies the signature.

### DNS Lookup:

 - The recipient's server performs DNS lookups for the sender's domain.
 - MX records determine the recipient's mail server.
 - SPF, DKIM, and DMARC records provide authentication and policy information.

### Email Transmission:

 - The sender's SMTP server initiates communication with the recipient's SMTP server.
 - HELO/EHLO, MAIL FROM, RCPT TO, DATA commands are exchanged to negotiate and send the email.

### Cryptographic Verification:
- The recipient's server verifies the DKIM signature using the sender's public key.
- SPF records confirm that the sender's IP is authorized to send emails for the domain.
- DMARC policy defines actions to take based on authentication results.

### Email Delivery:
- The email is stored in the recipient's mailbox, ready for retrieval by the recipient's email client.


Feel free to use this as a starting point and customize it further based on your specific needs and preferences.
