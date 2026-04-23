# LianYu TryHackMe 
LianYu is a beginner level security challenge. This is Arrowverse themed beginner CTF box.

Room Link: https://tryhackme.com/room/lianyu

## 🎯 Main Objective

👉 Compromise the target machine and gain root access

That means:

- Break into the system
- Escalate privileges
- Capture flags (proof of access)

## Step 1: Scanning the IP
```
nmap 10.49.165.124
```
Findings: 
- Port 21 = FTP
- Port 22 = SSH
- Port 80 = HTTP
- Port 111 = rpcbind

<img width="905" height="383" alt="Screenshot 2026-04-23 131647" src="https://github.com/user-attachments/assets/0c3bff36-d94a-4a59-b012-19d5dbae5e98" />
