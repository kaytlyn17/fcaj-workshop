---
title: "AWS Cloud & AI Day Hanoi 2026"
linkTitle: "AWS Cloud & AI Day"
date: 2026-09-29
weight: 2
chapter: false
---

**When:** 29 September 2026, 09:00–17:00 (Vietnam time)  
**Venue:** Bitexco Financial Tower, Ho Chi Minh City 
**Role:** Attendee


## Taking an AI application beyond a demo

I came to the event with a security question. Once an AI application can call tools on someone’s behalf, how can an operator establish what data it used, what it was allowed to do and who was responsible for the outcome? Dr. Werner Vogels’ keynote and contributions from Eric Yeo, Mai Le and Dr. Jens Lottner put that question in the context of enterprise modernisation. Examples from Katalon and Vietnamese financial institutions made me think about the people who inherit an application after the demo.

## Three ideas I took away

- **Repeatable deployment.** The sessions on GitOps and internal developer platforms showed why teams invest in a standard route to production. With configuration in version control, an operator can look up who made a change and how it was reviewed.
- **Workload identity.** The IAM and AWS STS discussions connected the lifetime of a session to the permissions it carries. When an agent reads data or calls a tool, an operator needs to be able to trace the action to the identity that performed it.
- **Answer quality.** Latency and error rates help establish whether an application is serving traffic. For AI, I would also want to know which sources support an answer, whether the output exposes sensitive data and how many tokens the request consumed.

## A connection to PhishGuard

When presenting one incident, I can show the cited playbook passage, explain which policy rule proposed the action and identify who approved it. Together, those details let a reviewer trace the evidence behind the explanation, the basis for the decision and the record left afterwards.

![Event Picture]({{< asset "images/event_cloud.jpg" >}})
![Event Picture]({{< asset "images/event_cloud2.jpg" >}})
<!-- Optional photo: agenda or slide from a session you attended. -->
