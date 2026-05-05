
HTB — Fawn
Very Easy Starting Point FTP Linux

Fawn is a very easy Starting Point machine focused entirely on FTP. The goal is to understand what FTP is, how anonymous access works, and how to retrieve files from an FTP server.
Enumeration
Step 01
Port scan the target

First, find out what services are running.

nmap -sV <target-ip>

21/tcp open ftp vsftpd 3.0.3

The scan reveals port 21 is open running vsftpd 3.0.3. FTP (File Transfer Protocol) is a standard protocol for transferring files between a client and server.
Foothold
Step 02
Connect via FTP using anonymous login

Many FTP servers allow unauthenticated access using the username anonymous.

ftp <target-ip>

Name: anonymous Password: [press Enter] 230 Login successful.
Step 03
List files on the server

ftp> ls

-rw-r--r-- 1 0 0 32 Jun 04 2021 flag.txt
Step 04
Download the flag

ftp> get flag.txt

226 Transfer complete.
Step 05
Read the flag

cat flag.txt


What I learned:
What FTP actually is and how to use it, 
FTP (port 21) is a protocol for transferring files between machines,
Anonymous FTP access allows login without an account using the username anonymous,
nmap -sV identifies services and their versions running on open ports,
FTP response code 230 means login successful,
Basic FTP commands: ls to list files, get to download them
