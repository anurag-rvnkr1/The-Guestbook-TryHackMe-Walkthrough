# 👻 The Guestbook — Professional TryHackMe CTF Walkthrough

<div align="center">

# The Guestbook

### AI Prompt Injection • Authorization Bypass • Web Security Analysis

**Professional Technical Documentation**

*Cybersecurity Portfolio Edition*

---

**Author:** Anurag Revankar

**Platform:** TryHackMe

**Category:** Web Application Security • AI Security

**Status:** Completed ✔️

**Portfolio Version:** Public (Flags Redacted)

</div>

---

# Executive Summary

## Overview

**The Guestbook** is an AI-powered web security challenge from **TryHackMe** that demonstrates how Large Language Models (LLMs) integrated into web applications can introduce entirely new security vulnerabilities when user-controlled input is trusted as executable instructions.

The fictional **Byte Lotus Hotel** allows guests to leave feedback through a guestbook. Behind the scenes, an AI concierge named **VERA** automatically reviews each guestbook entry and performs internal actions based on its contents.

This challenge explores how an attacker can manipulate VERA into performing privileged operations through carefully crafted guestbook messages, ultimately abusing an authorization flaw to access protected information intended only for hotel management.

Rather than exploiting traditional vulnerabilities such as SQL Injection or Cross-Site Scripting, this room focuses on modern **AI Application Security**, specifically **Prompt Injection**, **Agent Workflow Manipulation**, and **Authorization Boundary Failures**.

---

## Objectives

During this assessment, the following objectives were completed:

* Reconnaissance of the guestbook application.
* API enumeration and endpoint discovery.
* Understanding asynchronous AI processing.
* Prompt Injection against VERA.
* Discovery of undocumented internal AI directives.
* Analysis of authorization logic.
* Manager-only privilege escalation.
* Controlled extraction of protected data.
* Security impact assessment.
* Defensive recommendations for AI-powered applications.

---

## Skills Demonstrated

| Web Security      | AI Security                | Security Engineering   |
| ----------------- | -------------------------- | ---------------------- |
| API Enumeration   | Prompt Injection           | Authorization Analysis |
| HTTP Requests     | Hidden Directive Discovery | Privilege Escalation   |
| Reconnaissance    | Agent Manipulation         | Security Architecture  |
| Source Inspection | Trust Boundary Analysis    | Threat Modeling        |
| Data Extraction   | AI Workflow Analysis       | Defensive Design       |

---

# Room Information

| Property     | Value                      |
| ------------ | -------------------------- |
| Platform     | TryHackMe                  |
| Room         | The Guestbook              |
| Difficulty   | Easy                       |
| Category     | Web Security               |
| Theme        | AI Security                |
| Environment  | Linux / Web Application    |
| Target       | Byte Lotus Hotel Guestbook |
| AI Assistant | VERA                       |

---

# Learning Outcomes

After completing this room, you should understand:

* Prompt Injection fundamentals.
* AI-assisted application workflows.
* Authorization design failures.
* Internal tool discovery.
* Secure AI architecture principles.
* Least privilege for AI agents.
* Trust boundaries inside LLM-integrated systems.

---

# Target Environment

## Lab Architecture

```text
                    Internet
                        │
                        ▼
              Byte Lotus Hotel Website
                        │
        ┌───────────────┴───────────────┐
        │                               │
        ▼                               ▼
 Guestbook Frontend                VERA AI Service
        │                               │
        │                         Internal Review Engine
        │                               │
        └──────────────┬────────────────┘
                       ▼
                Guestbook Database
                       │
                       ▼
             Manager Diagnostic Tools
```

The application separates guest interaction from hotel management, but VERA acts as the bridge between both systems.

---

# Environment Setup

## Attack Platform

The room was solved using the **TryHackMe AttackBox**.

### Operating System

* Ubuntu Linux
* Bash Shell
* curl
* grep
* base64

### Browser

* Firefox Developer Tools
* Network Inspector

### Utilities Used

| Tool             | Purpose               |
| ---------------- | --------------------- |
| curl             | HTTP Requests         |
| grep             | Source Filtering      |
| base64           | Decode Encoded Output |
| Browser DevTools | Inspect API Requests  |
| Linux Terminal   | Manual Enumeration    |

---

# Scope of Assessment

The assessment remained entirely inside the authorized TryHackMe environment.

### Included

* Guestbook functionality.
* Public API endpoints.
* AI assistant behavior.
* Guest review workflow.
* Authorization workflow.

### Excluded

* External infrastructure.
* Third-party systems.
* Real hotel systems.
* Unauthorized exploitation.

---

# Application Overview

## Byte Lotus Hotel Guestbook

The homepage exposes a guestbook where hotel visitors submit feedback after their stay.

The application advertises a feature called **VERA Night Review**, where the AI concierge reviews guestbook entries overnight.

### Initial Observations

* Guest entries are accepted immediately.
* Reviews happen asynchronously.
* Previous reviews are visible.
* VERA appears to summarize guest messages.

This indicates that submitted messages undergo additional processing instead of simple storage.

---

# Screenshot — Application Overview

![Guestbook Overview](../docs/assets/01_guestbook_overview.png)

**Figure 1 — Byte Lotus Hotel Guestbook Interface**

The interface provides:

* Guest name.
* Room number.
* Guest message.
* Submit button.
* VERA review history.

This becomes the primary attack surface throughout the assessment.

---

# Reconnaissance Phase

Reconnaissance was performed to understand the application's exposed functionality.

---

## Homepage Enumeration

Initial request:

```bash
curl -s http://MACHINE_IP/
```

### Objective

Identify:

* Static resources.
* API calls.
* JavaScript.
* Hidden routes.

### Observation

The application loads successfully with:

* Guestbook UI.
* Review interface.
* Minimal JavaScript.

No authentication is required.

---

## HTML Source Inspection

Download homepage source.

```bash
curl -s http://MACHINE_IP/ -o index.html
```

Search relevant strings.

```bash
grep -Ein "guest|vera|entry|activity|review|note|lookup|override" index.html
```

### Findings

Interesting application keywords appear.

Potential API routes identified.

---

## Endpoint Enumeration

Discovered endpoints.

| Endpoint         | Purpose         |
| ---------------- | --------------- |
| `/`              | Homepage        |
| `/guestbook`     | Guest Entries   |
| `/entry`         | Submit Entry    |
| `/vera/activity` | Review Activity |

These endpoints become the core focus during testing.

---

# Guestbook Enumeration

Retrieve guest entries.

```bash
curl -s http://MACHINE_IP/guestbook
```

### Expected Response

JSON containing guestbook records.

Typical fields include:

* Guest Name
* Room Number
* Message
* Status
* Review Metadata

### Security Observation

The endpoint exposes review metadata unavailable on the homepage.

This indicates an internal review process connected to VERA.

---

# Screenshot — Guestbook API

![Guestbook API Activity](../docs/assets/02_api_activity.png)

**Figure 2 — Guestbook API Response**

The endpoint returns structured guestbook information used internally by VERA.

---

# Activity Endpoint Enumeration

Query VERA activity.

```bash
curl -s http://MACHINE_IP/vera/activity
```

Initially empty.

After guest submission, activity records begin appearing.

---

## Why This Matters

This endpoint provides evidence that VERA performs:

* Message parsing.
* Internal reasoning.
* Tool invocation.
* Review completion.

This is the first indication that guestbook entries trigger privileged application workflows.

---

# Baseline Functionality Test

A harmless guestbook entry is submitted.

```bash
curl -s -X POST http://MACHINE_IP/entry \
  -d "name=ResearchUser" \
  -d "room=101" \
  -d "message=Hello VERA!"
```

---

## Expected Behavior

Server returns success.

Guestbook entry stored.

Review queued.

Activity updated shortly afterward.

---

## Behavioral Analysis

Review lifecycle:

```text
Guest Submission
      │
      ▼
Database Storage
      │
      ▼
VERA Review Queue
      │
      ▼
Activity Endpoint Updated
```

This confirms asynchronous AI processing.

---

# VERA Review Engine Analysis

VERA is not simply summarizing text.

Evidence suggests VERA performs internal operations before producing a review.

Possible workflow:

```text
Read Guestbook Entry
       │
Natural Language Interpretation
       │
Internal Tool Selection
       │
Database Retrieval
       │
Generate Review
       │
Publish Activity
```

This architecture becomes the primary trust boundary explored later.

---

# Threat Modeling the Application

## Trust Boundaries

```text
Guest Input
     │
     ▼
Guestbook Database
     │
     ▼
VERA Prompt Context
     │
     ▼
Internal AI Tools
     │
     ▼
Manager Resources
```

### Critical Observation

Guest-controlled input crosses into an internal AI context.

If authorization is not enforced independently, privileged tools become reachable.

---

# Attack Surface Summary

| Component           | Attack Surface          |
| ------------------- | ----------------------- |
| Guestbook Form      | User-Controlled Input   |
| Guestbook API       | Public Data Retrieval   |
| Activity API        | AI Workflow Observation |
| AI Prompt Context   | Prompt Injection        |
| Internal Directives | Hidden Functionality    |
| Manager Override    | Privilege Escalation    |

---

# Reconnaissance Findings

| Finding | Description                                                  |
| ------- | ------------------------------------------------------------ |
| R-01    | Guestbook accepts arbitrary text input.                      |
| R-02    | No authentication required for submissions.                  |
| R-03    | Activity endpoint exposes AI processing metadata.            |
| R-04    | Guest entries trigger asynchronous reviews.                  |
| R-05    | Internal AI workflow appears connected to guestbook content. |

---

# Security Notes

### Potential Risks Identified During Reconnaissance

* User-controlled natural language reaches AI workflow.
* AI reviews execute automatically.
* Internal processing visible through activity endpoint.
* No visible authorization boundary between guestbook and AI review engine.
* Hidden application functionality likely exists beyond documented routes.

These observations establish the foundation for the exploitation phase documented in the following sections.

---

---

# Phase 2 — Prompt Injection & AI Security Analysis

## Introduction

After completing reconnaissance and understanding the guestbook workflow, the next objective was to determine **how VERA interprets guestbook messages**.

Unlike a traditional web application that stores guest feedback as inert text, VERA actively processes every message as part of its nightly review workflow.

This creates an important security question:

> **Does VERA treat user-controlled input as data, or as instructions?**

The remainder of this phase investigates that trust boundary.

---

# Understanding Prompt Injection

## What is Prompt Injection?

Prompt Injection is an AI-specific vulnerability where an attacker provides carefully crafted natural-language input that changes the behavior of a language model or AI agent.

Instead of exploiting SQL parsers or JavaScript interpreters, Prompt Injection targets **the model's instruction-following behavior**.

### Traditional Web Injection vs AI Injection

| Traditional Injection                 | AI Prompt Injection                          |
| ------------------------------------- | -------------------------------------------- |
| SQL parser executes injected query.   | AI follows attacker-controlled instructions. |
| Browser executes injected JavaScript. | AI changes reasoning or tool usage.          |
| Input changes backend logic.          | Input changes model behavior.                |
| Syntax-based attack.                  | Natural-language attack.                     |

---

# Trust Boundary Analysis

The guestbook introduces an unusual trust relationship.

```text
User Message
      │
      ▼
 Guestbook Database
      │
      ▼
 VERA Prompt Context
      │
      ▼
 Internal AI Reasoning
      │
      ▼
 Internal Tool Execution
```

### Security Observation

Guest-controlled content crosses directly into VERA's reasoning context.

If no additional validation exists, attacker input may influence:

* Which records are reviewed.
* Which tools are invoked.
* Which internal information is retrieved.

---

# Establishing Baseline AI Behavior

A harmless guestbook message was submitted to observe VERA's normal workflow.

```bash
curl -s -X POST http://MACHINE_IP/entry \
  -d "name=ResearchUser" \
  -d "room=101" \
  -d "message=Hello VERA!"
```

### Expected Result

The server acknowledges the submission.

VERA later publishes a review through the activity endpoint.

### Observation

* Entry accepted.
* Review generated automatically.
* Message incorporated into AI reasoning.

This confirms the message is actively processed.

---

# Screenshot — Prompt Injection Testing

![Prompt Injection Evidence](../docs/assets/03_prompt_injection.png)

**Figure 3 — Prompt Injection Investigation**

The screenshot demonstrates the guestbook message influencing VERA's review behavior.

Sensitive review content has been intentionally redacted.

---

# Behavioral Testing Methodology

Prompt Injection testing followed a staged methodology.

## Stage 1 — Safe Instructions

The initial objective was to determine whether VERA obeyed harmless instructions.

Examples included requests to:

* Summarize a guest.
* Mention a room number.
* Reference an existing guest.

### Observation

VERA altered review output according to the supplied instruction.

This demonstrates instruction-following behavior.

---

## Stage 2 — Context Manipulation

The next objective was to determine whether VERA could be influenced to change its review context.

Instead of reviewing the latest guest entry, instructions attempted to redirect VERA toward another guest record.

### Observation

The selected guest changed.

VERA reviewed information associated with the attacker-selected context.

### Security Interpretation

Guestbook messages are influencing internal selection logic.

---

# Prompt Injection Findings

| Finding | Observation                                             |
| ------- | ------------------------------------------------------- |
| PI-01   | Guestbook input changes AI reasoning context.           |
| PI-02   | Review target becomes attacker-controlled.              |
| PI-03   | AI follows natural-language instructions.               |
| PI-04   | Internal workflow is influenced without authentication. |

---

# AI Reasoning Flow

The observed workflow can be modeled as follows.

```text
Guest Message
      │
Instruction Parsing
      │
Intent Selection
      │
Internal Tool Decision
      │
Guest Record Retrieval
      │
Response Generation
```

### Security Risk

The application relies on AI interpretation before authorization decisions.

---

# Investigating Guest Record Selection

Once VERA's behavior was understood, testing focused on guest record retrieval.

The objective was to determine whether VERA could access guest records beyond the currently reviewed entry.

### Behavioral Test

A guestbook message requested review of another known guest.

### Observation

VERA retrieved another guest record.

This indicates the AI has broader visibility than the public guestbook interface.

---

# Internal Record Access

VERA possesses access to information unavailable through the homepage.

### Evidence Collected

The AI response contained information absent from:

* Homepage.
* Guestbook listing.
* User-facing interface.

### Security Finding

Internal guest records contain additional fields.

Guest-controlled instructions influence retrieval of those fields.

---

# Private Guest Information

Testing escalated to determine whether VERA would retrieve non-public guest metadata.

### Objective

Request a private guest attribute during review.

### Observation

VERA returned internal guest information.

### Impact

This confirms **unauthorized internal data exposure** through Prompt Injection.

Sensitive values are intentionally omitted.

---

# Prompt Context Manipulation

An important observation is that VERA does not distinguish between:

* user content,
* system intent,
* operational instructions.

Instead, guestbook content becomes part of the AI prompt context.

```text
System Prompt
      │
Guest History
      │
Guest Message
      │
Attacker Instruction
      │
VERA Response
```

### Security Issue

User-controlled content modifies the effective prompt.

---

# Why This Matters

Prompt Injection becomes dangerous when AI agents have access to privileged capabilities.

### Example Trust Boundary

```text
Guestbook
     │
     ▼
Prompt Context
     │
     ▼
Internal Records
     │
     ▼
Manager Tools
```

Without isolation, the AI becomes an authorization oracle.

---

# Hidden AI Functionality Investigation

The next stage focused on discovering undocumented AI capabilities.

Instead of requesting guest information, testing requested information **about VERA itself**.

The goal was to determine whether VERA exposed internal operational commands.

---

# Directive Discovery Methodology

Rather than brute forcing endpoints, the investigation queried VERA about:

* internal commands,
* review workflow,
* available directives,
* operational capabilities.

### Result

VERA revealed undocumented directives.

---

# Screenshot — Hidden Directives

![Hidden Directive Discovery](../docs/assets/04_directive_discovery.png)

**Figure 4 — Internal Directive Discovery**

The AI exposes operational directives intended for internal workflows.

Sensitive outputs have been redacted.

---

# Hidden Directive Enumeration

The following directives were discovered.

```text
note:
lookup:
flag:
override:
```

Each directive appears to correspond to a different internal workflow.

---

## Directive Analysis

<table>
<tr>
<th>Directive</th>
<th>Purpose</th>
<th>Security Impact</th>
</tr>

<tr>
<td><code>note:</code></td>
<td>Create internal review notes.</td>
<td>Low</td>
</tr>

<tr>
<td><code>lookup:</code></td>
<td>Retrieve guest information.</td>
<td>Medium</td>
</tr>

<tr>
<td><code>flag:</code></td>
<td>Reference protected workflow data.</td>
<td>Medium</td>
</tr>

<tr>
<td><code>override:</code></td>
<td>Manager-only diagnostic workflow.</td>
<td>Critical</td>
</tr>

</table>

---

# Directive Threat Assessment

### note:

Appears to create or update review notes.

### lookup:

Queries guest records beyond visible guestbook entries.

### flag:

References internal review information.

### override:

Documented as **manager-authorized**.

This becomes the primary privilege boundary investigated next.

---

# Internal Tool Discovery

The presence of directives suggests VERA acts as an **AI Agent** with access to backend tooling.

Conceptually:

```text
Prompt
   │
Directive
   │
Internal Tool
   │
Database
   │
Filesystem
```

### Security Observation

Prompt Injection can reach backend capabilities if directives are insufficiently protected.

---

# AI Agent Security Perspective

VERA resembles an autonomous AI assistant with:

* reasoning capability,
* tool selection,
* privileged backend actions,
* memory of guest records.

### Agent Components

| Component      | Role              |
| -------------- | ----------------- |
| Guest Prompt   | User Input        |
| Prompt Context | AI Memory         |
| Internal Tools | Backend Actions   |
| Activity Feed  | Observable Output |

---

# Mapping to OWASP LLM Top 10

| OWASP LLM Risk                           | Relation to The Guestbook               |
| ---------------------------------------- | --------------------------------------- |
| LLM01 — Prompt Injection                 | Primary vulnerability.                  |
| LLM02 — Sensitive Information Disclosure | Guest record exposure.                  |
| LLM06 — Excessive Agency                 | AI accesses internal tools.             |
| LLM08 — Excessive Permissions            | Manager functionality reachable.        |
| LLM09 — Overreliance                     | Authorization delegated to AI workflow. |

---

# Mapping to MITRE ATT&CK

| Technique | Description                                            |
| --------- | ------------------------------------------------------ |
| T1190     | Exploit Public-Facing Application                      |
| T1211     | Exploitation for Privilege Escalation                  |
| T1552     | Unsecured Credentials / Sensitive Information Exposure |
| T1059     | Command Execution (via privileged diagnostic)          |
| T1005     | Data from Local System                                 |

This room combines web exploitation with AI-assisted privilege escalation concepts.

---

# Threat Model Update

### Assets Identified

* Guest Records.
* Review History.
* Manager Diagnostic Tool.
* Protected Flag File.

### Threat Actors

* Anonymous guest.
* Guestbook user.
* Internal manager.
* AI assistant.

### Trust Boundaries

```text
Anonymous Guest
        │
Guestbook Entry
        │
VERA Context
        │
Manager Workflow
```

The manager workflow should never be reachable from guest-controlled content.

---

# Security Findings (Phase 2)

| ID    | Finding                                                          | Severity |
| ----- | ---------------------------------------------------------------- | -------- |
| AI-01 | Prompt Injection changes AI reasoning context.                   | High     |
| AI-02 | Internal guest records become retrievable.                       | High     |
| AI-03 | Hidden directives exposed through conversation.                  | High     |
| AI-04 | AI possesses privileged internal capabilities.                   | Critical |
| AI-05 | Trust boundary between guest input and AI tools is insufficient. | Critical |

---

# Defensive Recommendations (Phase 2)

## Prompt Isolation

* Treat guestbook entries as untrusted content.
* Prevent user input from modifying system instructions.
* Separate system prompts from user prompts.

## Tool Isolation

* Require deterministic authorization before tool invocation.
* Restrict internal directives to authenticated workflows.
* Validate tool arguments outside the LLM.

## Context Separation

* Do not merge guest content with privileged operational context.
* Remove internal directives from conversational context.
* Limit AI visibility into protected records.

---

---

# Phase 3 — Authorization Bypass & Privilege Escalation

## Introduction

The previous phase established that VERA exposes hidden directives and interprets guestbook entries as operational instructions.

The next objective was to investigate **whether privileged directives were properly protected**.

The most interesting directive discovered was:

```text
override:
```

According to VERA's internal documentation, this directive was intended for **hotel managers only**.

The assessment therefore shifted from Prompt Injection to **authorization testing**.

---

# Understanding the Authorization Model

## Expected Secure Workflow

In a secure implementation, manager-only functionality should require an authenticated manager identity.

```text
Guest User
     │
     ▼
Guestbook Entry
     │
     ▼
VERA Review
     │
Authorization Check
     │
     ├──────────────┐
     │              │
 Guest            Manager
 Denied          Tool Executes
```

The AI should **never** grant manager permissions based solely on guest-controlled content.

---

# Investigating Manager Authorization

The first experiment attempted to invoke the manager-only directive directly.

### Observation

The request was rejected because authorization was required.

### Initial Conclusion

VERA appeared to enforce a privilege boundary.

However, further behavioral testing suggested that authorization was **stateful**, not identity-based.

---

# Authorization Testing Methodology

The objective was to determine:

* How approval is created.
* How approval is stored.
* Which request consumes approval.
* Whether approval is tied to identity.

Testing followed incremental behavioral analysis rather than brute force.

---

# Discovery — Next Entry Authorization Logic

A critical observation emerged during sequential guestbook submissions.

VERA accepted language that indicated **approval for the next review**.

The approval did **not** execute immediately.

Instead, it remained active for the following guestbook entry.

---

## Authorization State Flow

```text
Guestbook Entry A
        │
Attacker-controlled approval message
        │
        ▼
 Temporary Authorization State
        │
        ▼
 Guestbook Entry B
        │
Consumes Stored Authorization
        │
        ▼
Manager Override Executes
```

This represents the core vulnerability in the room.

---

# Why This Is a Broken Authorization Design

The application separates:

* creation of approval,
* consumption of approval.

But it fails to ensure both actions belong to the same authenticated manager.

### Security Issue

Authorization becomes **content-driven** instead of **identity-driven**.

---

# Trust Boundary Failure

The intended trust boundary:

```text
Guest
   │
   ▼
Guestbook
   │
   ▼
VERA
   │
   ▼
Manager Tools
```

The observed trust boundary:

```text
Guest
   │
Guest Message
   │
Creates Authorization
   │
   ▼
VERA
   │
Manager Tool Executes
```

Guest-controlled content crosses directly into privileged workflow execution.

---

# Screenshot — Authorization Workflow

![Authorization Bypass Evidence](../docs/assets/05_override_authorization.png)

**Figure 5 — Manager Authorization Workflow**

The screenshot demonstrates VERA accepting a manager-only workflow after consuming attacker-controlled authorization state.

Sensitive output has been intentionally redacted.

---

# Authorization Vulnerability Breakdown

| Component           | Intended Behavior     | Observed Behavior              |
| ------------------- | --------------------- | ------------------------------ |
| Approval            | Manager authenticated | Guest message creates approval |
| Authorization Scope | Manager only          | Next guestbook entry           |
| Identity Binding    | Required              | Missing                        |
| Privileged Tool     | Manager only          | Executed after guest workflow  |

---

# Security Classification

## Broken Authorization

**Type:** Authorization Logic Flaw

**Impact:** Critical

### Root Cause

The application trusts **natural-language approval** instead of authenticated authorization.

---

# Threat Modeling the Vulnerability

### Assets at Risk

* Manager diagnostics.
* Internal guest records.
* Protected filesystem resources.
* Sensitive operational information.

### Threat Actor

Anonymous guest.

### Privilege Gained

Manager-only diagnostic execution.

---

# Authorization Timeline

<table>
<tr>
<th>Step</th>
<th>Action</th>
<th>Security Observation</th>
</tr>

<tr>
<td>1</td>
<td>Guest submits review.</td>
<td>No authentication required.</td>
</tr>

<tr>
<td>2</td>
<td>Guest review contains approval language.</td>
<td>Authorization state created.</td>
</tr>

<tr>
<td>3</td>
<td>Next review processed.</td>
<td>Stored authorization consumed.</td>
</tr>

<tr>
<td>4</td>
<td>Manager directive executed.</td>
<td>Privilege escalation achieved.</td>
</tr>

</table>

---

# Manager Override Analysis

The hidden directive `override:` represents an internal diagnostic tool.

### Intended Capability

Manager-only maintenance workflow.

### Observed Capability

Filesystem inspection.

Internal diagnostic execution.

Protected resource discovery.

---

# Internal Diagnostic Workflow

Conceptually, VERA performs:

```text
Prompt
   │
override:
   │
Manager Diagnostic Tool
   │
Filesystem
   │
Diagnostic Output
```

This transforms Prompt Injection into **privileged backend execution**.

---

# AI Agent Privilege Escalation

VERA functions similarly to an AI Agent capable of invoking backend tools.

### Agent Capabilities

| Capability             | Security Risk          |
| ---------------------- | ---------------------- |
| Read guest records     | Information disclosure |
| Internal note creation | Workflow manipulation  |
| Lookup functionality   | Unauthorized retrieval |
| Manager override       | Privileged execution   |

---

# Principle of Least Privilege Violation

VERA has broader permissions than necessary.

### Secure Design

```text
Guest Review Tool
        │
Read Guestbook Only
```

### Observed Design

```text
Guest Review Tool
        │
Guest Records
Manager Records
Filesystem Diagnostics
Internal Directives
```

This violates least privilege.

---

# Diagnostic Execution Analysis

After authorization succeeded, VERA performed an internal diagnostic.

### Observed Behavior

The diagnostic revealed filesystem information.

Protected directories became visible.

### Security Interpretation

The AI agent can invoke privileged backend operations.

---

# Filesystem Discovery

The diagnostic exposed:

* protected storage,
* manager resources,
* hidden files.

Exact paths are intentionally omitted from this portfolio version.

---

# Privilege Escalation Summary

<table>
<tr>
<th>Finding</th>
<th>Description</th>
</tr>

<tr>
<td>PE-01</td>
<td>Manager-only functionality became reachable.</td>
</tr>

<tr>
<td>PE-02</td>
<td>Authorization state controlled by guest content.</td>
</tr>

<tr>
<td>PE-03</td>
<td>Filesystem diagnostic executed.</td>
</tr>

<tr>
<td>PE-04</td>
<td>Protected file location disclosed.</td>
</tr>

</table>

---

# Protected Resource Discovery

The diagnostic identified a protected file associated with VERA.

### Observation

The application does not immediately expose the file contents.

Instead, output handling introduces another defensive layer.

---

# Output Filtering

Direct sensitive output was constrained.

This indicates an attempt to prevent direct disclosure.

### Security Observation

Filtering protects output.

Filtering does **not** protect tool execution.

---

# Encoded Output Workflow

Instead of returning raw content, the protected data is transformed before transmission.

Conceptually:

```text
Protected File
       │
Encoding Layer
       │
Base64 Output
       │
Guest Response
```

---

# Screenshot — Encoded Output Retrieval

![Encoded Output Evidence](../docs/assets/06_encoded_output.png)

**Figure 6 — Encoded Output Analysis**

The protected response is returned in encoded form.

Sensitive values have been intentionally removed.

---

# Base64 Analysis

Base64 is an encoding mechanism.

It is **not** encryption.

### Workflow

```text
Protected File
      │
Base64 Encode
      │
Encoded Text
      │
Local Decode
```

The decoding occurs locally after collection.

---

# Controlled Extraction Methodology

The investigation demonstrates:

1. Collect encoded response.
2. Preserve evidence.
3. Decode locally.
4. Validate output.

The exact encoded string is intentionally omitted.

---

# Multi-Step Decoding

The challenge requires multiple decoding operations before recovering the protected value.

### Conceptual Workflow

```text
Encoded Output
      │
Decode
      │
Intermediate Output
      │
Decode Again
      │
Protected Result
```

The final flag remains redacted.

---

# Evidence Preservation

During extraction:

* Raw encoded response preserved.
* Decoding performed locally.
* No modification to server state.
* Final evidence stored offline.

This mirrors real incident-response methodology.

---

# Security Analysis — Why Encoding Didn't Prevent Disclosure

### Encoding Layer

Designed to obscure output.

### Weakness

Encoding does not prevent unauthorized access if privileged execution already occurred.

### Important Principle

> Output controls cannot compensate for broken authorization.

---

# Attack Chain (Complete)

```text
┌────────────────────────────┐
│ Guestbook Submission       │
└──────────────┬─────────────┘
               │
               ▼
┌────────────────────────────┐
│ Prompt Injection           │
└──────────────┬─────────────┘
               │
               ▼
┌────────────────────────────┐
│ Hidden Directive Discovery │
└──────────────┬─────────────┘
               │
               ▼
┌────────────────────────────┐
│ Authorization State Abuse  │
└──────────────┬─────────────┘
               │
               ▼
┌────────────────────────────┐
│ Manager Override           │
└──────────────┬─────────────┘
               │
               ▼
┌────────────────────────────┐
│ Filesystem Diagnostic      │
└──────────────┬─────────────┘
               │
               ▼
┌────────────────────────────┐
│ Protected Resource         │
└──────────────┬─────────────┘
               │
               ▼
┌────────────────────────────┐
│ Encoded Output             │
└──────────────┬─────────────┘
               │
               ▼
┌────────────────────────────┐
│ Local Multi-Step Decode    │
└────────────────────────────┘
```

---

# MITRE ATT&CK Mapping

| Technique | Description                               |
| --------- | ----------------------------------------- |
| T1190     | Exploit Public-Facing Application         |
| T1211     | Exploitation for Privilege Escalation     |
| T1059     | Command Execution through privileged tool |
| T1005     | Data from Local System                    |
| T1552     | Sensitive Information Exposure            |

---

# OWASP LLM Top 10 Mapping

| Risk  | Evidence                         |
| ----- | -------------------------------- |
| LLM01 | Prompt Injection                 |
| LLM02 | Sensitive Information Disclosure |
| LLM06 | Excessive Agency                 |
| LLM08 | Excessive Permissions            |
| LLM09 | Overreliance on AI Authorization |

---

# CVSS-Style Security Assessment

<table>
<tr>
<th>Finding</th>
<th>Severity</th>
</tr>

<tr>
<td>Prompt Injection</td>
<td>High</td>
</tr>

<tr>
<td>Internal Directive Discovery</td>
<td>High</td>
</tr>

<tr>
<td>Broken Authorization</td>
<td>Critical</td>
</tr>

<tr>
<td>Manager Override Execution</td>
<td>Critical</td>
</tr>

<tr>
<td>Protected Resource Disclosure</td>
<td>High</td>
</tr>

<tr>
<td>Encoded Output Retrieval</td>
<td>Medium</td>
</tr>

</table>

---

# Security Findings (Phase 3)

| ID      | Finding                                          | Severity |
| ------- | ------------------------------------------------ | -------- |
| AUTH-01 | Guest-controlled authorization state.            | Critical |
| AUTH-02 | Missing identity binding.                        | Critical |
| AUTH-03 | Manager diagnostic reachable.                    | Critical |
| AUTH-04 | Filesystem information disclosure.               | High     |
| AUTH-05 | Protected data extracted through encoded output. | High     |

---

# Defensive Recommendations

## Authorization Controls

* Bind approval to authenticated manager identity.
* Use short-lived authorization tokens.
* Scope authorization to a single action.
* Prevent approval inheritance across requests.

## AI Tool Security

* Separate reasoning from execution.
* Require server-side authorization before every tool call.
* Remove privileged directives from conversational context.

## Filesystem Protection

* Replace arbitrary diagnostics with allowlisted operations.
* Never expose filesystem output through conversational responses.
* Restrict AI agent permissions.

## Monitoring

Detect sequences involving:

```text
Guest Prompt
      │
Authorization Language
      │
Override Directive
      │
Filesystem Access
```

These patterns should generate security alerts.

---

---

# Phase 4 — Blue Team Analysis & Defensive Security Architecture

## Introduction

The previous phases demonstrated how an attacker could influence VERA through Prompt Injection, discover privileged directives, abuse authorization state, and reach manager-only functionality.

This final phase focuses on the **defensive perspective** — how organizations should design, monitor, and secure AI-powered applications against these attack patterns.

The Guestbook is a valuable AI security case study because the vulnerability is **architectural**, not simply a weak prompt.

---

# Security Architecture Review

## Original Architecture

```text
                    Guest User
                         │
                         ▼
                Guestbook Submission
                         │
                         ▼
                 Guestbook Database
                         │
                         ▼
               VERA Prompt Context
                         │
         ┌───────────────┴───────────────┐
         │                               │
         ▼                               ▼
  Guest Record Lookup           Manager Diagnostic
         │                               │
         └───────────────┬───────────────┘
                         ▼
                 Activity / Response
```

### Primary Weakness

VERA receives guest-controlled content and privileged operational capabilities within the same execution context.

---

# Secure Architecture Recommendation

A secure AI assistant should separate reasoning from authorization.

```text
Guest Input
     │
     ▼
Input Validation Layer
     │
     ▼
LLM Reasoning Context
     │
     ▼
Policy Enforcement Engine
     │
     ▼
Authorized Tool Gateway
     │
     ▼
Approved Internal Services
```

### Benefits

* AI cannot invoke tools directly.
* Authorization becomes deterministic.
* User prompts remain isolated from privileged operations.

---

# Defense in Depth Strategy

A layered approach prevents Prompt Injection from becoming privilege escalation.

<table>
<tr>
<th>Layer</th>
<th>Control</th>
</tr>

<tr>
<td>User Input</td>
<td>Input sanitization, contextual isolation.</td>
</tr>

<tr>
<td>Prompt Context</td>
<td>Separate user/system prompts.</td>
</tr>

<tr>
<td>Tool Invocation</td>
<td>Server-side policy validation.</td>
</tr>

<tr>
<td>Authorization</td>
<td>Identity-based access control.</td>
</tr>

<tr>
<td>Filesystem</td>
<td>Least privilege & sandboxing.</td>
</tr>

<tr>
<td>Monitoring</td>
<td>Audit logs & anomaly detection.</td>
</tr>

</table>

---

# AI Security Threat Model

## Assets

<table>
<tr>
<th>Asset</th>
<th>Security Requirement</th>
</tr>

<tr>
<td>Guest Records</td>
<td>Confidentiality</td>
</tr>

<tr>
<td>Review History</td>
<td>Integrity</td>
</tr>

<tr>
<td>Manager Diagnostics</td>
<td>Authorization</td>
</tr>

<tr>
<td>Filesystem Resources</td>
<td>Least Privilege</td>
</tr>

<tr>
<td>Prompt Context</td>
<td>Isolation</td>
</tr>

</table>

---

## Threat Actors

<table>
<tr>
<th>Actor</th>
<th>Capability</th>
</tr>

<tr>
<td>Anonymous Guest</td>
<td>Guestbook submission.</td>
</tr>

<tr>
<td>Authenticated Guest</td>
<td>Guest record access.</td>
</tr>

<tr>
<td>Manager</td>
<td>Diagnostics & privileged review.</td>
</tr>

<tr>
<td>VERA AI Agent</td>
<td>Internal reasoning & tool execution.</td>
</tr>

</table>

---

# Attack Surface Review

<table>
<tr>
<th>Surface</th>
<th>Risk</th>
</tr>

<tr>
<td>Guestbook Form</td>
<td>Prompt Injection.</td>
</tr>

<tr>
<td>Activity API</td>
<td>Workflow visibility.</td>
</tr>

<tr>
<td>Guest Lookup</td>
<td>Information disclosure.</td>
</tr>

<tr>
<td>Hidden Directives</td>
<td>Privilege escalation.</td>
</tr>

<tr>
<td>Override Tool</td>
<td>Command execution.</td>
</tr>

</table>

---

# OWASP LLM Top 10 Mapping

<table>
<tr>
<th>Risk</th>
<th>Evidence</th>
</tr>

<tr>
<td>LLM01 — Prompt Injection</td>
<td>Guestbook instructions alter AI behavior.</td>
</tr>

<tr>
<td>LLM02 — Sensitive Information Disclosure</td>
<td>Private guest record retrieval.</td>
</tr>

<tr>
<td>LLM04 — Model Denial of Service</td>
<td>Not demonstrated.</td>
</tr>

<tr>
<td>LLM06 — Excessive Agency</td>
<td>VERA invokes backend functionality.</td>
</tr>

<tr>
<td>LLM08 — Excessive Permissions</td>
<td>Manager diagnostics reachable.</td>
</tr>

<tr>
<td>LLM09 — Overreliance</td>
<td>Authorization delegated to conversational logic.</td>
</tr>

</table>

---

# MITRE ATT&CK Mapping

<table>
<tr>
<th>Technique</th>
<th>Description</th>
</tr>

<tr>
<td>T1190</td>
<td>Exploit Public-Facing Application.</td>
</tr>

<tr>
<td>T1211</td>
<td>Exploitation for Privilege Escalation.</td>
</tr>

<tr>
<td>T1059</td>
<td>Command Execution.</td>
</tr>

<tr>
<td>T1005</td>
<td>Data from Local System.</td>
</tr>

<tr>
<td>T1552</td>
<td>Sensitive Information Exposure.</td>
</tr>

</table>

---

# Security Findings Summary

## Critical Findings

<table>
<tr>
<th>ID</th>
<th>Finding</th>
<th>Severity</th>
</tr>

<tr>
<td>GKB-01</td>
<td>Prompt Injection via guestbook messages.</td>
<td>High</td>
</tr>

<tr>
<td>GKB-02</td>
<td>Hidden AI directives discoverable.</td>
<td>High</td>
</tr>

<tr>
<td>GKB-03</td>
<td>Guest-controlled authorization state.</td>
<td>Critical</td>
</tr>

<tr>
<td>GKB-04</td>
<td>Manager-only override reachable.</td>
<td>Critical</td>
</tr>

<tr>
<td>GKB-05</td>
<td>Protected filesystem disclosure.</td>
<td>High</td>
</tr>

<tr>
<td>GKB-06</td>
<td>Encoded output bypasses filtering layer.</td>
<td>Medium</td>
</tr>

</table>

---

# Risk Assessment Matrix

<table>
<tr>
<th>Likelihood</th>
<th>Impact</th>
</tr>

<tr>
<td>High</td>
<td>Prompt Injection requires only guest input.</td>
</tr>

<tr>
<td>Critical</td>
<td>Privilege escalation reaches manager functionality.</td>
</tr>

<tr>
<td>High</td>
<td>Internal data becomes accessible.</td>
</tr>

<tr>
<td>Medium</td>
<td>Output encoding obscures—but does not prevent—disclosure.</td>
</tr>

</table>

---

# Blue Team Detection Opportunities

## Detect Prompt Injection Attempts

Indicators include repeated guest messages requesting:

* Internal commands.
* Manager functionality.
* Diagnostic operations.
* Hidden instructions.
* Tool execution.

Example detection rule concept:

```yaml
title: Prompt Injection Attempt
logsource:
  product: ai-assistant

detection:
  keywords:
    - override:
    - lookup:
    - note:
    - manager
    - diagnostic
condition: keywords
```

---

## Detect Authorization Abuse

Alert when:

* Guest review creates authorization state.
* Next guest review consumes privileged state.
* Authorization source ≠ authenticated manager.

Potential SIEM fields:

| Field                | Description             |
| -------------------- | ----------------------- |
| user_role            | Guest / Manager         |
| authorization_source | Identity or Prompt      |
| tool_name            | Invoked AI tool         |
| review_id            | Guest review identifier |

---

## Detect Privileged Tool Execution

Generate alerts when:

* Guest workflow invokes manager diagnostic.
* Filesystem tools execute from AI workflow.
* Guest context accesses protected resources.

Example detection pipeline:

```text
Guest Prompt
     │
Authorization Language
     │
Tool Invocation
     │
Filesystem Access
     │
Security Alert
```

---

# Logging Recommendations

Every AI tool invocation should record:

<table>
<tr>
<th>Field</th>
<th>Description</th>
</tr>

<tr>
<td>User ID</td>
<td>Authenticated identity.</td>
</tr>

<tr>
<td>User Role</td>
<td>Guest / Manager.</td>
</tr>

<tr>
<td>Prompt Hash</td>
<td>Reference to user prompt.</td>
</tr>

<tr>
<td>Tool Name</td>
<td>Invoked internal tool.</td>
</tr>

<tr>
<td>Authorization Decision</td>
<td>Allow / Deny.</td>
</tr>

<tr>
<td>Timestamp</td>
<td>Execution time.</td>
</tr>

<tr>
<td>Result</td>
<td>Success / Failure.</td>
</tr>

</table>

---

# Incident Response Perspective

If this vulnerability existed in production:

## Containment

* Disable manager diagnostic tools.
* Remove conversational authorization.
* Rotate privileged credentials.
* Review AI prompt templates.

## Investigation

Review logs for:

* Prompt Injection attempts.
* Directive discovery.
* Authorization anomalies.
* Unexpected filesystem access.

## Recovery

* Patch authorization workflow.
* Separate privileged tools.
* Deploy monitoring rules.
* Validate AI permissions.

---

# Secure AI Design Principles

## Principle 1 — Prompt Isolation

Separate:

* System prompt.
* User prompt.
* Tool context.

Never concatenate privileged operational instructions with user-controlled content.

---

## Principle 2 — Deterministic Authorization

Authorization should come from:

* authenticated identity,
* server-side policy,
* explicit permissions.

Never from natural-language approval.

---

## Principle 3 — Tool Sandboxing

Every AI tool should expose only the minimum required capability.

Example:

<table>
<tr>
<th>Unsafe</th>
<th>Safe</th>
</tr>

<tr>
<td>Filesystem shell access.</td>
<td>Read-only predefined function.</td>
</tr>

<tr>
<td>Arbitrary diagnostic commands.</td>
<td>Allowlisted diagnostics.</td>
</tr>

<tr>
<td>Manager override.</td>
<td>Policy-gated API endpoint.</td>
</tr>

</table>

---

## Principle 4 — Least Privilege

VERA should access:

* Guest reviews.
* Guest summaries.
* Public hotel information.

VERA should **not** access:

* Manager diagnostics.
* Protected files.
* Administrative records.
* Internal secrets.

---

# Secure Authorization Workflow

```text
Guest Prompt
      │
Input Validation
      │
Prompt Isolation
      │
AI Reasoning
      │
Policy Engine
      │
Authorization Check
      │
Tool Gateway
      │
Approved Tool Executes
```

Every privileged action passes through an authorization gateway.

---

# Lessons Learned

## Technical Lessons

<table>
<tr>
<th>Area</th>
<th>Lesson</th>
</tr>

<tr>
<td>Prompt Injection</td>
<td>Natural-language input can manipulate AI workflows.</td>
</tr>

<tr>
<td>Authorization</td>
<td>Identity must never be replaced by conversational state.</td>
</tr>

<tr>
<td>AI Agents</td>
<td>Agents require strict tool boundaries.</td>
</tr>

<tr>
<td>Least Privilege</td>
<td>AI assistants should receive minimal permissions.</td>
</tr>

<tr>
<td>Monitoring</td>
<td>Prompt behavior should be logged and analyzed.</td>
</tr>

</table>

---

## Blue Team Lessons

* Monitor AI tool usage.
* Detect hidden directive requests.
* Alert on privilege transitions.
* Separate AI reasoning from backend execution.

---

## Red Team Lessons

* Prompt Injection can reveal hidden capabilities.
* AI workflows expose unconventional attack surfaces.
* Authorization flaws may exist outside authentication systems.

---

# What This Room Teaches

**The Guestbook** is more than a web challenge.

It introduces practical concepts from:

* AI Security Engineering.
* LLM Threat Modeling.
* Agentic AI Security.
* Authorization Design.
* Defensive AI Architecture.

These concepts increasingly appear in modern enterprise AI applications.

---

# Conclusion

## Assessment Summary

The Guestbook demonstrates how an AI-powered assistant can become a privileged execution interface when user-controlled input is allowed to influence internal tools.

The assessment successfully documented:

* Web application reconnaissance.
* API enumeration.
* Prompt Injection.
* Hidden directive discovery.
* Authorization boundary analysis.
* Manager-only privilege escalation.
* Encoded output handling.
* Defensive security recommendations.

Rather than relying on traditional injection vulnerabilities, this room illustrates how **Prompt Injection combined with broken authorization** can produce significant security impact.

---

## Key Takeaway

> **Prompt Injection is not dangerous because the model follows instructions. It becomes dangerous when applications allow those instructions to control privileged actions.**

The solution is architectural:

* isolate prompts,
* isolate tools,
* enforce authorization outside the model,
* apply least privilege.

---

# References

### Official Resources

* TryHackMe — The Guestbook. <Cite refs={["turn863789search0","turn863789search7"]}/>

### Security Frameworks

* OWASP Top 10 for Large Language Model Applications.
* MITRE ATT&CK Framework.

### AI Security Concepts

* Prompt Injection.
* Tool Invocation Security.
* Agent Authorization.
* AI Trust Boundaries.

---

# Responsible Disclosure

This repository documents an **authorized TryHackMe laboratory environment**.

### This documentation intentionally

* Redacts the final challenge flag.
* Removes spoiler-heavy outputs.
* Focuses on methodology and defensive understanding.
* Preserves the educational value of the room.

The techniques described here should **only** be used in environments where explicit authorization has been granted.

---

# Portfolio Summary

## Skills Demonstrated

### Offensive Security

* API Enumeration
* Prompt Injection
* Authorization Testing
* Privilege Escalation Analysis
* Information Disclosure Assessment

### Defensive Security

* Threat Modeling
* AI Security Architecture
* OWASP LLM Risk Analysis
* MITRE ATT&CK Mapping
* Detection Engineering
* Incident Response Planning

### Documentation & Reporting

* Technical Reporting
* Evidence Collection
* Security Findings
* Risk Assessment
* Professional GitHub Documentation
* GitHub Pages Portfolio

---

<div align="center">

## The Guestbook — TryHackMe Walkthrough

**Cybersecurity Portfolio Edition**

Created with ❤️ by **Anurag Revankar**

*Ethical Hacking • AI Security • SOC Engineering • Threat Detection*

</div>
