# The Guestbook — TryHackMe Walkthrough

> Professional, redacted portfolio documentation for **The Guestbook**, an AI-assisted web security challenge featuring VERA, prompt injection, hidden directives, a broken next-entry authorization flow, privileged diagnostics, and controlled extraction.

![TryHackMe](https://img.shields.io/badge/TryHackMe-The%20Guestbook-212C3D?style=for-the-badge) ![Focus](https://img.shields.io/badge/Focus-AI%20Security%20%7C%20Web%20Security-0B7285?style=for-the-badge) ![Portfolio](https://img.shields.io/badge/Portfolio-Redacted-7C3AED?style=for-the-badge)

## Overview

The challenge places an AI concierge named **VERA** behind a hotel guestbook. Guest-supplied text is interpreted as instructions, creating a trust boundary between an untrusted input form and privileged internal tooling.

The documented investigation follows:

`Guestbook → Prompt Injection → Directive Discovery → Authorization Bypass → Privileged Override → File Discovery → Encoded Output → Double Decode`

The final flag and selected raw outputs are intentionally redacted in this public repository to preserve the learning value of the lab and reduce answer-copying.

## Challenge Snapshot

| Field | Value |
|---|---|
| Platform | TryHackMe |
| Room | The Guestbook |
| Theme | AI Security / Web Security |
| Target | Byte Lotus Hotel guestbook |
| AI component | VERA |
| Key issue | Untrusted instructions reaching privileged tooling |
| Public flag | **Redacted** |

## Skills Demonstrated

- Web reconnaissance and API enumeration
- HTTP testing with `curl`
- Prompt-injection analysis
- Hidden functionality discovery
- Authorization-boundary testing
- Privileged command execution analysis
- Output-filter bypass reasoning
- Base64 decoding
- Evidence-driven reporting

## Repository Layout

```text
.
├── Documentation/
│   ├── Documentation.md
│   └── Documentation.docx
├── Resources/
│   └── notes.md
├── Screenshots/
│   ├── 01_guestbook_overview.png
│   ├── 02_api_activity.png
│   ├── 03_prompt_injection.png
│   ├── 04_directive_discovery.png
│   ├── 05_override_authorization.png
│   └── 06_encoded_output.png
├── docs/
│   ├── index.md
│   └── assets/
│       ├── 01_guestbook_overview.png
│       ├── 02_api_activity.png
│       ├── 03_prompt_injection.png
│       ├── 04_directive_discovery.png
│       ├── 05_override_authorization.png
│       └── 06_encoded_output.png
├── _config.yml
└── .github/workflows/pages.yml
```

## Methodology

### 01 — Reconnaissance

The application exposed the guestbook and VERA review workflow. The observable API surface included `/guestbook`, `/vera/activity`, and `/entry`.

### 02 — Behavioral Validation

A harmless entry established that submissions were automatically processed by VERA. The activity endpoint became the primary source of execution evidence.

### 03 — Prompt Injection

Natural-language instructions influenced which guest record VERA selected and what information she retrieved. This established that the guestbook field acted as an instruction channel.

### 04 — Directive Discovery

Testing exposed internal directives:

```text
note:
lookup:
flag:
override:
```

The `override:` directive was specifically described as manager-only.

### 05 — Authorization Boundary Failure

The critical flaw was a workflow that allowed an attacker-controlled entry to establish approval for the **next** guestbook entry. The following entry consumed that state.

### 06 — Privileged Execution

The authorized review reached manager-only diagnostic functionality and exposed the protected flag-file location.

### 07 — Controlled Extraction

Direct output was constrained by application filtering. Encoding the target file before retrieval produced an encoded result that could be decoded locally. The final flag is deliberately omitted.

## Security Takeaways

> **Natural-language instructions must never become a substitute for authorization.**

Useful controls include server-side authorization tied to authenticated identity, explicit tool allowlists, least privilege, action-specific approval, strict tool-argument validation, protected manager data, and detailed audit logs.

## Responsible Use

This repository documents an authorized TryHackMe lab for education and security practice. Do not apply these techniques to systems without permission.

## Lab Reference

TryHackMe — The Guestbook: https://tryhackme.com/room/hh-theguestbook-0130ffaf

**Author:** Anurag Revankar  
**Documentation date:** 2026-09-24
