---
layout: post
title: Team Sizing and Team States
type: blog
---

I've had managers with a large number of reports and found it difficult to receive meaningful feedback to grow my career. When I came across an [article](https://review.firstround.com/how-to-size-and-assess-teams-from-an-eng-lead-at-stripe-uber-and-digg/#keep-innovation-and-maintenance-together) about team sizing, I was curious to learn more. 

The author (Will Larson) explains that a manager should ideally have 6-8 reports and Sr managers should have 4-6 reports in order to do their role effectively.

Too few reports and the manager is coding a lot and unable to develop management skills. Small teams are also fragile so that if one person leaves, the team roadmap has to change significantly. Too many reports and the manager is too busy to invest in their team's domain (it might be fine temporarily). A reasonable size will enable the manager to invest time in one-on-ones and reflect deeply on their report's performance.

Sr managers with few reports should be learning about the new domain and supporting their managers, otherwise they will be tempted to meddle in daily operations. Sr managers with too many reports have the same problems as managers with too many reports.

Setting the team sizing also makes it easier to discuss career growth opportunities for managers because it's clearer how a manager can transition to a more senior level. For example, a manager can create a new team by growing an existing team to 10 and then budding into two teams of 5. A smaller team also allows a manager to work on collaborative efforts to invest in their own growth.

As a team, Will states that it's important to keep innovation and maintenance together in order to develop a culture of learning and avoid creating 2 different tiers (innovation vs maintenance). Related to this, Will discuss the various states of a team.

![_config.yml]({{ site.baseurl }}/images/teamstates.png)

* A team is falling behind when they can't deliver on their product roadmap. In this state, the team and customers are both unhappy. 
* A team is treading water if they can deliver smaller features but cannot work on major projects or any form of tech debt. In this state, customers might be happier but major product gaps still exist. 
* A team is repaying debt when they're able to benefit from paying down tech debt (e.g., fewer bugs due to tests, faster product delivery due to "clean" code and fast CI/CD, etc). This team is able to create more space for future tech debt in a snowball effect.
* A team is innovating tech debt is low and the work is satisfying new user needs.

A manager needs to identify the current team state and implement solutions to transition to a better state. **When the team is falling behind, hire more people and celebrate every small win**. When a team is treading water, the idea is to **reduce concurrent work** to make space for tech debt. When the team is repaying debt, make more space for the team to continue repaying debt **while balancing customer needs**. When a team is innovating, create some slack in the team to prevent backtracking and importantly, **ensure that team output is being valued** so that the team isn't creating "science projects" which might result in being defunded. 

These fixes are slow because systems accumulate years of debt that must be unwound, but have patience. **Don't expect quick wins and inflection points immediately. Stay the course.** Also, difficult conversations must be had because customers have to wait while tech debt is being repaid. However, it's a case of failing today so the team can succeed tomorrow, instead of failing indefinitely.