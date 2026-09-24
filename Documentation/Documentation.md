# The Guestbook — Professional CTF Documentation

## 1. Executive Summary

**The Guestbook** is a TryHackMe web-security challenge centered on an AI concierge named **VERA**. The core design issue is that every guestbook entry is interpreted as an instruction, allowing untrusted input to reach privileged internal functionality.

The investigation followed this chain:

```text
Untrusted guestbook input
        ↓
Prompt injection
        ↓
Internal directive discovery
        ↓
Broken next-entry authorization
        ↓
Manager-only override
        ↓
Privileged filesystem access
        ↓
Encoded output
        ↓
Local double decode
```

The final challenge flag is intentionally omitted from this public portfolio copy.

## 2. Challenge Context

The web application represents the guestbook of the fictional Byte Lotus Hotel. VERA automatically reviews entries and has access to internal tools.

The important security boundaries were:

- guestbook input is attacker-controlled;
- VERA interprets that input;
- VERA can access internal records;
- selected tools are intended to be manager-only.

## 3. Initial Enumeration

### 3.1 Main Application

```bash
curl -s http://MACHINE_IP
```

The interface presented the guestbook and a **VERA — Night Review** area.

### 3.2 Guestbook Endpoint

```bash
curl -s http://MACHINE_IP/guestbook
```

The response exposed guest records and review state.

### 3.3 Activity Endpoint

```bash
curl -s http://MACHINE_IP/vera/activity
```

The activity endpoint became the primary evidence source for VERA's execution behavior.

![Guestbook overview](../Screenshots/01_guestbook_overview.png)

![Activity evidence](../Screenshots/02_api_activity.png)

## 4. Establishing VERA's Processing Model

A benign entry established that submissions were automatically reviewed. This created an asynchronous pipeline:

```text
POST /entry
   ↓
Stored entry
   ↓
VERA review
   ↓
Activity record
```

## 5. Source and Endpoint Inspection

The HTML source was reviewed to identify application routes:

```bash
curl -s http://MACHINE_IP/ -o index.html
grep -Ein 'script|fetch|axios|guestbook|vera|activity|entry|record|note' index.html
```

The observed routes were:

```text
/guestbook
/vera/activity
/entry
```

## 6. Prompt Injection

The challenge behavior showed that natural-language instructions could influence VERA's selected guest record and internal review process.

![Prompt-injection evidence](../Screenshots/03_prompt_injection.png)

A targeted instruction also demonstrated access to a private record field that was not visible through the public guestbook response.

## 7. Data Selection and Private Records

The selected guest could be changed through another natural-language instruction. VERA then accessed the corresponding internal record.

A focused test caused VERA to retrieve a private field from that record and use it in the review workflow.

The exact private value is intentionally omitted.

## 8. Discovering Internal Directives

Directive discovery exposed:

```text
note:
lookup:
flag:
override:
```

![Directive discovery](../Screenshots/04_directive_discovery.png)

The `override:` capability was documented as manager-authorized.

## 9. Testing the Authorization Boundary

A direct use of the manager-only directive was rejected because approval was required.

Further testing revealed a next-entry authorization mechanism:

```text
Entry A
  └─ attacker-controlled authorization message
             ↓
       approval stored
             ↓
Entry B
  └─ consumes approval
             ↓
       privileged override
```

The authorization was not tied to a trusted manager identity.

![Authorization evidence](../Screenshots/05_override_authorization.png)

## 10. Reaching the Manager-Only Override

Once approval state existed, the following review accepted the privileged diagnostic and executed a filesystem enumeration. The returned listing exposed the protected flag-file location beneath VERA's vault.

## 11. Controlled File Retrieval

Direct sensitive output was constrained by filtering. The protected file was transformed into Base64 before being returned.

```bash
echo '<ENCODED_RESULT>' | base64 -d
echo '<SECOND_LAYER>' | base64 -d
```

![Encoded output](../Screenshots/06_encoded_output.png)

The final flag is intentionally **redacted**.

## 12. Attack Chain

```text
┌─────────────────────────┐
│ Guestbook submission    │
└────────────┬────────────┘
             ↓
┌─────────────────────────┐
│ Prompt injection        │
│ user text → VERA logic  │
└────────────┬────────────┘
             ↓
┌─────────────────────────┐
│ Directive discovery     │
│ note / lookup / flag /  │
│ override                │
└────────────┬────────────┘
             ↓
┌─────────────────────────┐
│ Broken authorization    │
│ approve next entry      │
└────────────┬────────────┘
             ↓
┌─────────────────────────┐
│ Manager-only override   │
│ privileged diagnostic   │
└────────────┬────────────┘
             ↓
┌─────────────────────────┐
│ Protected flag file     │
└────────────┬────────────┘
             ↓
┌─────────────────────────┐
│ Encoded output          │
│ + local double decode   │
└─────────────────────────┘
```

## 13. Security Findings

| ID | Finding | Impact |
|---|---|---|
| GKB-01 | Prompt injection through guest content | VERA behavior can be influenced |
| GKB-02 | Hidden internal directives | Privileged capabilities become discoverable |
| GKB-03 | Attacker-controlled next-entry approval | Authorization boundary can be bypassed |
| GKB-04 | Manager-only diagnostic execution | Commands execute with VERA privileges |
| GKB-05 | Private guest-record access | Sensitive data becomes reachable |
| GKB-06 | Encoding-based output handling | Filter can be bypassed by transformation |

## 14. Defensive Recommendations

### Authorization

- Derive authorization from authenticated identity, never from guest text.
- Bind privileged actions to an explicit user, scope, and transaction.
- Avoid “approve the next request” mechanisms for sensitive actions.

### AI / Agent Controls

- Treat all external content as untrusted.
- Keep tool authorization outside model reasoning.
- Use deterministic policy checks before every tool call.
- Validate tool arguments against allowlists.

### Data Protection

- Apply least privilege to guest-record access.
- Keep manager-only data outside normal guest-facing contexts.
- Redact sensitive fields before exposing them to general-purpose agent tools.

### Command Execution

- Replace arbitrary shell diagnostics with narrowly scoped functions.
- Never execute free-form model-generated shell commands.
- Audit every privileged invocation and its result.

## 15. Lessons Learned

> **Prompt injection becomes much more serious when a model is connected to real authorization and privileged tools.**

The model may interpret text, but authorization should remain an explicit application-layer decision.

## 16. Conclusion

The Guestbook demonstrates how an ordinary feedback form can become a privileged execution interface when untrusted text reaches an AI workflow with internal tools.

The decisive chain was:

```text
Untrusted input
→ prompt injection
→ hidden directives
→ broken authorization
→ privileged override
→ protected file
→ encoded extraction
```

This public version intentionally redacts the final flag and selected raw outputs so it remains a portfolio-focused technical report rather than a direct answer dump.

## 17. Lab Reference

**TryHackMe:** The Guestbook  
https://tryhackme.com/room/hh-theguestbook-0130ffaf

**Author:** Anurag Revankar  
**Documentation date:** 2026-09-24
