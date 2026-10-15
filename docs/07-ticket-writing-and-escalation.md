# Ticket Writing, Escalation Templates and SLA Priority Matrix

A good ticket lets someone else continue the work without calling the user again. Write for the next person, who could be you in a month.

## 1. Anatomy of a good ticket

| Field | What to write |
| --- | --- |
| Short description | Symptom + affected item, for example "Outlook crashes on start, Windows 11, one user" |
| Requester and contact | Who, how to reach them, location, preferred hours |
| Details | What happened, expected behavior, exact error text, when it started, frequency |
| Scope | One user, group, location or everyone. Number of people affected |
| Impact | Can the user work? Any deadline, customer or financial impact? |
| Environment | Device name or asset tag, OS and build, app version, network (office, VPN, home) |
| Actions taken | What you tried and the result, in order |
| Resolution | Root cause (or likely cause) and what fixed it |
| Category | Consistent category and subcategory for reporting |

Habits: one issue per ticket, no passwords or personal data in notes, factual tone, timestamped updates, attach screenshots with sensitive content hidden.

## 2. Templates

### 2.1 Incident intake

```
Summary:
Requester / contact:
Location:
Device / asset tag:
OS and build / app version:
What is happening:
What should happen:
Exact error message:
When did it start / last work:
Recent changes:
Scope (just me / team / site):
Business impact:
Workaround available:
Steps already tried by user:
```

### 2.2 Work note (during troubleshooting)

```
[HH:MM] Action: <what I did>
Result: <what happened>
Next step: <what I will do or wait for>
```

### 2.3 Resolution note

```
Root cause: <known or likely cause>
Fix: <what was done>
Verified by: <user confirmed / test performed>
Prevention / follow-up: <KB article, monitoring, replacement>
Time spent: <minutes>
```

### 2.4 Request for more information

```
Hello <name>,
To continue with ticket <number>, I need:
1. <item>
2. <item>
I'll check back <time>. If it is urgent, call <service desk number>.
```

### 2.5 Escalation handoff (to Tier 2/3 or another team)

```
Escalating to: <team>
Reason for escalation: <outside my access, needs vendor, needs engineering, SLA risk>
Priority and business impact:
Summary:
Affected user(s) / device(s):
Timeline (first seen, changes):
Evidence collected: <logs, screenshots, command output, error codes>
Steps taken and results:
What I suspect and why:
What I need from you:
User expectations communicated: <what the user was told>
Contact for follow-up:
```

### 2.6 Major incident or outage notice

```
Subject: [Incident] <service> - <short status>
Status: Investigating / Identified / Monitoring / Resolved
Start time:
Impact: <who and what is affected>
Workaround:
Actions underway:
Next update: <time>
```

### 2.7 Customer-facing closure message

```
Hello <name>,
Your ticket <number> about <issue> has been resolved. <One-sentence summary of the fix.>
If the problem returns, reply to this message or contact the service desk within <n> days and we will reopen it.
```

## 3. Priority matrix (impact x urgency)

Priority is derived from **impact** (how many people and how critical) and **urgency** (how soon it must be fixed). This matrix follows common ITIL-style practice. Adjust labels and targets to the organization's policy.

| Impact \ Urgency | High (work stopped, deadline) | Medium (work degraded) | Low (minor inconvenience) |
| --- | --- | --- | --- |
| High (many users, critical service) | P1 Critical | P2 High | P3 Medium |
| Medium (a team or important user) | P2 High | P3 Medium | P4 Low |
| Low (one user) | P3 Medium | P4 Low | P4 Low |

## 4. Sample SLA targets (example only)

| Priority | Example | First response | Target resolution | Update frequency |
| --- | --- | --- | --- | --- |
| P1 Critical | Email down for the company, network outage at a site | 15 minutes | 4 hours | Every 30 minutes |
| P2 High | Department cannot access a key application | 30 minutes | 8 business hours | Every 2 hours |
| P3 Medium | One user cannot print or install an approved app | 4 business hours | 3 business days | Daily |
| P4 Low | Request for peripheral, cosmetic or how-to question | 1 business day | 5 business days | On milestones |

The times above are examples to learn from. Real targets come from the organization's service-level agreement.

## 5. Incident vs service request vs problem vs change

| Type | Meaning | Example |
| --- | --- | --- |
| Incident | Unplanned interruption or degradation | Laptop will not boot |
| Service request | Standard pre-approved ask | New monitor, software install, account access |
| Problem | Underlying cause of one or more incidents | Docks of one model keep dropping displays |
| Change | Planned modification of a service | Rolling out a new Windows build |

Link repeated incidents to a problem record, so root causes get fixed.

## 6. When to escalate

Escalate when: you lack access or authority, the SLA is at risk, a security incident is suspected, multiple users show the same fault, a vendor is needed, or you have spent the allowed time without progress. Escalating with good evidence is a strength. Hold-and-hope is not.

Security triggers to escalate immediately: a user reports a phishing click, lost or stolen device, unexpected MFA prompts, malware alerts, or requests to bypass the verification process.

## 7. Communication tips

- Acknowledge quickly, set expectations honestly, and give a time for the next update.
- Use plain language. Avoid jargon with end users, and give numbered steps.
- Do not blame the user. Explain how to avoid the problem next time.
- Close the loop: confirm the fix with the user before closing.
- Keep VIP and executive requests inside the same process, with white-glove handling, not shortcuts around identity checks.

## 8. Knowledge base article structure

Title, applies to (OS, app, version), symptom, cause, resolution steps, how to verify, when to escalate, related articles, author, review date. Templates are in the `service-desk-ticket-templates` repository.
