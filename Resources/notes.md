# Resources/notes.md

> **The Guestbook — Technical Research Notes**
>
> Professional pentesting notes for the TryHackMe **The Guestbook** room. These notes document the investigation process, API interactions, prompt-injection research, authorization analysis, and defensive observations while intentionally **redacting flags and spoiler-heavy outputs**.

---

# 🎯 Lab Summary

| Property          | Value                                   |
| ----------------- | --------------------------------------- |
| **Platform**      | TryHackMe                               |
| **Room**          | The Guestbook                           |
| **Category**      | Web Security / AI Security              |
| **Difficulty**    | Easy                                    |
| **Target**        | Byte Lotus Hotel Guestbook              |
| **AI Assistant**  | VERA                                    |
| **Primary Focus** | Prompt Injection & Authorization Bypass |

---

# 🧠 Objective

The objective was to investigate how **VERA**, an AI-powered concierge, processes guestbook entries and determine whether guest-controlled input could influence privileged application functionality.

The assessment focused on:

* Application reconnaissance.
* API discovery.
* Prompt Injection testing.
* Hidden directive enumeration.
* Authorization workflow analysis.
* Privileged diagnostic execution.
* Controlled data extraction.

---

# 🛠️ Environment Notes

## Target

```text
http://MACHINE_IP
```

Replace `MACHINE_IP` with the active TryHackMe target.

## Tools Used

| Tool                    | Purpose                        |
| ----------------------- | ------------------------------ |
| Browser Developer Tools | Inspect requests and responses |
| curl                    | Manual HTTP requests           |
| grep                    | Source code filtering          |
| base64                  | Decode encoded output          |
| Linux Terminal          | Enumeration & decoding         |
| Firefox / Chrome        | Reconnaissance                 |

---

# 🌐 Initial Reconnaissance

## Homepage

```bash
curl -s http://MACHINE_IP/
```

### Observation

* Guestbook interface loads.
* Guest submission form available.
* VERA Night Review panel visible.
* AI workflow appears asynchronous.

---

## Guestbook Endpoint

```bash
curl -s http://MACHINE_IP/guestbook
```

### Purpose

Retrieve stored guest entries.

### Observation

Records include:

* Guest name.
* Room number.
* Guest message.
* Review status.
* Internal identifiers.

---

## Activity Endpoint

```bash
curl -s http://MACHINE_IP/vera/activity
```

### Purpose

Monitor VERA's review activity.

### Observation

Initially empty.

After submitting entries:

* AI review history appears.
* Internal workflow becomes visible.
* Useful evidence for prompt execution.

---

# 🔍 Source Code Enumeration

Download homepage.

```bash
curl -s http://MACHINE_IP/ -o index.html
```

Search interesting strings.

```bash
grep -Ein 'guest|vera|activity|entry|note|lookup|override|flag' index.html
```

### Useful Findings

Potential routes discovered:

```text
/
 /guestbook
 /vera/activity
 /entry
```

No complex client-side JavaScript exploitation required.

---

# ✍️ Baseline Guest Submission

Create a harmless guestbook entry.

```bash
curl -s -X POST http://MACHINE_IP/entry \
  -d "name=TestGuest" \
  -d "room=101" \
  -d "message=Hello VERA!"
```

### Expected Behavior

* Entry accepted.
* AI review scheduled.
* Activity endpoint updated.

---

# 🤖 Understanding VERA

### Workflow Identified

```text
Guest Submission
      │
      ▼
Guestbook Storage
      │
      ▼
VERA Review Queue
      │
      ▼
Activity Log
```

Important finding:

Guestbook content is **processed**, not simply stored.

---

# 💉 Prompt Injection Research

## Goal

Determine whether VERA interprets natural language as executable instructions.

### Conceptual Test

Guest message requested VERA to review a specific guest record.

### Observation

VERA changed review behavior.

### Security Interpretation

Prompt Injection successful.

Guest-controlled text influences AI workflow.

---

## Behavioral Notes

Prompt Injection allowed influence over:

* Selected guest.
* Review context.
* Internal retrieval behavior.

No authentication required.

---

# 🗂️ Private Record Enumeration

Prompt requested retrieval of non-public guest information.

### Observation

VERA exposed information unavailable through the public guestbook endpoint.

### Impact

Internal record access confirmed.

Sensitive output intentionally omitted.

---

# 🧩 Hidden Directive Discovery

Testing revealed undocumented directives.

```text
note:
lookup:
flag:
override:
```

## Directive Summary

| Directive   | Purpose                            |
| ----------- | ---------------------------------- |
| `note:`     | Internal note workflow             |
| `lookup:`   | Record lookup                      |
| `flag:`     | Internal reference mechanism       |
| `override:` | Manager-only diagnostic capability |

Most interesting capability:

**override**

---

# 🔒 Authorization Testing

## Direct Override Attempt

Attempting `override:` immediately fails.

### Response

Authorization required.

### Conclusion

Privilege boundary exists.

---

## Authorization Workflow Analysis

Critical observation:

Approval can be established for the **next guestbook entry**.

### State Transition

```text
Entry A
      │
Authorization Message
      │
      ▼
Approval Stored
      │
      ▼
Entry B
      │
Consumes Approval
      ▼
Manager Override Executes
```

### Security Finding

Authorization is **state-based**, not identity-based.

---

# ⚙️ Privileged Diagnostic Research

After approval state existed:

Manager diagnostic executed.

### Observation

Filesystem enumeration occurs.

Protected manager resources become visible.

Sensitive paths omitted.

---

# 📂 Protected Resource Discovery

Manager diagnostic reveals protected storage.

Observed behavior only.

No sensitive filenames documented here.

---

# 📦 Encoded Output Handling

Direct sensitive output is filtered.

### Alternative Workflow

Output returned in Base64.

### Local Decoding

```bash
echo "<BASE64_OUTPUT>" | base64 -d
```

### Second Decode

```bash
echo "<SECOND_LAYER>" | base64 -d
```

### Result

Protected flag recovered locally.

**Redacted from repository.**

---

# 📡 Useful curl Commands

## GET Homepage

```bash
curl -s http://MACHINE_IP/
```

## GET Guestbook

```bash
curl -s http://MACHINE_IP/guestbook
```

## GET Activity

```bash
curl -s http://MACHINE_IP/vera/activity
```

## POST Guest Entry

```bash
curl -X POST http://MACHINE_IP/entry \
-d "name=Guest" \
-d "room=101" \
-d "message=Hello"
```

## Save JSON Response

```bash
curl -s http://MACHINE_IP/guestbook | jq .
```

---

# 🔍 Investigation Timeline

| Phase    | Activity                   |
| -------- | -------------------------- |
| Phase 1  | Reconnaissance             |
| Phase 2  | Endpoint Enumeration       |
| Phase 3  | Guest Submission           |
| Phase 4  | Prompt Injection Testing   |
| Phase 5  | Private Record Enumeration |
| Phase 6  | Directive Discovery        |
| Phase 7  | Authorization Analysis     |
| Phase 8  | Manager Override           |
| Phase 9  | Encoded Output Retrieval   |
| Phase 10 | Local Decoding             |

---

# 📈 Attack Chain Notes

```text
Guest Input
     │
Prompt Injection
     │
Hidden Directives
     │
Authorization State Manipulation
     │
Manager Diagnostic
     │
Protected File Discovery
     │
Encoded Output
     │
Local Decode
```

---

# 🚨 Security Findings

| Finding ID | Description                                                         |
| ---------- | ------------------------------------------------------------------- |
| **GKB-01** | Prompt Injection through guestbook input.                           |
| **GKB-02** | Hidden AI directives exposed through instruction probing.           |
| **GKB-03** | Authorization delegated through guest-controlled state.             |
| **GKB-04** | Manager-only diagnostic reachable after authorization manipulation. |
| **GKB-05** | Internal guest record disclosure.                                   |
| **GKB-06** | Encoded output bypasses direct response filtering.                  |

---

# 🛡️ Defensive Notes

## AI Security

* Treat prompts as untrusted input.
* Separate prompts from permissions.
* Restrict model tool access.

## Authorization

* Bind approval to authenticated identity.
* Prevent "next request" authorization.
* Validate privileged actions server-side.

## Application Security

* Allowlist diagnostic tools.
* Audit privileged operations.
* Remove hidden directives from public workflows.
* Apply least privilege to AI agents.

---

# 🧪 Commands Cheat Sheet

### Enumeration

```bash
curl -s http://MACHINE_IP/
curl -s http://MACHINE_IP/guestbook
curl -s http://MACHINE_IP/vera/activity
```

### Source Inspection

```bash
curl -s http://MACHINE_IP/ -o index.html
grep -Ein 'guest|vera|activity|override|lookup|flag' index.html
```

### Guest Submission

```bash
curl -X POST http://MACHINE_IP/entry \
-d "name=Research" \
-d "room=202" \
-d "message=Testing VERA"
```

### Decode Output

```bash
echo "<BASE64_OUTPUT>" | base64 -d
```

---

# 📚 Key Takeaways

### Technical Concepts Practiced

* REST API Enumeration
* AI Prompt Injection
* Authorization Boundary Testing
* Hidden Functionality Discovery
* Privileged Workflow Analysis
* Base64 Output Analysis
* Trust Boundary Violations
* Evidence Collection
* AI Security Misconfiguration Analysis

---

# ⚠️ Portfolio Disclaimer

These notes accompany an **authorized TryHackMe laboratory**.

This repository intentionally:

* ❌ Does not publish the final flag.
* ❌ Does not expose spoiler-heavy sensitive outputs.
* ❌ Does not include exploit-ready privileged payloads.
* ✅ Focuses on methodology, reasoning, and defensive understanding.

Use this repository **only for educational and authorized cybersecurity practice**.

---

## 👨‍💻 Author

**Anurag Revankar**

Cybersecurity Portfolio • TryHackMe Writeups • AI Security Research • Ethical Hacking
