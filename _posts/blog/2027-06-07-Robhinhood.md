---
layout: post
title: TechToronto - Robinhood event
type: blog
tags: [Technology, Tech Event]
---
![_config.yml]({{ site.baseurl }}/images/robinhood.jpg)

I attended a tech event at the Robinhood office in Toronto titled "Engineering The Future of Finance". Their AI devex project caught my attention, where an incident response bot would automatically respond to alerts in Slack and present the oncall engineer with links to the runbook. This bot would also scan all the slack threads on a weekly basis and keep their runbooks automatically updated (create a PR to update the runbooks). This is a particularly creative way for AI to help as the first line of defense especially when an oncall rotation is large or system boundaries are fuzzy. In these scenarios, humans would struggle to respond quickly and an AI responder could help guide the oncall devs which would improve the incident resolution time for customers.

The non-AI presentations were related to testing and infra. The test-related presentation shared an internal framework called Apollo. My previous company had a similar system built upon the [Signadot](https://www.signadot.com/) product almost 1:1. An attendee asked a sharp question about e2e testing on stateful systems and the response was that they're still figuring it out. It was similar in my older firm. My own product teams were unable to run e2e tests over kafka in our CI or staging envs. I read that Signadot does offer "resource plugins" to resolve this issue. Thankfully my current company uses a product called vcluster which offers a stateful environment per developer, and e2e stateful tests are possible in CI. The infra presentations were about their Istio service mesh and were rushed over so I couldn't understand those properly.