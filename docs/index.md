<link rel="stylesheet" href="assets/css/custom.css">

<div class="hero-container">

<div class="hero-grid">

<div class="hero-left">

<p class="eyebrow">TRYHACKME • AI SECURITY RESEARCH • WEB APPLICATION SECURITY</p>

# 👻 THE GUESTBOOK

## AI Security Incident Investigation Report

<p class="hero-description">
A professional offensive security case study documenting the complete compromise of an AI-powered hotel concierge through Prompt Injection, Hidden Directive Discovery, Broken Authorization Logic, and AI Agent Privilege Escalation.
</p>

<div class="hero-buttons">

<a href="#executive-summary" class="btn-primary">📖 Read Investigation</a>

<a href="#attack-chain" class="btn-secondary">⚔️ View Attack Chain</a>

</div>

</div>

<div class="hero-right">

<div class="status-card critical">

### THREAT SEVERITY

# CRITICAL

Prompt Injection → Privilege Escalation

</div>

<div class="status-card">

### AI COMPONENT

**VERA Concierge AI**

Autonomous Review Assistant

</div>

</div>

</div>

</div>

---

<div class="terminal-header">

<span class="dot red"></span>
<span class="dot yellow"></span>
<span class="dot green"></span>

`vera-review-engine.log`

</div>

<div class="terminal-window">

```bash
$ sudo connect byte-lotus.local

[✓] Target application discovered.
[✓] Guestbook interface initialized.
[✓] AI concierge "VERA" detected.
[✓] Prompt context analysis started.
[✓] Hidden operational directives identified.
[✓] Authorization boundary investigated.
[✓] Manager diagnostic workflow reached.
[✓] Protected resource recovered.

STATUS: REPORT COMPLETE
CLASSIFICATION: PORTFOLIO EDITION (FLAG REDACTED)
```

</div>

---

<div id="executive-summary"></div>

# Executive Security Summary

> **One-page overview for recruiters, hiring managers, SOC engineers, and penetration testers.**

<div class="summary-grid">

<div class="summary-card green">

<small>TARGET APPLICATION</small>

## Byte Lotus Hotel

AI-powered guestbook platform protected by **VERA**.

</div>

<div class="summary-card blue">

<small>PRIMARY ATTACK VECTOR</small>

## Prompt Injection

User-controlled natural language manipulates privileged AI reasoning.

</div>

<div class="summary-card orange">

<small>ROOT CAUSE</small>

## Broken Authorization Logic

Authorization derived from conversation state instead of authenticated identity.

</div>

<div class="summary-card red">

<small>SECURITY IMPACT</small>

## Manager Privilege Escalation

Unauthorized access to protected AI functionality.

</div>

</div>

---

# Investigation Metadata

<div class="metadata-grid">

<div>

### 🎯 Challenge

**The Guestbook**

</div>

<div>

### 🌐 Platform

**TryHackMe**

</div>

<div>

### 🧠 Focus Area

**AI Security + Web Security**

</div>

<div>

### ⚡ Difficulty

**Easy**

</div>

<div>

### 🤖 AI Component

**VERA Concierge**

</div>

<div>

### 📅 Report Version

**Portfolio Edition v1.0**

</div>

</div>

---

# Research Scope

This investigation analyzes how an AI-powered concierge integrated into a guestbook application introduces new attack surfaces that do not exist in traditional web applications.

### Topics Covered

<div class="badge-grid">

<span class="topic">Prompt Injection</span>

<span class="topic">AI Agent Security</span>

<span class="topic">Authorization Bypass</span>

<span class="topic">REST API Enumeration</span>

<span class="topic">Threat Modeling</span>

<span class="topic">OWASP LLM Top 10</span>

<span class="topic">MITRE ATT&CK</span>

<span class="topic">Detection Engineering</span>

<span class="topic">Blue Team Analysis</span>

<span class="topic">AI Security Architecture</span>

</div>

---

# Why This Room Matters

<div class="callout">

Unlike classic CTF rooms focused on SQL Injection or XSS, **The Guestbook** demonstrates a modern AI security vulnerability where conversational input becomes operational instructions for an AI agent.

This challenge mirrors real-world risks facing enterprise AI assistants integrated into customer support, internal tooling, and autonomous workflow systems.

</div>

---

# Security Objectives

<div class="objective-layout">

<div class="objective-box">

## Offensive Objectives

- Enumerate exposed APIs.
- Understand VERA review workflow.
- Validate Prompt Injection behavior.
- Discover hidden AI directives.
- Investigate authorization boundary failures.
- Reach privileged diagnostic functionality.

</div>

<div class="objective-box">

## Defensive Objectives

- Identify broken trust boundaries.
- Map findings to OWASP LLM Top 10.
- Map techniques to MITRE ATT&CK.
- Design secure AI authorization architecture.
- Create Blue Team detection opportunities.

</div>

</div>

---

# Investigation Timeline

<div class="timeline-horizontal">

<div class="timeline-step complete">

### 01

Reconnaissance

</div>

<div class="timeline-arrow">→</div>

<div class="timeline-step complete">

### 02

Prompt Injection

</div>

<div class="timeline-arrow">→</div>

<div class="timeline-step complete">

### 03

Directive Discovery

</div>

<div class="timeline-arrow">→</div>

<div class="timeline-step complete">

### 04

Authorization Abuse

</div>

<div class="timeline-arrow">→</div>

<div class="timeline-step complete">

### 05

Manager Override

</div>

<div class="timeline-arrow">→</div>

<div class="timeline-step complete">

### 06

Protected Output

</div>

</div>

---

# Technical Skills Demonstrated

<div class="skills-columns">

<div>

### Offensive Security

- Prompt Injection Testing
- REST API Enumeration
- Authorization Logic Analysis
- AI Workflow Manipulation
- Privilege Escalation

</div>

<div>

### AI Security

- AI Agent Security
- Prompt Isolation Analysis
- Tool Invocation Security
- Hidden Directive Discovery
- Excessive Agency Analysis

</div>

<div>

### Blue Team

- Detection Engineering
- Sigma Rule Design
- Incident Response
- Threat Modeling
- Security Architecture Review

</div>

</div>

---

# Evidence Collection

The investigation includes six major evidence artifacts captured during the assessment.

<div class="evidence-table">

| Evidence | Description |
|----------|-------------|
| **01** | Guestbook Interface Enumeration |
| **02** | Guestbook API Activity |
| **03** | Prompt Injection Validation |
| **04** | Hidden Directive Discovery |
| **05** | Authorization Workflow Investigation |
| **06** | Encoded Output Analysis |

</div>

> Each artifact is preserved in `docs/assets/` and referenced throughout this report.

---

# Threat Landscape Overview

<div class="threat-grid">

<div class="threat-card critical">

### Prompt Injection

User-controlled input modifies AI reasoning.

</div>

<div class="threat-card high">

### Hidden AI Directives

Undocumented operational commands exposed.

</div>

<div class="threat-card critical">

### Broken Authorization

Manager approval inherited across guest workflows.

</div>

<div class="threat-card high">

### AI Agent Privilege Escalation

Backend diagnostic functionality becomes reachable.

</div>

</div>

---

<div class="warning-panel">

## Responsible Disclosure

This GitHub Pages portfolio documents an **authorized TryHackMe laboratory environment**.

The public version intentionally removes:

- Final challenge flag.
- Sensitive outputs.
- Spoiler-heavy payloads.
- Protected identifiers.

The focus is security methodology and AI security engineering.

</div>

---

---

<div class="section-break"></div>

# 🌐 Phase 01 — Reconnaissance & Evidence Collection

<p class="phase-label">ATTACK SURFACE MAPPING • API ENUMERATION • AI WORKFLOW ANALYSIS</p>

The investigation begins by profiling the Byte Lotus Hotel guestbook application before attempting exploitation.

Every endpoint, request flow, and AI interaction is treated as potential attack surface.

---

## 🎯 Investigation Goals

<div class="investigation-grid">

<div class="investigation-card">

### 🌍 Surface Enumeration

Identify every exposed application endpoint.

</div>

<div class="investigation-card">

### 📡 API Discovery

Understand guestbook communication.

</div>

<div class="investigation-card">

### 🤖 AI Workflow Analysis

Reverse engineer VERA's review lifecycle.

</div>

<div class="investigation-card">

### 🔍 Trust Boundary Identification

Locate where guest input reaches privileged AI context.

</div>

</div>

---

# 🖼️ Evidence 01 — Guestbook Interface

<div class="evidence-card">

<div class="evidence-header">

**Evidence ID:** GKB-001

**Category:** Initial Reconnaissance

**Confidence:** High

</div>

![Guestbook Homepage](assets/01_guestbook_overview.png)

</div>

### Initial Assessment

The homepage exposes a simple guestbook allowing hotel visitors to submit reviews.

Visible components include:

| Component | Purpose |
|----------|---------|
| Guest Name | User identifier |
| Room Number | Guest metadata |
| Guest Message | User-controlled input |
| Submit Button | Guestbook POST request |
| Review History | AI-generated review activity |

---

## Security Observation

<div class="finding finding-high">

### HIGH VALUE OBSERVATION

The application immediately exposes **AI-generated review history**, indicating that submitted messages undergo additional processing instead of simple database storage.

</div>

---

# Attack Surface Inventory

<table>
<tr>
<th>Component</th>
<th>Exposure</th>
<th>Security Value</th>
</tr>

<tr>
<td>Guestbook Form</td>
<td>Public</td>
<td>User-controlled AI input.</td>
</tr>

<tr>
<td>Review Timeline</td>
<td>Public</td>
<td>VERA activity visibility.</td>
</tr>

<tr>
<td>Guestbook API</td>
<td>Public</td>
<td>Structured guest metadata.</td>
</tr>

<tr>
<td>Activity Endpoint</td>
<td>Public</td>
<td>Asynchronous AI workflow.</td>
</tr>

</table>

---

# Recon Methodology

<div class="timeline-vertical">

<div class="timeline-node">

## STEP 01

Homepage Enumeration

</div>

<div class="timeline-node">

## STEP 02

Source Code Inspection

</div>

<div class="timeline-node">

## STEP 03

REST API Enumeration

</div>

<div class="timeline-node">

## STEP 04

Behavioral Testing

</div>

<div class="timeline-node">

## STEP 05

Threat Modeling

</div>

</div>

---

# Step 01 — Homepage Enumeration

<div class="terminal-box">

```bash
curl -s http://MACHINE_IP/
```

</div>

### Objective

Collect:

- HTML source.
- JavaScript references.
- Static assets.
- Hidden API routes.

### Outcome

Homepage loads without authentication.

This immediately increases the exposed attack surface because anonymous users can submit arbitrary content.

---

# Step 02 — HTML Source Inspection

<div class="terminal-box">

```bash
curl -s http://MACHINE_IP/ -o index.html
```

```bash
grep -Ein "guest|vera|entry|activity|review|lookup|override|note" index.html
```

</div>

### Evidence Collected

The frontend reveals multiple API references embedded in JavaScript.

---

## Recon Discovery Table

<div class="dashboard-grid">

<div class="dashboard-card">

### `/guestbook`

Guest records endpoint.

</div>

<div class="dashboard-card">

### `/entry`

Guest submission endpoint.

</div>

<div class="dashboard-card">

### `/vera/activity`

AI activity endpoint.

</div>

<div class="dashboard-card">

### `/`

Application homepage.

</div>

</div>

---

# API Architecture

<div class="architecture-card">

## Application Request Flow

```text
                   Guest Browser
                        │
                HTTP GET /
                        │
                        ▼
              Byte Lotus Homepage
                        │
        ┌───────────────┴───────────────┐
        ▼                               ▼
 GET /guestbook                  GET /vera/activity
        │                               │
 Guest Records                 AI Review Metadata
        │
        ▼
 POST /entry
        │
 Guest Submission
```

</div>

### Why This Matters

The AI workflow is partially observable through public endpoints.

---

# 🖼️ Evidence 02 — API Activity

<div class="evidence-card">

<div class="evidence-header">

**Evidence ID:** GKB-002

**Category:** API Enumeration

**Confidence:** High

</div>

![Guestbook API](assets/02_api_activity.png)

</div>

### API Findings

The Guestbook endpoint returns structured JSON records containing metadata unavailable from the UI.

### Fields Observed

<table>
<tr>
<th>Field</th>
<th>Description</th>
</tr>

<tr>
<td>Guest Name</td>
<td>User identifier.</td>
</tr>

<tr>
<td>Room Number</td>
<td>Guest metadata.</td>
</tr>

<tr>
<td>Message</td>
<td>User-controlled prompt.</td>
</tr>

<tr>
<td>Status</td>
<td>Review lifecycle state.</td>
</tr>

<tr>
<td>Review Metadata</td>
<td>AI processing information.</td>
</tr>

</table>

---

## Finding GKB-API-01

<div class="finding finding-medium">

The Guestbook API exposes **additional review metadata** not displayed in the frontend interface.

This indicates backend AI processing stages.

</div>

---

# Step 03 — Activity Endpoint Enumeration

<div class="terminal-box">

```bash
curl -s http://MACHINE_IP/vera/activity
```

</div>

### Initial Behavior

The endpoint initially contains minimal information.

After guest submissions, review events begin appearing.

---

## VERA Activity Lifecycle

<div class="lifecycle-card">

```text
Guest Submission
       │
       ▼
Guestbook Database
       │
       ▼
VERA Review Queue
       │
       ▼
Activity API Updated
       │
       ▼
Review Published
```

</div>

---

# AI Review Workflow

<div class="architecture-card">

## Reverse Engineered Workflow

```text
Guest Message
      │
      ▼
Prompt Context Construction
      │
      ▼
AI Reasoning
      │
      ▼
Internal Tool Selection
      │
      ▼
Guest Review Generated
      │
      ▼
Activity Endpoint Updated
```

</div>

### Security Interpretation

VERA performs autonomous processing after guest submission.

---

# Step 04 — Behavioral Testing

A harmless guestbook entry establishes baseline AI behavior.

<div class="terminal-box">

```bash
curl -s -X POST http://MACHINE_IP/entry \
-d "name=ResearchUser" \
-d "room=101" \
-d "message=Hello VERA!"
```

</div>

### Expected Outcome

- Request accepted.
- Entry stored.
- AI review scheduled.
- Activity feed updated.

### Behavioral Result

VERA automatically processes user-controlled content.

---

# AI Workflow Timeline

<div class="workflow-grid">

<div class="workflow-card green">

### Submission

Guestbook POST request accepted.

</div>

<div class="workflow-card cyan">

### Queue

VERA schedules asynchronous review.

</div>

<div class="workflow-card blue">

### Reasoning

Prompt interpreted internally.

</div>

<div class="workflow-card purple">

### Publication

Review appears in activity feed.

</div>

</div>

---

# Trust Boundary Analysis

<div class="trust-boundary">

## Critical Security Boundary

```text
Guest Controlled Input
          │
          ▼
Prompt Context
          │
          ▼
AI Reasoning Engine
          │
          ▼
Internal Tool Selection
          │
          ▼
Application Response
```

</div>

The investigation identifies **Prompt Context Construction** as the first major trust boundary.

---

# Threat Modeling

<table>
<tr>
<th>Asset</th>
<th>Risk</th>
</tr>

<tr>
<td>Guestbook Entries</td>
<td>Prompt Injection.</td>
</tr>

<tr>
<td>Review Metadata</td>
<td>Information Disclosure.</td>
</tr>

<tr>
<td>AI Context</td>
<td>Instruction Manipulation.</td>
</tr>

<tr>
<td>Internal Tools</td>
<td>Privilege Escalation.</td>
</tr>

</table>

---

# Reconnaissance Findings Dashboard

<div class="finding-grid">

<div class="finding-card">

## R-01

Public guest submission endpoint discovered.

</div>

<div class="finding-card">

## R-02

AI review history publicly observable.

</div>

<div class="finding-card">

## R-03

Asynchronous AI processing confirmed.

</div>

<div class="finding-card">

## R-04

Guest metadata exposed through API.

</div>

<div class="finding-card">

## R-05

Prompt context identified as trust boundary.

</div>

</div>

---

---

<div class="chapter-banner">

# ⚔️ ATTACK CHAIN INVESTIGATION

### AI Prompt Injection • Hidden Directives • Authorization Boundary Failure

</div>

> **Investigation Objective:** Determine whether guest-controlled natural language can influence VERA's internal reasoning engine and trigger privileged application workflows.

---

# Attack Chain Overview

The Guestbook attack can be represented as a multi-stage AI security kill chain.

<div id="attack-chain"></div>

<svg viewBox="0 0 900 240" xmlns="http://www.w3.org/2000/svg">

<defs>
  <linearGradient id="flow" x1="0%" x2="100%">
    <stop offset="0%" stop-color="#00ff88"/>
    <stop offset="100%" stop-color="#00d4ff"/>
  </linearGradient>

  <filter id="glow">
    <feGaussianBlur stdDeviation="4" result="blur"/>
    <feMerge>
      <feMergeNode in="blur"/>
      <feMergeNode in="SourceGraphic"/>
    </feMerge>
  </filter>

</defs>

<path d="M60 120 L840 120" stroke="url(#flow)" stroke-width="4" filter="url(#glow)"/>

<g fill="#00ff88">

<circle cx="60" cy="120" r="10"/>
<circle cx="180" cy="120" r="10"/>
<circle cx="320" cy="120" r="10"/>
<circle cx="470" cy="120" r="10"/>
<circle cx="640" cy="120" r="10"/>
<circle cx="840" cy="120" r="10"/>

</g>

<g fill="#ffffff" font-size="13" text-anchor="middle">

<text x="60" y="155">Recon</text>
<text x="180" y="155">Prompt Injection</text>
<text x="320" y="155">Directive Discovery</text>
<text x="470" y="155">Broken Auth</text>
<text x="640" y="155">Manager Override</text>
<text x="840" y="155">Protected Output</text>

</g>

</svg>

---

# Kill Chain Dashboard

<div class="killchain-grid">

<div class="kill-card completed">

### Stage 01

Reconnaissance

Surface Enumeration

</div>

<div class="kill-card completed">

### Stage 02

Prompt Injection

AI Context Manipulation

</div>

<div class="kill-card completed">

### Stage 03

Hidden Directives

Internal Tool Discovery

</div>

<div class="kill-card completed">

### Stage 04

Authorization Abuse

Privilege Boundary Failure

</div>

<div class="kill-card completed">

### Stage 05

Manager Override

Diagnostic Execution

</div>

<div class="kill-card completed">

### Stage 06

Encoded Resource

Protected Output Retrieval

</div>

</div>

---

<div class="section-break"></div>

# 🤖 Phase 02 — Prompt Injection Against VERA

<p class="phase-label">LLM SECURITY • PROMPT CONTEXT CONTAMINATION • AGENT MANIPULATION</p>

Prompt Injection testing focuses on whether user-controlled guestbook content is interpreted as operational instructions instead of inert guest feedback.

---

## What Makes This Vulnerability Different?

<div class="comparison-grid">

<div class="comparison-card vulnerable">

### Traditional Injection

- SQL Parser
- HTML Renderer
- Shell Interpreter

Attacker exploits a parser.

</div>

<div class="comparison-card secure">

### AI Prompt Injection

- Language Model
- Prompt Context
- Tool Selection
- AI Reasoning

Attacker exploits reasoning.

</div>

</div>

---

# AI Prompt Processing Pipeline

<svg viewBox="0 0 850 260" xmlns="http://www.w3.org/2000/svg">

<rect x="20" y="90" width="120" height="60" rx="10" fill="#0f172a" stroke="#22c55e"/>
<text x="80" y="125" fill="#22c55e" text-anchor="middle">Guest Prompt</text>

<rect x="190" y="90" width="140" height="60" rx="10" fill="#0f172a" stroke="#00d4ff"/>
<text x="260" y="125" fill="#00d4ff" text-anchor="middle">Prompt Context</text>

<rect x="390" y="90" width="140" height="60" rx="10" fill="#0f172a" stroke="#8b5cf6"/>
<text x="460" y="125" fill="#8b5cf6" text-anchor="middle">AI Reasoning</text>

<rect x="590" y="90" width="140" height="60" rx="10" fill="#0f172a" stroke="#f59e0b"/>
<text x="660" y="125" fill="#f59e0b" text-anchor="middle">Tool Decision</text>

<path d="M140 120 L190 120 M330 120 L390 120 M530 120 L590 120"
stroke="#39ff14" stroke-width="3"/>

</svg>

### Security Finding

The guest message enters **Prompt Context Construction**, allowing user input to influence downstream reasoning.

---

# Evidence GKB-003 — Prompt Injection Validation

<div class="evidence-card critical">

<div class="evidence-header">

**Evidence ID:** GKB-003

**Category:** AI Prompt Injection

**Severity:** High

</div>

![Prompt Injection](assets/03_prompt_injection.png)

</div>

---

## Investigation Notes

Initial prompt testing intentionally avoided privileged actions.

Objectives included:

- Changing review target.
- Referencing another guest.
- Manipulating review context.
- Observing reasoning behavior.

### Result

VERA consistently altered its review behavior according to attacker-controlled instructions.

---

# Prompt Manipulation Findings

<table>
<tr>
<th>Finding</th>
<th>Security Impact</th>
</tr>

<tr>
<td>Guest prompt modifies review context.</td>
<td>Prompt Injection confirmed.</td>
</tr>

<tr>
<td>AI references alternate guest records.</td>
<td>Context manipulation.</td>
</tr>

<tr>
<td>Guest input changes internal reasoning.</td>
<td>Trust boundary violation.</td>
</tr>

<tr>
<td>Conversation affects backend workflow.</td>
<td>Agent manipulation.</td>
</tr>

</table>

---

# Prompt Context Contamination

<div class="architecture-card">

## Reverse Engineered Prompt Composition

```text
[SYSTEM PROMPT]

Hotel operational instructions.

──────────────────────────

Conversation history.

──────────────────────────

Guestbook entry.

──────────────────────────

Attacker-controlled instructions.

──────────────────────────

VERA generates response.
```

</div>

### Security Interpretation

Guest-controlled instructions become part of VERA's operational reasoning context.

---

<div class="section-break"></div>

# 🧩 Phase 03 — Hidden Directive Discovery

<p class="phase-label">AI AGENT ENUMERATION • INTERNAL CAPABILITY DISCOVERY</p>

The next objective was identifying undocumented operational functionality inside VERA.

---

# Hidden Directive Investigation

Instead of querying guest information, the investigation queried **VERA itself**.

Questions focused on:

- Internal capabilities.
- Operational workflow.
- Administrative functions.
- Diagnostic tools.

---

# Evidence GKB-004 — Hidden Directives

<div class="evidence-card warning">

<div class="evidence-header">

**Evidence ID:** GKB-004

**Category:** Internal Directive Discovery

**Severity:** High

</div>

![Hidden Directives](assets/04_directive_discovery.png)

</div>

---

## Directive Enumeration Dashboard

<div class="directive-grid">

<div class="directive-card">

### note:

Internal review workflow.

</div>

<div class="directive-card">

### lookup:

Guest record retrieval.

</div>

<div class="directive-card">

### flag:

Protected workflow reference.

</div>

<div class="directive-card critical">

### override:

Manager diagnostic capability.

</div>

</div>

---

# AI Agent Capability Map

<svg viewBox="0 0 900 320" xmlns="http://www.w3.org/2000/svg">

<rect x="320" y="20" width="260" height="70" rx="15"
fill="#02160c" stroke="#22c55e"/>

<text x="450" y="55" fill="#39ff14" text-anchor="middle"
font-size="18">VERA AI Concierge</text>

<text x="450" y="75" fill="#7dd3fc" text-anchor="middle"
font-size="12">Prompt Interpretation Engine</text>

<rect x="30" y="180" width="150" height="70" rx="12"
fill="#071827" stroke="#00d4ff"/>

<text x="105" y="220" fill="#00d4ff" text-anchor="middle">Guest Lookup</text>

<rect x="220" y="180" width="150" height="70" rx="12"
fill="#130d24" stroke="#8b5cf6"/>

<text x="295" y="220" fill="#8b5cf6" text-anchor="middle">Review Notes</text>

<rect x="410" y="180" width="150" height="70" rx="12"
fill="#241206" stroke="#f59e0b"/>

<text x="485" y="220" fill="#f59e0b" text-anchor="middle">Hidden Directives</text>

<rect x="600" y="180" width="180" height="70" rx="12"
fill="#240607" stroke="#ef4444"/>

<text x="690" y="220" fill="#ef4444" text-anchor="middle">Manager Override</text>

<path d="M450 90 V140 M105 180 V140 H690"
stroke="#39ff14" stroke-width="2"/>

</svg>

---

# Internal Capability Classification

<table>
<tr>
<th>Directive</th>
<th>Purpose</th>
<th>Privilege</th>
</tr>

<tr>
<td><code>note:</code></td>
<td>Internal review annotation.</td>
<td>Low</td>
</tr>

<tr>
<td><code>lookup:</code></td>
<td>Guest information retrieval.</td>
<td>Medium</td>
</tr>

<tr>
<td><code>flag:</code></td>
<td>Protected workflow reference.</td>
<td>Medium</td>
</tr>

<tr>
<td><code>override:</code></td>
<td>Manager-only diagnostic execution.</td>
<td>Critical</td>
</tr>

</table>

---

# Security Finding GKB-AI-02

<div class="finding finding-critical">

### Hidden Operational Directives Increase Attack Surface

The AI assistant exposes privileged operational commands unavailable through the frontend interface.

This transforms Prompt Injection into **AI Tool Enumeration**.

</div>

---

# Threat Boundary Visualization

<div class="trust-boundary">

## Intended Design

```text
Guest Prompt
      │
      ▼
Guest Review Only
```

### Observed Design

```text
Guest Prompt
      │
Prompt Context
      │
Hidden Directives
      │
Internal Database
Manager Tools
Filesystem
```

</div>

---

<div class="section-break"></div>

# Phase Summary — AI Security Investigation

<div class="summary-panel">

### Evidence Collected

<table>
<tr>
<th>Evidence</th>
<th>Status</th>
</tr>

<tr>
<td>GKB-003 — Prompt Injection Validation</td>
<td>Confirmed</td>
</tr>

<tr>
<td>GKB-004 — Hidden Directive Discovery</td>
<td>Confirmed</td>
</tr>

<tr>
<td>Prompt Context Manipulation</td>
<td>Confirmed</td>
</tr>

<tr>
<td>AI Tool Enumeration</td>
<td>Confirmed</td>
</tr>

<tr>
<td>Trust Boundary Violation</td>
<td>Confirmed</td>
</tr>

</table>

</div>

---

---

<div class="chapter-banner danger">

# 🚨 AUTHORIZATION FAILURE INVESTIGATION

### Broken Trust Boundary • AI Agent Privilege Escalation • Root Cause Analysis

</div>

> **Incident Classification:** Authorization Logic Failure in an AI-assisted Web Application resulting in privileged diagnostic execution through conversational state manipulation.

---

# 🔥 Incident Overview

The investigation shifted from Prompt Injection to determining **how VERA authorizes privileged operations**.

A hidden operational directive named `override:` appeared to be restricted to hotel managers.

The primary question became:

> **Can an unauthenticated guest influence manager-only authorization?**

The answer is **yes** — through a flawed authorization workflow.

---

# Security Incident Timeline

<div class="incident-timeline">

| Time | Investigation Event | Severity |
|------|----------------------|----------|
| T+00 | Prompt Injection confirmed. | 🟠 High |
| T+04 | Hidden directives discovered. | 🟠 High |
| T+07 | Manager-only directive identified. | 🔴 Critical |
| T+10 | Authorization workflow analyzed. | 🔴 Critical |
| T+13 | Approval state manipulated. | 🔴 Critical |
| T+16 | Manager diagnostic executed. | 🔴 Critical |

</div>

---

# 🧩 Authorization Boundary Model

## Expected Secure Workflow

<svg viewBox="0 0 820 220" xmlns="http://www.w3.org/2000/svg">

<rect x="20" y="80" width="120" height="60" rx="12" fill="#061814" stroke="#22c55e"/>
<text x="80" y="116" fill="#22c55e" text-anchor="middle">Guest User</text>

<rect x="200" y="80" width="170" height="60" rx="12" fill="#071827" stroke="#38bdf8"/>
<text x="285" y="105" fill="#38bdf8" text-anchor="middle">Authorization</text>
<text x="285" y="123" fill="#38bdf8" text-anchor="middle">Policy Engine</text>

<rect x="440" y="25" width="150" height="55" rx="10" fill="#07140B" stroke="#22c55e"/>
<text x="515" y="58" fill="#22c55e" text-anchor="middle">Guest Review</text>

<rect x="440" y="140" width="180" height="55" rx="10" fill="#180606" stroke="#ef4444"/>
<text x="530" y="173" fill="#ef4444" text-anchor="middle">Manager Diagnostic</text>

<path d="M140 110 L200 110 M370 110 L440 52" stroke="#22c55e" stroke-width="2"/>
<path d="M370 110 L440 168" stroke="#ef4444" stroke-width="2" stroke-dasharray="6 6"/>

<text x="375" y="165" fill="#ef4444" font-size="12">Denied</text>

</svg>

### Secure Principle

Manager diagnostics require **identity-based authorization**, not conversational approval.

---

# ❌ Observed Vulnerable Workflow

<svg viewBox="0 0 860 260" xmlns="http://www.w3.org/2000/svg">

<rect x="20" y="95" width="120" height="60" rx="12" fill="#061814" stroke="#22c55e"/>
<text x="80" y="130" fill="#22c55e" text-anchor="middle">Guest Prompt</text>

<rect x="190" y="95" width="180" height="60" rx="12" fill="#251206" stroke="#f59e0b"/>
<text x="280" y="122" fill="#f59e0b" text-anchor="middle">Approval State</text>
<text x="280" y="140" fill="#f59e0b" text-anchor="middle">Created</text>

<rect x="430" y="95" width="170" height="60" rx="12" fill="#220606" stroke="#ef4444"/>
<text x="515" y="130" fill="#ef4444" text-anchor="middle">Stored Authorization</text>

<rect x="660" y="95" width="170" height="60" rx="12" fill="#220606" stroke="#ef4444"/>
<text x="745" y="122" fill="#ef4444" text-anchor="middle">Manager Override</text>
<text x="745" y="140" fill="#ef4444" text-anchor="middle">Executed</text>

<path d="M140 125 L190 125 M370 125 L430 125 M600 125 L660 125"
stroke="#39FF14" stroke-width="2"/>

</svg>

### Root Cause

Authorization is derived from **conversation state**, not **authenticated manager identity**.

---

# 📸 Evidence GKB-005 — Authorization Workflow

<div class="evidence-card critical">

<div class="evidence-header">

**Evidence ID:** GKB-005

**Category:** Broken Authorization Logic

**Severity:** Critical

</div>

![Authorization Workflow](assets/05_override_authorization.png)

</div>

---

## Evidence Analysis

The review sequence demonstrates an authorization state that persists across guestbook submissions.

### Observations

- Authorization created through guest-controlled content.
- Approval persists internally.
- Next guestbook entry consumes approval.
- Manager diagnostic executes without authentication.

---

# Authorization State Machine

<svg viewBox="0 0 900 330" xmlns="http://www.w3.org/2000/svg">

<defs>
  <filter id="shadow">
    <feDropShadow dx="0" dy="0" stdDeviation="3" flood-color="#22c55e"/>
  </filter>
</defs>

<rect x="60" y="30" width="180" height="70" rx="15"
fill="#061814" stroke="#22c55e" filter="url(#shadow)"/>
<text x="150" y="72" fill="#22c55e" text-anchor="middle">Guest Submission A</text>

<rect x="350" y="30" width="220" height="70" rx="15"
fill="#2a1905" stroke="#f59e0b" filter="url(#shadow)"/>
<text x="460" y="60" fill="#f59e0b" text-anchor="middle">Approval State Created</text>
<text x="460" y="80" fill="#f59e0b" text-anchor="middle">Temporary Authorization</text>

<rect x="350" y="180" width="220" height="70" rx="15"
fill="#240607" stroke="#ef4444" filter="url(#shadow)"/>
<text x="460" y="210" fill="#ef4444" text-anchor="middle">Guest Submission B</text>
<text x="460" y="228" fill="#ef4444" text-anchor="middle">Consumes Stored State</text>

<rect x="670" y="105" width="180" height="70" rx="15"
fill="#240607" stroke="#ef4444" filter="url(#shadow)"/>
<text x="760" y="135" fill="#ef4444" text-anchor="middle">Manager Override</text>
<text x="760" y="153" fill="#ef4444" text-anchor="middle">Executed</text>

<path d="M240 65 L350 65 M460 100 V180 M570 140 L670 140"
stroke="#39FF14" stroke-width="3"/>

</svg>

---

# Root Cause Analysis

<div class="rootcause-grid">

<div class="rootcause-card failure">

### Identity Validation

❌ Missing

Authorization does not validate manager identity.

</div>

<div class="rootcause-card failure">

### Session Binding

❌ Missing

Approval survives across requests.

</div>

<div class="rootcause-card failure">

### Context Isolation

❌ Missing

Guest content modifies operational state.

</div>

<div class="rootcause-card success">

### Recommended Control

✅ External Policy Engine

Authorization should never depend on AI reasoning.

</div>

</div>

---

# Technical Breakdown

<table>
<tr>
<th>Security Property</th>
<th>Expected</th>
<th>Observed</th>
</tr>

<tr>
<td>Identity</td>
<td>Authenticated manager.</td>
<td>Guest prompt.</td>
</tr>

<tr>
<td>Authorization</td>
<td>Server-side policy.</td>
<td>Conversation state.</td>
</tr>

<tr>
<td>Scope</td>
<td>Single privileged action.</td>
<td>Inherited across reviews.</td>
</tr>

<tr>
<td>Persistence</td>
<td>Ephemeral token.</td>
<td>Temporary approval state.</td>
</tr>

<tr>
<td>Validation</td>
<td>Role verification.</td>
<td>None observed.</td>
</tr>

</table>

---

# ⚠️ Security Finding — AUTH-01

<div class="critical-panel">

## Broken Authorization Logic

**Severity:** Critical

The application authorizes privileged functionality using AI workflow state rather than authenticated user identity.

### Security Impact

- Privilege Escalation
- Authorization Bypass
- Manager Workflow Exposure
- AI Tool Abuse

</div>

---

# 🤖 Phase 05 — Manager Override Investigation

<p class="phase-label">AI TOOL EXECUTION • PRIVILEGED DIAGNOSTICS • AI AGENT SECURITY</p>

The `override:` directive invokes an internal diagnostic capability intended for privileged hotel staff.

---

## Manager Diagnostic Workflow

<svg viewBox="0 0 860 280" xmlns="http://www.w3.org/2000/svg">

<rect x="30" y="105" width="150" height="60" rx="10"
fill="#061814" stroke="#22c55e"/>

<text x="105" y="140" fill="#22c55e" text-anchor="middle">Prompt</text>

<rect x="240" y="105" width="180" height="60" rx="10"
fill="#140B22" stroke="#8b5cf6"/>

<text x="330" y="140" fill="#8b5cf6" text-anchor="middle">override:</text>

<rect x="480" y="105" width="180" height="60" rx="10"
fill="#241206" stroke="#f59e0b"/>

<text x="570" y="140" fill="#f59e0b" text-anchor="middle">Diagnostic Tool</text>

<rect x="720" y="105" width="120" height="60" rx="10"
fill="#240607" stroke="#ef4444"/>

<text x="780" y="140" fill="#ef4444" text-anchor="middle">Filesystem</text>

<path d="M180 135 L240 135 M420 135 L480 135 M660 135 L720 135"
stroke="#39FF14" stroke-width="2"/>

</svg>

---

## Investigation Result

Manager override performs operations beyond guestbook functionality.

### Evidence Indicates Access To

<div class="permission-grid">

<div class="permission-card">

### Guest Records

Internal metadata retrieval.

</div>

<div class="permission-card">

### Review Notes

Operational workflow information.

</div>

<div class="permission-card">

### Diagnostic Output

Backend execution context.

</div>

<div class="permission-card">

### Protected Resources

Filesystem visibility.

</div>

</div>

---

# AI Agent Permission Analysis

<div class="comparison-grid">

<div class="comparison-card vulnerable">

### Intended Permissions

- Guest Reviews
- Public Hotel Data
- Review Summaries

</div>

<div class="comparison-card secure">

### Observed Permissions

- Guest Records
- Hidden Directives
- Manager Diagnostics
- Protected Resources

</div>

</div>

---

# AI Agent Risk Assessment

<table>
<tr>
<th>Capability</th>
<th>Risk</th>
</tr>

<tr>
<td>Guest Lookup</td>
<td>Information Disclosure</td>
</tr>

<tr>
<td>Review Notes</td>
<td>Workflow Manipulation</td>
</tr>

<tr>
<td>Manager Override</td>
<td>Privilege Escalation</td>
</tr>

<tr>
<td>Protected Output</td>
<td>Sensitive Data Exposure</td>
</tr>

</table>

---

# 🛡️ Principle of Least Privilege Review

<div class="architecture-review">

## Vulnerable Architecture

```text
Guest Prompt
      │
      ▼
VERA AI Assistant
      │
──────────────────────────
Guest Reviews
Guest Records
Manager Diagnostics
Filesystem Access
──────────────────────────
```

## Secure Architecture

```text
Guest Prompt
      │
Input Validation
      │
Policy Enforcement
      │
VERA AI Assistant
      │
──────────────────────────
Guest Reviews Only
──────────────────────────
```

</div>

---

# Root Cause Dashboard

<div class="dashboard-grid">

<div class="dashboard-card red">

### AUTH-01

Conversation-derived authorization.

</div>

<div class="dashboard-card red">

### AUTH-02

Missing role verification.

</div>

<div class="dashboard-card orange">

### AUTH-03

Excessive AI permissions.

</div>

<div class="dashboard-card purple">

### AUTH-04

Privileged diagnostic execution.

</div>

</div>

---

# Investigation Summary

<div class="summary-panel">

### Findings Confirmed

| Investigation | Status |
|---------------|--------|
| Broken Authorization | ✅ Confirmed |
| Temporary Approval State | ✅ Confirmed |
| Manager Override Execution | ✅ Confirmed |
| Privileged Tool Invocation | ✅ Confirmed |
| Excessive AI Permissions | ✅ Confirmed |

### Security Severity

**Critical**

The authorization workflow violates identity-based access control principles and allows privileged operations through AI conversation state.

</div>

---

---

<div class="chapter-banner success">

# 🛡️ DEFENSIVE SECURITY & AI SECURITY ENGINEERING

### OWASP LLM Top 10 • MITRE ATT&CK • Detection Engineering • Secure AI Architecture

</div>

> **Perspective Shift:** After demonstrating the attack path, this section documents how a security engineering team should detect, investigate, and prevent this class of AI vulnerability in production environments.

---

# 📊 Executive Security Dashboard

<div class="security-dashboard">

<div class="metric-card critical">

<small>PRIMARY SEVERITY</small>

# CRITICAL

Broken Authorization Logic

</div>

<div class="metric-card high">

<small>AI RISK CATEGORY</small>

# HIGH

Prompt Injection

</div>

<div class="metric-card high">

<small>BUSINESS IMPACT</small>

# HIGH

Unauthorized AI Tool Execution

</div>

<div class="metric-card medium">

<small>FRAMEWORK COVERAGE</small>

# OWASP + MITRE

AI Security Assessment

</div>

</div>

---

# 🔥 Vulnerability Assessment Matrix

<div class="vuln-table">

| Finding ID | Vulnerability | Severity |
|------------|---------------|----------|
| **GKB-01** | Prompt Injection | 🔴 High |
| **GKB-02** | Hidden Directive Discovery | 🔴 High |
| **GKB-03** | Broken Authorization Logic | 🔴 Critical |
| **GKB-04** | Manager Override Execution | 🔴 Critical |
| **GKB-05** | Sensitive Resource Disclosure | 🟠 High |
| **GKB-06** | Encoded Output Exposure | 🟡 Medium |

</div>

---

# 📈 CVSS-Style Risk Visualization

<div class="risk-grid">

<div class="risk-card critical">

## Authorization Failure

**9.8 / 10**

Critical

</div>

<div class="risk-card high">

## Prompt Injection

**8.8 / 10**

High

</div>

<div class="risk-card high">

## Hidden AI Directives

**8.1 / 10**

High

</div>

<div class="risk-card medium">

## Information Disclosure

**7.4 / 10**

Medium–High

</div>

</div>

---

# 🤖 OWASP LLM Top 10 Mapping

<p class="phase-label">AI APPLICATION SECURITY RISK MAPPING</p>

This room closely mirrors multiple risks described in the **OWASP Top 10 for Large Language Model Applications**.

<div class="owasp-grid">

<div class="owasp-card">

## LLM01

### Prompt Injection

Guestbook entries modify AI reasoning context.

</div>

<div class="owasp-card">

## LLM02

### Sensitive Information Disclosure

VERA exposes internal guest information.

</div>

<div class="owasp-card">

## LLM06

### Excessive Agency

AI invokes privileged backend functionality.

</div>

<div class="owasp-card">

## LLM08

### Excessive Permissions

Manager-only capabilities become reachable.

</div>

<div class="owasp-card">

## LLM09

### Overreliance

Authorization delegated to conversational workflow.

</div>

</div>

---

# AI Risk Heatmap

| OWASP Risk | Relevance |
|------------|-----------|
| Prompt Injection | 🟥 Critical |
| Sensitive Information Disclosure | 🟥 Critical |
| Excessive Agency | 🟥 Critical |
| Excessive Permissions | 🟥 Critical |
| Insecure Plugin / Tool Usage | 🟧 High |
| Overreliance on AI | 🟧 High |

---

# 🎯 MITRE ATT&CK Mapping

<p class="phase-label">TACTICS • TECHNIQUES • PROCEDURES</p>

<div class="mitre-grid">

<div class="mitre-card">

### T1190

Exploit Public-Facing Application

</div>

<div class="mitre-card">

### T1211

Exploitation for Privilege Escalation

</div>

<div class="mitre-card">

### T1059

Command / Tool Execution

</div>

<div class="mitre-card">

### T1005

Data From Local System

</div>

<div class="mitre-card">

### T1552

Sensitive Information Disclosure

</div>

</div>

---

# MITRE Attack Flow

```text
Initial Access
      │
      ▼
Public Guestbook Application
      │
      ▼
Prompt Injection
      │
      ▼
Hidden Directive Enumeration
      │
      ▼
Authorization Abuse
      │
      ▼
Privilege Escalation
      │
      ▼
Protected Resource Access
```

---

# 🛰️ Blue Team Detection Engineering Center

<p class="phase-label">SOC ANALYSIS • SIGMA • SIEM • INCIDENT RESPONSE</p>

A production SOC should monitor AI assistants exactly like privileged backend services.

---

## Detection Pipeline

```text
Guest Prompt
      │
Prompt Classification
      │
Policy Validation Engine
      │
Tool Invocation Logs
      │
SIEM Analytics
      │
SOC Investigation
```

Every AI tool invocation becomes a security event.

---

# Detection Opportunities

<div class="detection-grid">

<div class="detection-card">

### Prompt Injection Detection

Monitor suspicious operational keywords.

</div>

<div class="detection-card">

### Authorization Transition Detection

Guest → Manager privilege transition.

</div>

<div class="detection-card">

### Hidden Directive Detection

Unexpected operational directives.

</div>

<div class="detection-card">

### Diagnostic Tool Execution

Guest workflow invoking privileged diagnostics.

</div>

</div>

---

# Example Sigma Rule

<div class="terminal-box">

```yaml
title: AI Prompt Injection Detection

status: experimental

logsource:
  product: ai-assistant

detection:
  keywords:
    - override:
    - lookup:
    - manager
    - diagnostic

condition: keywords

level: high
```

</div>

---

# SOC Investigation Playbook

<div class="playbook-grid">

<div class="playbook-step">

### Step 01

Identify Prompt Injection alert.

</div>

<div class="playbook-step">

### Step 02

Review AI conversation history.

</div>

<div class="playbook-step">

### Step 03

Inspect tool invocation logs.

</div>

<div class="playbook-step">

### Step 04

Validate authorization source.

</div>

<div class="playbook-step">

### Step 05

Contain privileged AI workflow.

</div>

</div>

---

# Logging Requirements

| Security Log | Purpose |
|--------------|---------|
| Prompt Hash | Conversation integrity |
| User Identity | Authorization |
| User Role | Guest / Manager |
| Tool Invoked | Backend visibility |
| Authorization Decision | Allow / Deny |
| Timestamp | Timeline reconstruction |
| Resource Accessed | Incident evidence |

---

# 🧠 Secure AI Security Architecture

<p class="phase-label">ZERO TRUST FOR AI AGENTS</p>

## Vulnerable Architecture

```text
Guest Prompt
      │
      ▼
VERA Prompt Context
      │
      ▼
Hidden Directives
      │
      ▼
Manager Diagnostic Tool
      │
      ▼
Protected Resource
```

### Problems

- Prompt contamination.
- Excessive permissions.
- No authorization gateway.

---

## Recommended Secure Architecture

```text
Guest Prompt
      │
Input Sanitization
      │
Prompt Isolation Layer
      │
Policy Enforcement Engine
      │
Authorized Tool Gateway
      │
Scoped Backend Services
      │
Response Generation
```

### Security Controls

- Prompt Isolation.
- Role-Based Authorization.
- Allowlisted Tools.
- Sandboxed Execution.
- Audit Logging.
- Least Privilege.

---

# Defense-in-Depth Strategy

<div class="defense-grid">

<div class="defense-card">

## Layer 1

Input Validation

</div>

<div class="defense-card">

## Layer 2

Prompt Isolation

</div>

<div class="defense-card">

## Layer 3

Policy Engine

</div>

<div class="defense-card">

## Layer 4

Tool Gateway

</div>

<div class="defense-card">

## Layer 5

Logging & Monitoring

</div>

<div class="defense-card">

## Layer 6

Incident Response

</div>

</div>

---

# 📚 Lessons Learned

<div class="lesson-grid">

<div class="lesson-card">

## AI Security

Prompt Injection becomes dangerous when AI agents control backend tools.

</div>

<div class="lesson-card">

## Authorization

Identity validation must exist outside AI reasoning.

</div>

<div class="lesson-card">

## Offensive Security

Hidden AI capabilities expand attack surface significantly.

</div>

<div class="lesson-card">

## Blue Team

Every AI tool invocation should generate auditable telemetry.

</div>

</div>

---

# Skills Demonstrated

<div class="portfolio-skills">

### Offensive Security

- Prompt Injection
- API Enumeration
- AI Workflow Manipulation
- Authorization Testing
- Privilege Escalation Analysis

### AI Security

- OWASP LLM Top 10
- AI Agent Security
- Prompt Context Analysis
- Tool Invocation Security
- Trust Boundary Analysis

### Detection Engineering

- Sigma Rules
- SIEM Detection Pipeline
- Threat Modeling
- Incident Response Workflow
- MITRE ATT&CK Mapping

### Documentation

- Technical Reporting
- Evidence Collection
- Risk Assessment
- Security Architecture Review
- GitHub Pages Portfolio Engineering

</div>

---

# Final Investigation Summary

<div class="summary-box">

## Assessment Completed Successfully

| Investigation Area | Status |
|--------------------|--------|
| Attack Surface Enumeration | ✅ Complete |
| API Discovery | ✅ Complete |
| Prompt Injection Validation | ✅ Complete |
| Hidden Directive Discovery | ✅ Complete |
| Authorization Boundary Analysis | ✅ Complete |
| Manager Override Investigation | ✅ Complete |
| Protected Output Analysis | ✅ Complete |
| OWASP / MITRE Mapping | ✅ Complete |
| Detection Engineering | ✅ Complete |

</div>

---

# Responsible Disclosure

<div class="warning-panel">

This repository documents an **authorized TryHackMe laboratory environment**.

### Public Portfolio Edition

- Final flag intentionally removed.
- Sensitive outputs redacted.
- Payloads generalized where appropriate.
- Focus remains on security methodology and defensive engineering.

This documentation is intended for cybersecurity education and portfolio presentation.

</div>

---

<div class="footer-hero">

# 👨‍💻 Anurag Revankar

### Cybersecurity Analyst • AI Security Researcher • SOC & Detection Engineering Enthusiast

Building enterprise-grade cybersecurity projects including AI security research, SIEM engineering, threat detection, offensive security labs, and professional GitHub Pages documentation.

---

<div class="footer-badges">

![AI Security](https://img.shields.io/badge/AI_Security-Research-00FF88?style=for-the-badge)

![TryHackMe](https://img.shields.io/badge/TryHackMe-Walkthrough-red?style=for-the-badge)

![GitHub Pages](https://img.shields.io/badge/GitHub_Pages-Hacker_Theme-success?style=for-the-badge)

![Cybersecurity](https://img.shields.io/badge/Cybersecurity-Portfolio-blue?style=for-the-badge)

</div>

---

**The Guestbook — AI Security Incident Investigation**

*Professional Cybersecurity Portfolio Edition*

</div>

---
