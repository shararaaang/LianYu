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

## Step 2: Enumeration
Visit the target IP in browser 
```
http://10.49.165.124
```
<img width="901" height="769" alt="Screenshot 2026-04-23 131751" src="https://github.com/user-attachments/assets/25160691-244a-4611-a19b-66bce1d35609" />

Then, run gobuster for hidden directories.
```
gobuster dir -u http://10.49.165.124 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

<img width="897" height="539" alt="Screenshot 2026-04-23 131708" src="https://github.com/user-attachments/assets/801b3650-feb9-4640-88d3-b0353beba6bf" />

Findings:
Found a directory: /island

After found the hidden directory, go to browser and search http://10.49.165.124/island/

<img width="900" height="773" alt="Screenshot 2026-04-23 131810" src="https://github.com/user-attachments/assets/c154c667-ae01-454d-b5df-73b432d78a34" />

<img width="902" height="780" alt="Screenshot 2026-04-23 131826" src="https://github.com/user-attachments/assets/3016d5af-4dcc-43fa-b0f6-2cdc93a69074" />

<img width="898" height="780" alt="Screenshot 2026-04-23 131916" src="https://github.com/user-attachments/assets/40aa7774-4309-4fa2-96b3-a92681793cc7" />

Found out the Code Word by highlighting the page text or viewing the page source.
Code Word -  'vigilante' - (this is our FTP username)

Again run gobuster on /island directory to discover a different directory.
```
gobuster dir -u http://10.49.165.124/island -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```
Here we found another directory : /2100 

<img width="942" height="536" alt="Screenshot 2026-04-23 132104" src="https://github.com/user-attachments/assets/edfd9899-fe46-40b5-9fd3-99cbffbd44ed" />

Now doing the same again go to the browser and search http://10.49.165.124/island/2100

<img width="954" height="854" alt="Screenshot 2026-04-23 132127" src="https://github.com/user-attachments/assets/e403ec13-3245-432a-af79-fa8938c2b0b9" />

View the page source again cause usually they like to hide something here-- 

<img width="938" height="861" alt="Screenshot 2026-04-23 132140" src="https://github.com/user-attachments/assets/c337518b-9be4-4f0c-ba57-3a8c6482849d" />

Here it says there is a file with a '.ticket' extension.
Run gobuster to find .ticket as the hint said
```
gobuster dir -u http://10.49.165.124/island/2100/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x ticket
```
<img width="932" height="543" alt="Screenshot 2026-04-23 132510" src="https://github.com/user-attachments/assets/73a9e7cf-bc1f-4065-8d2d-5fbde87367a4" />

Found another director : /green_arrow.ticket

Again going to the browser search http://10.49.165.124/island/2100/green_arrow.ticket.

<img width="943" height="866" alt="Screenshot 2026-04-23 132559" src="https://github.com/user-attachments/assets/7cb727e8-35d1-4f15-9365-f29b332699a4" />

Found an encryption : 'RTy8yhBQdscX' .  So now lets try to decode it ---

Go to https://gchq.github.io/CyberChef/

Use 'FromBase58' to decode it.

<img width="1918" height="919" alt="Screenshot 2026-04-23 132730" src="https://github.com/user-attachments/assets/5f6f0f68-4880-43de-9706-b9bc96cf6351" />

We have cracked it : '!#th3h00d' - This is the FTP Password.

## Step 3: FTP Login
