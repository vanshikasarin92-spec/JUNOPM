# AI PRD · Juno

> Module 3 · Harness / AI PRD. The AI product requirements doc specifying all six harness surfaces, built with the **M3 · AI PRD Builder** (archetype and starting settings from the **M3 · Harness Architecture Decider**). Paste the tool's markdown over this file.

## Problem & user

_The user problem and who has it._
Customer feedback reaches Jira as noisy, duplicated and inconsistently labeled tickets. Operational bugs are mixed with feedback that points to UX gaps. PMs can’t quickly see what to fix first, why, or what is slipping.

Who has it: Product Managers (primary). Engineering leads, designers and support leads are secondary, since they receive the assigned work.
_____

## The harness

| Surface | Specification |
|---|---|
| 01 Context | _what Juno gets to see_ |
| 02 Tools | _the verb list_ |
| 03 Loop | _turn ceiling and escalation_ |
| 04 Memory | _what persists, at what scope_ |
| 05 Permissions | _read … · draft … · write … · send …_ |
| 06 Verification | _the check before output ships_ |


01 Context	Jira feedback tickets (UX enhancement and Operations epics), surveys, research decks, Confluence pages, third-party feedback platform, Jira roadmaps, sprint capacity and velocity
02 Tools	Read sources, sprint data and resolution times. Draft briefs, specs, subtasks and proposals. Write to Jira only with approval. Send email to the PM only
03 Loop	8 steps per ticket, 2 re-queries per source, stop and hand back on low confidence, conflict or source failure
04 Memory	Decision Log, PM overrides, resolution times and escalation owners persist. Raw customer quotes and identities never do
05 Permissions	read auto · draft auto · write confirm · send auto (PM only)
06 Verification	Source, confidence, severity-consistency and restricted-data checks before anything ships; failures are routed to a person, never dropped

## 01 Context · Data Requirements

- **Required sources:** _name actual systems and documents, not "internal docs."_
- **Deliberate exclusions:** _what you left out, and why._
- **SLA of truth:** _how fast a change in a source becomes a change in the answer._
- **When a source is unreachable:** _fail loudly, or degrade how?_

Required sources

Jira feedback tickets and duplicates under the two feedback epics (UX Enhancement, Operations), plus Jira roadmaps.
Sprint capacity and velocity from Jira.
Surveys and the third-party platform holding user feedback data (name the actual tool here).
Research PowerPoint decks and Confluence pages linked from tickets.
Ticket resolution time from Jira history.
Deliberate exclusions

Customer names, emails and other identities. Juno shows “Restricted.”
Revenue and ARR data. Juno estimates impact from plan tier and says so.
HR data, other teams’ projects, and any unlinked Slack or email threads.

SLA of truth

Jira tickets: a change shows up in Juno’s answer within 15 minutes.
Sprint capacity: refreshed daily and again at sprint planning.
Confluence pages, decks and surveys: refreshed daily.

When a source is unreachable

Jira down: fail loudly. Juno does not triage from stale data and tells the PM.
Secondary source down: degrade. Juno proceeds with what it has, lowers confidence to Low, names what’s missing, and sends the item to “Issue with sources.”

## 02 Tools · System Capabilities

- **The verb list:** _every function Juno may call, with its side-effect class (read / draft / write / send)._
- **Deliberate omissions:** _the tools you did not build, and why. Name at least one._
- Deliberate omissions

No tool to message customers or send to anyone but the PM, because it’s irreversible and reputational.
No tool to commit sprints or approve releases, because that is PM authority.
No tool to close, delete or merge tickets, because it’s hard to undo and Juno can only flag duplicates.
No open web search, so Juno answers only from linked, auditable sources.

## 03 Loop · AI Costs & Latency

- **Turn ceiling:** _e.g. 5 turns per request._
- **Escalation trigger:** _what makes it stop and hand back to a human._
- **Latency and cost target:** _e.g. p95 under 8s, under $0.12 per task._
- 03 Loop · AI Costs & Latency
Turn ceiling: 8 steps per ticket, and at most 2 re-queries per source.
Escalation trigger (stop and hand back to a human):
Confidence is under 70%, or two sources conflict, which sends the item to “Needs human review.”
A source still fails after 2 attempts, which sends it to “Issue with sources.”
The ticket is in an escalation category (security, accessibility, customer harm), which goes to the named owner.
The same tool fails twice in a row, or the 90s per-ticket timeout is hit.
Latency and cost target (proposed, adjust after the baseline weeks):
p95 under 60s per ticket.
A batch of 25 tickets under 5 minutes.
Under $0.15 per ticket.
Juno never loops on its own output to raise its confidence.

## 04 Memory · Data Requirements

- **What persists, at what scope:** _turn, session, user, or team._
- - Raw customer quotes, identities and contents of linked sources are never stored; Juno re-reads them each time. Unconfirmed hypotheses are not stored as facts.
- **Expiry:** _TTL per fact._
- Decision Log: kept 12 months for audit.
Overrides: 90 days as individual signals, then reduced to aggregate rates.
Resolution times: rolling 90-day window.
Escalation owners: until changed, and reviewed quarterly.
- **Write rules:** _who outranks the model when a human disagrees._

The PM outranks the model. A PM override is stored as written, and Juno never rewrites or reverses it.
Juno can’t store a fact that contradicts a PM entry.
A past Juno suggestion is not a PM decision unless the PM accepted it.

## 05 Permissions · AI Risks & Mitigations

| Side-effect class | Tier |
|---|---|
| read | _auto / confirm / blocked_ |
| draft | _auto / confirm / blocked_ |
| write | _auto / confirm / blocked_ |
| send | _auto / confirm / blocked_ |

**Justification:** _blast radius and recoverability for each non-obvious tier._
read: low blast radius. Juno sees only what the PM can see, and customer identities are masked.
draft: nothing leaves Juno. Every draft carries an AI tag, and the PM can reject it.
write (confirm): subtasks, assignments and severity changes are visible to the whole team and trigger notifications. They are recoverable but cost trust, so the PM approves each one.
send (PM only): the PM is the only recipient, so the blast radius is one person and a mistake costs one email. Juno de-duplicates alerts to avoid noise.
Blocked: committing sprints, approving releases, messaging customers, and falsifying or implying approvals. Escalation items can’t be snoozed without a written reason.

## 06 Verification · AI Testing & Measurement

- **The check before output ships:** _what is checked, against what bar._
- Every material claim has a source chip, or says “No source available.”
Facts, interpretations and hypotheses are tagged separately.
A confidence level and a “What we don’t know” line are present.
S1 and escalation items never rank below S3.
Counts reconcile (funnel total equals ticket total, capacity sums match).
No restricted customer data appears.
Spec readiness is derived from open questions.
Bar: all checks pass. Confidence below 70% means PM review.

- **Failure behaviour:** _what happens when the check fails. Silently dropping the output is never the answer._
- The check before output ships

A failed check never drops the output silently. The output is labeled “Draft: failed checks,” shows the reasons, and goes to “Needs human review.”
A fabricated source or restricted data is blocked, not shown, and logged in the Decision Log with a root-cause note.


## Eval plan

_Stub. Module 5 fills this in: the human eval rubric in `05-evals/human-rubric.md`, with its pass bar._
Stub. Module 5 fills this in. The human eval rubric will live in 05-evals/human-rubric.md.
Pass bar: at least 85% of sampled answers score 4 or higher, the average is 4.0 or higher, and every S1 and escalation answer scores 4 or higher. The rubric’s deal-breakers fail an answer outright.

## Out of scope

_Any capability not in the verb list, and any action tiered `blocked`. Both are decisions on the record, not omissions._
Anything not in the verb list is out of scope. Anything tiered blocked is out too, and both are decisions on the record:

Messaging customers or any non-PM recipient.
Committing sprints or approving releases.
Closing, deleting or merging tickets.
Open web search.
Exposing restricted customer data.
