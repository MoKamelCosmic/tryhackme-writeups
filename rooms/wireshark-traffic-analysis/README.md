# Wireshark Traffic Analysis — TryHackMe Room

A full walkthrough of the Wireshark: Traffic Analysis room from TryHackMe.  
This write-up focuses on investigation methodology rather than simply providing answers.

Link to Full Medium Article: https://medium.com/@mohamedfathykamel/wireshark-traffic-analysis-a-complete-tryhackme-walkthrough-11fd4cf82eb5

---

## Task 2 — Detecting Nmap Scans

### Objective
Identify different types of Nmap scans inside the capture file.

### Approach
Used TCP flags, handshake behavior, and ICMP responses to distinguish:
- TCP Connect scans  
- SYN scans  
- UDP scan behavior  

### Results
- Total TCP Connect scans: 1000  
- Scan type used on port 80: TCP Connect  
- UDP closed-port responses: 1083  
- Open UDP port (range 55–70): 68  

---

## Task 3 — ARP Poisoning and MITM

### Objective
Determine whether ARP spoofing, ARP flooding, or MITM activity is occurring.

### Approach
Reviewed duplicate ARP announcements, MAC conflicts, and HTTP packet flows.

### Results
- ARP requests by the attacker: 284  
- HTTP packets received by attacker: 90  
- Sniffed credential entries: 6  
- Password for Client986: `clientnothere!`  
- Comment from Client354: `Nice Work!`  

---

## Task 4 — Host Identification (DHCP, NBNS, Kerberos)

### Objective
Identify hosts, MAC addresses, and user identities using DHCP, NBNS, and Kerberos.

### Results
- MAC of Galaxy A30: 9a:81:41:cb:96:6c  
- NetBIOS registration requests by LIVALJM: 16  
- Host requesting IP 172.16.13.85: Galaxy-A12  
- IP of user u5 (defanged): 10[.]1[.]12[.]2  
- Kerberos hostname: xp1$  

---

## Task 5 — Tunneling Traffic (DNS and ICMP)

### Objective
Identify covert tunneling through ICMP and DNS.

### Results
- Protocol tunneled inside ICMP: SSH  
- Suspicious DNS domain (defanged): dataexfil[.]com  

---

## Task 6 — Cleartext Protocol Analysis: FTP

### Objective
Investigate FTP authentication attempts, file transfers, and permission modifications.

### Results
- Incorrect login attempts: 737  
- Size of file accessed: 39424  
- File accessed: resume.doc  
- Permission modification command: CHMOD 777  

---

## Task 7 — Cleartext Protocol Analysis: HTTP

### Results
- Number of anomalous user-agent types: 6  
- Subtle user-agent misspelling packet: 52  
- Log4j attack starting packet: 444  
- Callback IP (defanged): 62[.]210[.]130[.]250  

---

## Task 8 — Encrypted Protocol Analysis (Decrypting HTTPS)

### Results
- Frame number of Client Hello to accounts.google.com: 16  
- Number of HTTP/2 packets: 115  
- Authority header (frame 322): safebrowsing[.]googleapis[.]com  
- Flag: FLAG{THM-PACKETMASTER}  

---

## Task 9 — Hunting Cleartext Credentials

### Results
- HTTP Basic Auth packet: 237  
- Packet with empty password: 170  

---

## Task 10 — Actionable Results (Firewall Rules)

### Results
- Deny source IPv4 (ipfw):  
  `add deny ip from 10.121.70.151 to any in`

- Allow destination MAC (ipfw):  
  `add allow MAC 00:d0:59:aa:af:80 any in`

---

## Key Takeaways

- Packet behavior often reveals attacker intent more clearly than signatures.  
- Combining ARP and HTTP traffic exposes MITM attacks effectively.  
- DNS tunneling is identifiable by examining query structure and length.  
- HTTPS traffic is not opaque if TLS key material is available.  
- Wireshark can rapidly convert findings into actionable firewall rules.

---

This Markdown version intentionally excludes screenshots to keep the repository lightweight.  
A visual walkthrough with images is available in the Medium version of this write-up.
