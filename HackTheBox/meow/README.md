# HTB: Meow — Starting Point

**Platform:** Hack The Box  
**Difficulty:** Very Easy  
**Category:** Starting Point  
**Date:** April 2026

---

## Overview

Meow is the first machine in Hack The Box's Starting Point series. It is designed to introduce the basic workflow of connecting to a target, enumerating services, and gaining access. The flag is retrieved by logging into an exposed Telnet service with no authentication.

---

## What I Learned

The main takeaway from this one was Telnet. I knew the name but had not thought much about what it actually is. Telnet is essentially an unencrypted remote login protocol that predates SSH — it lets you connect to a remote machine over the network and interact with it via a command line interface. The problem is that everything is sent in plaintext, including credentials, which is why it was replaced by SSH. But the key insight here is that if a machine has Telnet running and is not properly secured, you can simply connect with:
telnet <target IP>

and if there is no password set on a user account, you are in. No exploit required. No credentials required. Just the protocol doing exactly what it was designed to do, poorly secured.

I did not need to look up the answers to the conceptual questions in this room. The enumeration with nmap, identifying the open port, recognizing the service, and knowing to try logging in as root with no password were all steps I worked through on my own.

---

## Steps

**1. Ping the target to confirm connectivity**
ping <target IP>

**2. Enumerate open ports with nmap**
nmap -sV <target IP>
Port 23 is open — that is Telnet.

**3. Connect via Telnet**
telnet <target IP>

**4. Log in as root with no password**

The login prompt appears. Enter `root` as the username and press enter when prompted for a password. Access granted.

**5. Retrieve the flag**
ls
cat flag.txt

---

## Reflection

This room is intentionally simple but the concept is real. Misconfigured services with no authentication are a legitimate attack surface. Telnet on port 23 still shows up in enterprise network scans more often than it should. The nmap step is the more transferable skill here — getting comfortable running service version scans and reading the output is something that shows up in almost every box.

First box done.
