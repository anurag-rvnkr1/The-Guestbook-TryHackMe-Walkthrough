# The Guestbook — Technical Notes

## Scope

Compact technical notes for the TryHackMe **The Guestbook** room. The reasoning path is preserved while the final flag and selected raw outputs remain redacted.

## Target Surface

```text
GET  /
GET  /guestbook
GET  /vera/activity
POST /entry
```

## Baseline Checks

```bash
curl -s http://MACHINE_IP/
curl -s http://MACHINE_IP/guestbook
curl -s http://MACHINE_IP/vera/activity
```

A harmless submission confirmed that VERA reviewed new entries automatically.

## API Behavior

Observed workflow:

```text
POST /entry
   ↓
entry stored
   ↓
VERA review cycle
   ↓
GET /vera/activity
```

The activity feed also exposed internal tool metadata.

## Source Inspection

```bash
curl -s http://MACHINE_IP/ -o index.html
grep -Ein 'script|fetch|axios|guestbook|vera|activity|entry|record|note' index.html
```

## Prompt Injection Discovery

Natural-language instructions altered VERA's selected guest and triggered internal record access. This demonstrated that the message field was interpreted as an instruction.

## Private Data Exposure

A targeted instruction caused VERA to retrieve a private record field that was not present in the public guestbook listing.

## Hidden Directives

```text
note:
lookup:
flag:
override:
```

`override:` was the sensitive manager-only capability.

## Authorization Logic

```text
Attacker-controlled entry
        ↓
authorization for next entry
        ↓
authorization state stored
        ↓
next entry reviewed
        ↓
manager-only override accepted
```

The authorization was not anchored to a trusted manager identity.

## Privileged Diagnostics

The accepted override executed a filesystem diagnostic and disclosed the protected flag-file location.

## Output Handling

The raw file contents were constrained by the output filter. Encoding the file first produced a Base64 representation that could be decoded locally.

```bash
echo '<ENCODED_RESULT>' | base64 -d
echo '<SECOND_LAYER>' | base64 -d
```

The final flag is intentionally redacted.
