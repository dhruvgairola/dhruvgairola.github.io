---
layout: post
title: Agentic incident response and E2E Testing - Robinhood event
type: blog
tags: [Technology, Tech Event]
---
![_config.yml]({{ site.baseurl }}/images/robinhood.jpg)

I attended a tech event at the Robinhood office in Toronto titled "Engineering The Future of Finance". Their AI devex project caught my attention, where an incident response bot would automatically respond to alerts in Slack and present the oncall engineer with links to the runbook. This bot would also scan all the slack threads on a weekly basis and keep their runbooks automatically updated (create a PR to update the runbooks). This is a particularly creative way for AI to help as the first line of defense especially when an oncall rotation is large or system boundaries are fuzzy. In these scenarios, humans would struggle to respond quickly and an AI responder could help guide the oncall devs which would improve the incident resolution time for customers.

Another presetation I enjoyed was about an internal test framework called Apollo. My previous company had a similar system built using [Signadot](https://www.signadot.com/). Essentially we had a staging k8s cluster with all the hundreds of microservices deployed on their main builds. A developer could deploy a sandbox version of their service, and requests associated with that sandbox would be routed to it while the rest of the system continued using the baseline deployment. Hence, a developer could test their local changes inside the staging cluster, against real services instead of mocks. If any of their changes were breaking, it would only impact their sandbox deployment and related traffic. All the other baseline services and traffic would remain unaffected.

![_config.yml]({{ site.baseurl }}/images/signadot.png)

An attendee asked a sharp question about e2e testing on stateful systems and the response was that they're still figuring it out. It was similar in my older firm because my own product teams were unable to test paths that involved Kafka as the infra team was still solving this issue. Since then, I've seen that Signadot offers [resource plugins](https://www.signadot.com/docs/overview) that enable devs to create stateful sandboxes. My current company uses a product called vcluster which offers a stateful environment per developer. It's a different model from Signadot - instead of traffic routing, vcluster actually provisions isolated "virtual" k8s cluster per developer, enabling isolated testing.