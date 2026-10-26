# Protection and Security in Operating Systems

## 1. Introduction

Operating-system protection and security mechanisms help ensure that system resources are used correctly and that only authorized users or processes can access them.

A computer contains many resources and objects, including:
- Hardware such as disks, monitors, keyboards, and printers
- Software and applications
- Files, memory regions, and other system resources

Multiple processes may exist in memory at the same time. Protection mechanisms help prevent one process from improperly accessing or interfering with another process's resources.

## 2. Protection Model

A computer system can be viewed as a collection of **objects**. Each object has an identity and a defined set of operations that can be performed on it.

The operating system acts as a resource allocator and enforces access rules. Its protection goal is to ensure that:
- Objects are accessed correctly.
- Only processes with the required permissions can access an object.
- Processes cannot perform operations beyond the permissions they have been granted.

Examples of objects include files, disk drives, printers, and memory regions. Typical operations include reading, writing, executing, or printing.

## 3. The CIA Triad

A central security model is the **CIA triad**: Confidentiality, Integrity, and Availability.

### 3.1 Confidentiality

Confidentiality means preventing unauthorized people or processes from reading information.

A common method for protecting confidentiality is **encryption**:
- Plaintext is transformed into ciphertext.
- A party with the appropriate key can decrypt the ciphertext to recover the plaintext.

Encryption helps protect data when it is stored or transmitted. It does not guarantee that data can never be stolen or exposed; key management and other security controls also matter.

### 3.2 Integrity

Integrity means protecting information against unauthorized modification or destruction.

A **breach of integrity** occurs when data is changed without authorization. Access controls and integrity-checking mechanisms can help detect or prevent such changes.

### 3.3 Availability

Availability means ensuring that authorized users can access systems and data when needed.

A **denial-of-service (DoS)** attack attempts to make a service unavailable. A distributed denial-of-service (DDoS) attack uses multiple sources to overwhelm or disrupt a target.

The lecture mentions Ping of Death and SYN flooding as examples associated with denial of service. These are attack concepts; the exact techniques and their effectiveness depend on the systems and protections involved.

## 4. Other Security Terms

### 4.1 Theft of service

Theft of service is unauthorized use of computing resources, such as processing time, storage, network access, or paid services.

### 4.2 Denial of service

A denial-of-service attack prevents or degrades legitimate users' access to a service. For example, a server may become unable to handle legitimate requests if it is overwhelmed by traffic or resource-consuming requests.

## 5. Principle of Least Privilege

The **principle of least privilege** states that users and processes should receive only the permissions necessary to perform their tasks.

Privileges can differ by role. For example, a student, teacher, department head, and system administrator may require different permissions.

Least privilege limits the potential damage caused by mistakes or compromised accounts. Privileges may be:
- **Static:** granted for a longer period or for the lifetime of a role/process.
- **Temporary:** granted for a particular operation and revoked when no longer needed.

The lecture emphasizes that privileges should not automatically be identical for every user.

## 6. Protection Domains and Access Rights

A **protection domain** is a set of access rights that specifies which operations may be performed on which objects.

An access right can be represented as a pair:

\[
(\text{object}, \text{rights})
\]

Examples of rights include:
- Read
- Write
- Execute
- Print

For example, a domain may allow reading `file1`, reading and writing `file2`, and printing to a particular printer, while denying other operations.

The lecture sometimes uses “user,” “subject,” and “domain” informally in the same context. More precisely, a **subject** is an active entity requesting access, while a **domain** is the set of rights available to it at a particular time.

## 7. Access Matrix

An **access matrix** represents which domains or subjects can perform which operations on which objects.

- **Rows:** domains or subjects
- **Columns:** objects
- **Cells:** permitted operations

Example:

| Domain / Subject | File 1 | File 2 | Printer |
|---|---|---|---|
| D1 | Read | Read, Write | Print |
| D2 | Read, Write | — | — |
| D3 | — | Read | Print |

A dash indicates that no access right is listed in this example.

The operating system can consult the access-control model to determine whether an operation should be permitted. A full matrix can become large when there are many subjects and objects.

### 7.1 Alternative representations

The lecture mentions two alternatives for representing access rights:

- **Access-control table:** stores entries describing a subject, object, and permitted rights.
- **Capability model:** associates a subject or domain with capabilities that specify which objects it may access and what operations it may perform.

These are alternative ways to represent or implement access rights; their exact design depends on the operating system.

## 8. Threats and Attacks

### 8.1 Threat

A **threat** is a potential cause of a security violation.

### 8.2 Attack

An **attack** is an attempt to violate security—for example, to gain unauthorized access, modify information, or disrupt a service. An attack may succeed or fail.

## 9. Active and Passive Attacks

The lecture distinguishes active attacks from passive attacks.

- **Active attack:** attempts to alter system resources or affect system operation, such as modifying messages or disrupting a service.
- **Passive attack:** observes or monitors information without directly changing the system, such as eavesdropping.

Examples mentioned in the lecture include masquerading, replay, man-in-the-middle activity, and session hijacking. These terms describe different attack patterns and should not be treated as exact synonyms.

### 9.1 Masquerading

Masquerading occurs when an attacker pretends to be a legitimate user or system, for example by using stolen credentials or a forged identity.

### 9.2 Replay attack

A replay attack involves capturing a valid message or exchange and retransmitting it later in an attempt to gain an unauthorized result.

### 9.3 Man-in-the-middle attack

In a man-in-the-middle attack, an attacker positions themselves between two communicating parties and may observe or alter their communication.

### 9.4 Session hijacking

Session hijacking occurs when an attacker takes control of an authenticated session, potentially allowing actions to be performed as the legitimate user. Session expiration and secure session-token handling help reduce this risk.

## 10. Security Levels and Social Engineering

The lecture groups security concerns into several broad areas:

1. **Physical security:** protecting data centres, servers, devices, and facilities from unauthorized physical access.
2. **Human security:** reducing risks caused by human error or manipulation.
3. **Operating-system security:** protecting processes, files, accounts, and system resources.
4. **Network security:** protecting communications, network services, and connected systems.

**Social engineering** uses deception or manipulation to persuade people to disclose information or perform unsafe actions.

**Phishing** is a common social-engineering technique in which a message or website impersonates a trusted source to trick a person into revealing information or opening a malicious link or attachment.

## 11. Malware and Related Threats

### 11.1 Trojan horse

A **Trojan horse** is malicious software disguised as a legitimate or useful application.

- It relies on deception to persuade a user to run or install it.
- It may perform harmful actions or create unauthorized access.
- Unlike a self-replicating virus or worm, a Trojan horse is not defined by self-replication.

### 11.2 Virus

A **computer virus** attaches itself to a host file or program and can spread when the infected host is executed or otherwise activated. It may damage data or interfere with system operation.

### 11.3 Worm

A **worm** is malware that can replicate and spread between systems without requiring a user to manually transfer an infected host file each time. Worms may exploit network services or other mechanisms to propagate.

**Key distinction:** A virus typically depends on a host file or program; a worm is capable of self-propagation. The lecture informally describes a worm as related to a virus, but the terms refer to distinct malware categories.

## 12. Port Scanning, Spoofing, and Network Attacks

The lecture briefly mentions:
- **Port scanning:** checking network ports to identify accessible services.
- **Spoofing:** falsifying an identity or address, such as an IP address, to mislead a system or recipient.
- **SYN flooding:** sending many TCP connection-initiation requests in an attempt to exhaust server resources.
- **Ping of Death:** a historically known attack involving malformed or oversized IP packets against vulnerable systems.

These are introductory definitions only; the lecture does not develop their technical details.

## 13. Cryptography: Basic Terms

Cryptography uses techniques to protect information.

- **Plaintext:** readable original information.
- **Ciphertext:** information transformed by encryption.
- **Encryption:** converting plaintext into ciphertext.
- **Decryption:** converting ciphertext back into plaintext using the appropriate key or mechanism.

The lecture distinguishes two broad key models:

| Type | Key arrangement |
|---|---|
| Symmetric cryptography | Uses the same secret key for encryption and decryption. |
| Asymmetric cryptography | Uses a related public/private key pair. |

The lecture leaves detailed cryptographic algorithms and calculations to the computer-networks topic.

## 14. Firewalls

A **firewall** applies configured rules to network traffic to allow or block communications.

The lecture compares a firewall to a guard at an entrance who checks whether someone meets the rules before allowing entry. In practice, firewall rules can consider factors such as source, destination, port, protocol, and connection state.

A firewall is one layer of security; it does not replace authentication, patching, endpoint protection, or other controls.

## 15. Authentication

**Authentication** is the process of verifying an identity. A username and password are common authentication credentials, though systems may use additional methods such as security keys or multi-factor authentication.

Authentication answers, “Who or what is making this request?” Authorization determines what the authenticated identity is allowed to do.

The lecture mentions Kerberos in connection with Windows security. Kerberos is a network authentication protocol that uses tickets to support authentication; it should not be interpreted as a mechanism that simply stores all passwords and data in one place.

## 16. Quick Revision Table

| Term | Core meaning |
|---|---|
| Confidentiality | Prevent unauthorized reading of information |
| Integrity | Prevent or detect unauthorized changes |
| Availability | Keep systems and data accessible to authorized users |
| Encryption | Convert plaintext into ciphertext |
| Theft of service | Unauthorized use of computing resources |
| Least privilege | Grant only the permissions required |
| Protection domain | Set of access rights available in a context |
| Access matrix | Represents rights of subjects/domains over objects |
| Threat | Potential cause of a security violation |
| Attack | Attempt to violate security |
| Masquerading | Pretending to be another identity |
| Replay attack | Reusing a captured valid message or exchange |
| Man-in-the-middle | Intercepting communication between parties |
| Session hijacking | Taking control of an authenticated session |
| Trojan horse | Malicious software disguised as legitimate |
| Virus | Malware that attaches to a host file or program |
| Worm | Malware capable of self-propagation |
| Firewall | Filters traffic according to security rules |
| Authentication | Verifies identity |
| Authorization | Determines permitted actions |

## 17. Exam-Focused Points

- Memorize the **CIA triad**: Confidentiality, Integrity, Availability.
- Encryption is a key technique for confidentiality.
- Unauthorized data modification is a breach of integrity.
- DoS/DDoS attacks threaten availability.
- Least privilege means giving only the permissions needed.
- A protection domain is a set of access rights.
- In an access matrix, rows represent domains/subjects, columns represent objects, and cells list rights.
- Distinguish a Trojan horse, virus, and worm.
- Distinguish authentication (identity verification) from authorization (permission checking).
- A firewall filters traffic according to rules.
- The lecture presents this topic as primarily theoretical and introductory, with more detailed network-security treatment covered elsewhere.
