---
layout: post
title: Coding at work
type: blog
tags: [Management]
---
Will Larson's recent post about [coding at work](https://lethain.com/coding-at-work/) caught my attention because this is a question I sometimes struggle with. I was glad to learn that even experienced CTOs think about this often.

I have managed a manager for about 1 year and managed ICs for the last 5 years. As a manager of managers, it was much harder to find time to code. Mentally, I couldn't justify it and felt that my output as a manager would be lowered. I could use my time to plan or developing others. Over time, my distance from the codebase grew and it was increasingly difficult to make decisions with conviction. I had to instead obtain answers from trusted engineers and build relationships with stakeholders to align on decisions.

I transitioned from a tech lead to a manager around 5 years ago. During the initial days, I was still producing high quality PRs every week. However, with org shifts, I had to work with new codebases and languages where I didn't have familiarity. This is a more common scenario for a seasoned manager. My approach was to use breakpoints to debug code and understand the architecture. With LLMs, this is now much easier and I often find answers myself and verify them in code. **These days, I feel that managers should use LLMs to understand the codebase first before reaching out to ICs.** The cost of doing this is much lower and the information that you glean is often invaluable. Especially on legacy code, anti-patterns like deep nesting/indirection, massive functions, etc **helps you ask better questions to senior engineers**. The answers to these questions are often surprising and indicate bottlenecks to engineering velocity.

Will creates 4 PRs per week as a CTO of 50 engineers, which is a lot of code. Will did emphasize that these PRs are not in the critical path but it seems execssive and highly unusual even for a manager who likes to code. I hope it's just doc updates. In general though, **a typical codebase has a lot of easy tech debt that managers can contribute towards with relatively low effort and high impact** e.g., deleting feature flags, adding metrics, fixing non-critical vulnerabilities, polish items, scaling and sizing, refining CLAUDE.md, creating agentic skills, refining agentic commands, etc. 

I often find that in a mature repo, manually verifiying a PR is time consuming as a manager. Verifiying that something works e2e often requires integrating with multiple services to ensure no upstream/downstream bugs, and sometimes deploying your service locally can also fail which "wastes" a bunch of debugging time (it's not truly wasted time - it signals a bottleneck). **A manager should try to work on low effort / high impact PRs and verify changes locally, and this requires some regular cadence so they don't lose touch over time.**

I admire that Will finds time on weekends and after work to learn new skills. If someone wants to grow, then effort is necessary. A manager also needs time to reflect and improve soft skills.

The answer to "should managers code at work" depends on the team state. For example, a team that is falling behind would need managers to implement necessary fixes while a more stable team might afford the manager more time to investigate code. I think **the better question is "how much should a manager code at work"**, implying that managers should always keep this question in the back of their mind and consider it seriously when they have space because it unlocks a lot of insights.