# 🔐 Wireshark SOC Investigations
### NetworkWalks Cybersecurity Programme | Week 1 | Project PM4

## 📌 Project Overview

This project documents six hands-on Security Operations Centre (SOC) investigations completed as part of the NetworkWalks Cybersecurity Programme.

Working with simulated network traffic in Wireshark, I investigated suspicious activity, reconstructed communications, identified security vulnerabilities and examined how sensitive information can be exposed through unencrypted protocols.

The exercises provided practical experience in analysing network packets, interpreting protocols and recognising indicators of compromise.

## 🎯 Project Objectives

- Analyse network traffic using Wireshark.
- Identify attackers and targeted systems.
- Investigate TCP port-scanning activity.
- Reconstruct unencrypted TCP conversations.
- Identify sensitive information exposed through HTTP.
- Examine plaintext authentication credentials.
- Detect FTP brute-force attacks.
- Investigate VoIP communications using SIP and RTP.
- Recommend appropriate security controls.

## 🛠️ Tools and Technologies

| Tool / Protocol | Purpose |
|-----------------|---------|
| Wireshark | Network packet analysis |
| TCP/IP | Network communication analysis |
| HTTP | Web traffic investigation |
| FTP | Authentication attack investigation |
| SIP | VoIP call signalling |
| RTP | Voice traffic analysis |
| ICMP | Network diagnostics |
| Wireshark Display Filters | Isolating relevant packets |

---

# 🕵️ SOC Investigation Reports

## Lab 3: Identifying a TCP Port Scan

### Objective

Investigate suspicious network activity to identify an attacker scanning a server for open TCP ports.

### Investigation

I examined TCP SYN packets and analysed the server's responses to determine which ports were open or closed.

A TCP SYN scan can be identified when one host attempts connections to multiple destination ports within a short period.

### Wireshark Filters

Display SYN packets:

`tcp.flags.syn == 1`

Identify open-port responses:

`ip.src == 192.168.10.20 && tcp.flags.syn == 1 && tcp.flags.ack == 1`

### Findings

| Indicator | Result |
|-----------|--------|
| Attacker IP | 192.168.10.66 |
| Target IP | 192.168.10.20 |
| Attacker source port | 52311 |
| Ports scanned | 40 |
| Open ports | 22, 80, 443 |
| Closed ports | 37 |
| Attack type | TCP port scan |

The server responded with SYN-ACK packets for open ports and RST-ACK packets for closed ports.

### Security Recommendations

- Restrict unnecessary open ports.
- Apply appropriate firewall rules.
- Monitor unusual connection patterns.
- Use intrusion detection systems to identify reconnaissance activity.

### What I Learned

I learned how TCP flags reveal port states and how repeated connection attempts across multiple destination ports can indicate network reconnaissance.

---

## Lab 4: Investigating Plaintext TCP Conversations

### Objective

Identify and reconstruct an office chat conversation transmitted without encryption.

### Investigation

I excluded common web traffic to locate unusual TCP communications.

Wireshark filter:

`tcp && !(tcp.port == 80 || tcp.port == 443)`

I identified traffic using TCP port 5000 and used Follow TCP Stream to reconstruct the conversation.

### Findings

| Indicator | Result |
|-----------|--------|
| Chat server port | 5000 |
| Client IP | 192.168.10.45 |
| TCP stream | 5 |
| Conversation size | 413 bytes |
| Security issue | Unencrypted communication |

The conversation exposed information exchanged between staff members, demonstrating how plaintext communication can reveal confidential information.

### Security Recommendations

- Replace plaintext chat tools with encrypted alternatives.
- Use TLS to protect communications.
- Avoid exchanging passwords or sensitive information through insecure applications.

### What I Learned

I learned how to identify unusual application traffic and use Follow TCP Stream to reconstruct conversations from captured packets.

---

## Lab 5: Finding Hidden Information in HTTP Traffic

### Objective

Investigate HTTP traffic to identify information hidden within a web page's source code.

### Investigation

I examined HTTP requests and responses using:

`http.request`

I located a request for `/staff.html` and followed its TCP stream to inspect the server response.

### Findings

| Indicator | Result |
|-----------|--------|
| Website | intranet.company.local |
| Affected page | /staff.html |
| HTTP response | 200 OK |
| Hidden information | HTML comment |
| Direction | Server to client |

The page contained an HTML comment with internal information that would not normally appear in the rendered browser page.

Although hidden from the visible page, the information remained accessible in the HTTP response.

### Security Recommendations

- Remove confidential information from HTML comments.
- Review application source code before deployment.
- Avoid exposing internal infrastructure details.
- Implement secure development and code-review practices.

### What I Learned

I learned that information hidden from the browser's visual display is not necessarily protected from users or network analysts.

---

## Lab 6: Investigating Plaintext HTTP Credentials

### Objective

Determine whether login credentials could be intercepted from an unencrypted web application.

### Investigation

I examined HTTP traffic associated with a staff portal login.

Wireshark filter:

`http`

I located the authentication request and inspected the submitted form data.

### Findings

| Indicator | Result |
|-----------|--------|
| Protocol | HTTP |
| HTTP method | POST |
| Login endpoint | /login |
| Content type | application/x-www-form-urlencoded |
| Security issue | Credentials transmitted in plaintext |

The authentication request contained readable form fields, demonstrating the risks of transmitting login credentials without encryption.

### Security Recommendations

- Enforce HTTPS across the application.
- Redirect HTTP traffic to HTTPS.
- Configure secure TLS settings.
- Avoid transmitting credentials over unencrypted connections.

### What I Learned

I learned how HTTP POST requests carry form data and why HTTPS is essential for protecting credentials during transmission.

---

## Lab 7: Detecting an FTP Brute-Force Attack

### Objective

Investigate repeated authentication failures against an FTP server.

### Investigation

I filtered FTP traffic and examined the authentication sequence.

Wireshark filter:

`ftp`

I analysed repeated USER and PASS commands alongside server response codes.

### Findings

| Indicator | Result |
|-----------|--------|
| Attacker IP | 192.168.10.66 |
| FTP server IP | 192.168.10.20 |
| Targeted username | admin |
| Failed authentication code | 530 |
| Successful authentication code | 230 |
| Attack type | FTP brute-force |

The capture showed repeated unsuccessful authentication attempts followed by a successful login.

### Security Recommendations

- Disable unnecessary FTP services.
- Use encrypted alternatives such as SFTP.
- Implement account lockout or rate-limiting controls.
- Monitor repeated authentication failures.
- Apply strong authentication policies.

### What I Learned

I learned how FTP authentication commands and response codes can reveal brute-force activity and successful account compromise.

---

## Lab 8: Investigating VoIP Communications

### Objective

Investigate network traffic associated with a voice call and identify security risks arising from unencrypted communication.

### Investigation

I examined SIP signalling packets to identify call participants and analysed RTP traffic carrying the voice communication.

Wireshark filters:

`sip`

`rtp`

### Findings

| Indicator | Result |
|-----------|--------|
| Call setup protocol | SIP |
| Voice transport protocol | RTP |
| Caller | Ali Raza |
| Caller extension | 101 |
| Recipient | Sara |
| Recipient extension | 202 |
| Security issue | Unencrypted voice traffic |

SIP signalling revealed information about the call participants, while the RTP traffic demonstrated the risk of voice interception.

### Security Recommendations

- Encrypt voice communication using SRTP.
- Protect call signalling with TLS where supported.
- Restrict access to VoIP infrastructure.
- Monitor suspicious calling activity.

### What I Learned

I learned the difference between SIP signalling and RTP media transport, as well as how unencrypted VoIP communications can expose sensitive conversations.

---

# 💡 Key Learning Outcomes

Completing these six investigations strengthened my understanding of:

1. TCP connection establishment and flag analysis.
2. Recognising network reconnaissance and port scans.
3. Reconstructing application conversations using TCP streams.
4. Identifying information disclosure through HTTP.
5. Analysing authentication-related network activity.
6. Detecting brute-force attacks from protocol responses.
7. Investigating VoIP signalling and media traffic.
8. Recommending practical security mitigations.
9. Documenting technical findings clearly and systematically.

## 🔒 Security and Ethical Considerations

All investigations were completed within authorised, simulated NetworkWalks cybersecurity training environments.

The IP addresses and investigation findings relate to the training scenarios.

Sensitive credentials discovered during the exercises have intentionally been omitted from this public report.

## 📚 Project Information

**Programme:** NetworkWalks Cybersecurity Programme

**Batch:** B084

**Week:** 1

**Project:** PM4 – Wireshark SOC Investigations (Extra)

**Total Labs Completed:** 6

**Total Tasks:** 60
