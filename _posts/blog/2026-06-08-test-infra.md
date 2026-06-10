---
layout: post
title: Test infra
type: blog
tags: [Technology, Tech Event]
---
![_config.yml]({{ site.baseurl }}/images/robinhood.jpg)

I attended a tech event at the Robinhood office in Toronto titled "Engineering The Future of Finance". Two of their presentations caught my eye. The first was about testing and the second was related to AI devex. Robinhood shared a test framework called Apollo.

## Types of tests
Before I dive in, a quick note on the tests we'd run in a few of my previous firms.

###Within a service in CI -
* BE unit tests per layer of code, mocking the other layers.
* FE component tests, trying hard to avoid mocking state.
* FE visual regression tests (usually for the design system).
* BE integration tests within the service, avoiding mocks as much as possible. Most of the value came from these tests.
* API tests (Cypress or Playwright) that connected FE and BE within the service. External API calls were mocked. Many useful tests lived here too but were more flaky due to FE updates.
* NOTE - We'd set feature flags to mirror the production state in all layers.

###Cross-service tests within the same team in CI -
* We used a product called Pact to make mocks more trustworthy between services. This was met with moderate success since devs who managed a sister-service would forget to update contracts.

###Cross-service tests across teams -
* Usually, these tests were run manually by QA (or devs) before deployment. These tests were slower and reserved for the most critical flows.
* These tests can also be run after service deployment or in a CRON schedule. Playwright has largely replaced the old-school Selenium tests.
* We have used experimental AI crawlers like Gptdriver (for mobile) to supplement the automated tests. Today, claude/codex can easily crawl webapps.

My blog post describes 2 products - Signadot and vcluster. Signadot is similar to Robinhood Apollo which is what prompted me to write this post. These two products **lay out the infrastructure** that makes it easier to execute cross-service tests in CI, or pre/post deployment. The resulting infra also enables devs to test local changes against real services.

![_config.yml]({{ site.baseurl }}/images/signadot.png)

## Signadot
My previous company used a similar system built using [Signadot](https://www.signadot.com/). Essentially we had a staging k8s cluster with hundreds of microservices deployed on their main builds. A developer could deploy a sandbox version of their service, and requests associated with that sandbox would be routed to it while the rest of the system continued using the baseline deployment. Hence, a developer could test their local changes inside the staging cluster, against real services instead of mocks. If any of their changes were breaking, it would only impact their sandbox deployment and related traffic. All the other baseline services and traffic would remain unaffected. 

## Stateful testing
An attendee asked a sharp question about testing on stateful systems and the response was that they're still figuring it out. I've seen that Signadot offers [resource plugins](https://www.signadot.com/docs/overview) that enable devs to provision stateful sandboxes. For example, a DB can be deployed on a stateful sandbox for isolation. This can also work for messaging systems with a little more plumbing using Signadot's routing libraries.

## Complex shared services like Kafka
For complex shared services like Kafka, having a stateful sandbox per developer is overkill and costly for a large org. Hence, a solution here is [message level routing](https://www.signadot.com/docs/guides/set-up-message-queue-isolation). In this pattern, devs have to update the business logic of the producers and consumers. Producers have to append routing keys to the messages. Meanwhile, all consumers have to parse the routing key and decide whether to process or ignore these messages. This is a lightweight solution compared to dedicated stateful sandboxes.

## Vcluster
My current company uses a product called vcluster which offers an isolated environment per developer. It's a different model from Signadot - instead of traffic routing, vcluster actually provisions isolated "virtual" k8s cluster per developer, enabling isolated testing. All the vclusters share the same underlying node pool. It's similar "in spirit" to the stateful sandbox idea. The creators of Signadot did a biased comparison with vcluster and created a comparison [table](https://www.signadot.com/comparison/vcluster/) below.

![_config.yml]({{ site.baseurl }}/images/vcluster.png)

I think that Signadot is cheaper in terms of infra cost because a dev only has to deploy their overridden build over the shared baseline - compared to vcluster where they have to deploy their build in their own virtual env while a different dev does the same. If you have a larger org, more PRs and more services, the shared baseline environment is also faster for local development and CI. With vcluster, you'd have to invest more into speeding up CI. For stateful services or shared services, both solutions seem to roughly converge in setup cost and infra cost. One advantage of vcluster is strong isolation which makes Kubernetes-level testing possible unlike Signadot.

A brief note on Robinhood's second presentation - they presented an incident response bot that would automatically respond to alerts in Slack and present the oncall engineer with links to the runbook. This bot would also scan all the slack threads on a weekly basis and keep their runbooks automatically updated (create a PR to update the runbooks). This is a particularly creative way for AI to help as the first line of defense especially when an oncall rotation is large or system boundaries are fuzzy. In these scenarios, humans would struggle to respond quickly and an AI responder could help guide the oncall devs which would improve the incident resolution time for customers.
