---
layout: post
title: The Phoenix Project
type: blog
---
> Any improvement not made at the constraint is an illusion.

In the past, I worked in an enterprise where our infrastructure was on-premise, we had numerous handoffs to production and monthly deployments for a giant service. I remember filling up Excel sheets for provisioning new infra. Once, I even had to call the head office over a landline phone to debug a faulty configuration (they didn't copy an Oracle wallet to a node in a DB cluster). It took weeks to resolve. 

Years later, in a smaller firm with around 100 engineers, I struggled to get infra provisioned for my service due to the constantly ballooning number of IT initiatives and a lack of process. It seemed to me then that the tension between the development team and IT team was ever-present.

![_config.yml]({{ site.baseurl }}/images/phoenix.png)

With the popularization of devops, many of the ideas in The Phoenix Project have become standardized and have helped ease the tension between development teams and IT teams e.g., infrastructure-as-code, CI/CD, monitoring, feature flags, post-mortems, load testing, automated testing, metrics, dashboards, experimentation, etc. Even within engineering teams, practices such as agile or kanban have made things more efficient even if they're far from perfect.

Gaps still exist between engineering teams and the business. Don't get me wrong - most companies are really engineering companies these days. However, it's still a mystery how various projects relate to the top-level objectives of the business. There is also lack of transparency in how various teams and projects interrelate, which makes it harder to have an end-to-end "systems view" of the entire flow and of the current work-in-progress (WIP) at the org level.

Recently, my company (Wealthsimple with >1000 employees) implemented a system to relate top-level business goals to engineering efforts across the entire org. It provides complete org-level visibility, where anyone can trace repos, documentation, project briefs, tech briefs, UX designs, all the way back to the top-level business objectives. This provides a common goal for everyone. Prior to this, one would have to search across various scattered documents and sheets to figure out what other teams or departments were working on and identify interdependencies, with a vague idea of progress and how those related to business goals. In the past, I tried using JIRA to achieve this within the domain but JIRA is too developer-centric, clunky and slow. The UX and usability prevents non-developers from using JIRA. However, using organizational tools such as Notion, Wealthsimple was able to create a system to tie everything together.

No org is perfect and there are always processes that can be improved or automated. Diminishing returns often prevents orgs from investing in more automation. However we could benefit significantly with end-to-end mobile automation testing for critical flows because we test mobile builds manually and find integration issues frequently. Another opportunity is to incorporate the idea of "chaos" engineering to build resilience in our systems.

I have also summarized the appendix of the book below.

**The Three Ways for effective DevOps**
* The first way - workflow from left to right.
  * Make work visible. (e.g., Kanban)
  * Limit work-in-progress. (e.g., Kanban limits)
  * Reduce batch sizes. (e.g., CI/CD)
  * Reduce number of handoffs. (e.g., stream-aligned teams)
  * Continually identify and elevate constraints. (e.g., infra-as-code, CI/CD, automated testing)
  * Eliminate hardships in the value stream. (e.g., delete partially done work, unnecessary processes, context switching, manual work, etc)
* The second way - fast and constant feedback from right to left.
  * See problems as they occur. (e.g., fast feedback loops via IAC and CI/CD)
  * Swarm and solve problems to build new knowledge.
  * Keep pushing quality close to source. (e.g., test close to source, teams monitor and operate their own systems in prod)
  * Enable optimizing for downstream work centers. (e.g., prioritize non-functional features like performance, stability and testability)
* The third way - create a culture of continuous learning and experimentation.
  * Culture of safety. (e.g., blameless postmortems, sharing information)
  * Institutionalize improvement of daily work. (e.g., reserve time for tech debt)
  * Transform local discoveries into global improvements. (e.g., everything is searchable, eng-handbook, shared repos of knowledge)
  * Inject resilience into daily work. (e.g., chaos monkey)
  * Leaders reinforce learning culture.