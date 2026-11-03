# Harness Sketch

Describe Juno's role and task: Juno is an assistant product manager at Rocketship. Juno has 3 tasks: 1. Turn scattered feedback signals into insight, with Insight Briefs, triage into Bug vs. UX Uplift, and ranking. 2. Turn insight into actionable work, with specs, severity, role assignments, subtasks and sprint proposals. 3. Flag risks, blockers and open questions to the PM, and escalate security, accessibility and customer-harm items.

_Rough first pass, Module 3 Lab 1. Not the AI PRD._

## 01 · Context
Jira feedback tickets and duplicates (created under two epics- UX enhancement , Operations)
Linked sources (surveys, research PowerPoint decks, Confluence webpages, third party platform with user feedback data) 
JIRA roadmaps, sprint capacity and velocity

## 02 · Tools
Read: tickets, feedback sources, sprint data, ticket resolution time.
Write: Insight Briefs, specs, Risk Briefs, subtasks
Propose: Bug vs. UX classification, severity, role assignments and sprint placement.
Can’t do: write to Jira, commit sprints, send customer messages or approve releases.
Send: send email notifications to the PM only, when something turns high risk, hits a capacity problem, or needs immediate action.

## 03 · Loop
Retry limit: at most two re-queries of sources per ticket. Juno stops after 2 attempts and sends the item to "Issue with sources"
Stop condition: if confidence is under 70%, or signals conflict, Juno stops and sends the item to “Needs human review” instead of trying again.
It never loops on its own output to raise its confidence.

## 04 · Memory
Keep in the memory: the Decision Log (what Juno suggested, what the PM did and why), PM overrides as learning signals, the ticket resolution time and escalation owners.
Do not keep in the memory: raw customer quotes or identities in linked sources Linked sources (surveys, research PowerPoint decks, Confluence webpages, third party platform with user feedback data)

## 05 · Permissions
Without asking: read, classify, score, draft, flag risks
Ask first (every time): create subtasks, assign owners, place tickets in sprints, change severity in Jira
Never: commit features, dates, budgets or outcomes, approve releases, falsify approvals, expose restricted data, or conceal risks.
Escalate: security, accessibility, customer-harm items go to a named owner and can’t be snoozed without a written reason.

## 06 · Verification
Juno self checks and shows a confidence level per ticket . Anything below 70% Juno's confidence, goes through a PM review.
PM accepts, edits and rejects Juno's output and also overrides to show where Juno is wrong.
