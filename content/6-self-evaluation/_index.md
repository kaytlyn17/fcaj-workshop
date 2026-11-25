---
title: "6. Self-assessment"
weight: 6
chapter: false
---

In my first IAM, VPC and EC2 labs, a successful connection to an instance usually marked the end of the exercise for me. Monitoring, automation and security labs changed that habit. I started asking who could perform the operation, where its result appeared and whether another person could reproduce it. That change is the clearest sign of progress I see in my FCAJ worklog.

## Self-assessment

I based these ratings on lab notes, technical writing, events and project evidence available by 24 November 2026. I gave myself a Good rating where most of my experience still comes from individual exercises.

| No. | Criterion | Basis for my rating | Very good | Good | Average |
|---:|---|---|:---:|:---:|:---:|
| 1 | Technical knowledge | I can explain how identity, networking, compute and storage fit together. I need more practice choosing between services when both traffic and budget change. | | ✓ | |
| 2 | Ability to learn | I moved from console exercises to using the CLI, reading templates and checking the resulting deployment. | ✓ | | |
| 3 | Initiative | When a lab behaves differently from its instructions, I consult the documentation and change one setting at a time. | ✓ | | |
| 4 | Responsibility | I keep evidence for reported results and identify checks that my account permissions leave open. | ✓ | | |
| 5 | Discipline and time management | Weekly entries help me track progress. Some image captions and cost reconciliation still accumulated near the deadline. | | ✓ | |
| 6 | Response to feedback | Feedback on the diagram and navigation led me to retrace data flows and organize instructions around the reader’s tasks. | ✓ | | |
| 7 | Communication | Writing in Vietnamese and English improved my use of technical terms. My spoken explanations sometimes include more detail than the listener needs. | | ✓ | |
| 8 | Collaboration | I can prepare a reproducible example for discussion. Most of my evidence comes from individual labs, and I would benefit from paired deployments. | | ✓ | |
| 9 | Professional conduct | I pay attention to account scope, permissions and information visible in screenshots used for handover. | ✓ | | |
| 10 | Problem solving | During a rollback I traced a failed resource to the denied IAM action and distinguished it from updates cancelled as a consequence. | ✓ | | |
| 11 | Project contribution | I brought together security, monitoring and infrastructure-as-code exercises in a workshop that another learner can deploy, verify and clean up. | ✓ | | |
| 12 | Overall performance | I can build and explain a lab system. Team delivery, performance measurement and longer-term operations remain development areas. | | ✓ | |

## Strengths

I learn well when I have a result I can inspect. At first a monitoring graph with a single point looked incomplete. Comparing the request time, the rule dimension and the statistic showed me how one point could answer the question I was testing. Writing technical blog posts exercises the same skill. Before explaining how a service behaves, I have to find an example a reader could check independently.

Systems Manager and infrastructure as code gave me another example. In the operations week I focused on carrying out a task consistently across resources. Reading a change set the following week moved my question earlier in the process: which resources would update in place and which might be replaced? Those exercises also changed the way I plan and hand over work.

## Areas for improvement

I sometimes spend so long getting one stage to work that my notes omit its starting configuration. Several days later that makes the result harder to explain. The AI and architecture events also showed me how much separates following a demonstration from choosing between two reasonable designs myself. I want to be able to name the deciding constraint, estimate its cost and say what would change my decision.

## Improvement plan

- Before my next lab, I will record the goal, initial configuration and expected result. Afterwards I will document one failure and how I narrowed down its cause.
- Each week I will choose one design decision, write a short explanation in English and present it to a peer in three minutes. Their questions will show where my explanation needs work.
- On my next shared exercise I want to alternate between writing instructions and following someone else’s instructions in a fresh environment. I will use the reader’s observations to revise the handover.

My overall self-rating at this stage is **Good**. The FCAJ habit I most want to keep is supporting what I have learned with a result someone else can verify.

[Weekly worklog](../1-worklog/) · [Technical blog posts](../3-blogsposted/) · [Events attended](../4-eventparticipated/)
