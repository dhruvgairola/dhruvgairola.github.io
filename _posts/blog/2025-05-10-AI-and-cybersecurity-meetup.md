---
layout: post
title: AI and Cybersecurity meetup
type: blog
tags: [Technology]
---
I attended the AI and Cybersecurity meetup at Adaptavist in Toronto this Wednesday because I am curious how different components of the AI ecosystem can be scaled in a secure manner. I was able to attend 2 presentations.

<img src="/images/meetup_ai_cyb.jpg" alt="_config.yml" style="width:200px;height:300px;">

From the first speaker, I learned about a few companies at the forefront of AI security. Crowdstrike has a product called Charlotte AI that is able to detect suspicious activity via AI (e.g., suspicious attacks on API endpoints). Clarlotte AI also has a UI tool that enables analysts to define workflows (agentic workflows) so that when suspicious activity is flagged, these are then channelled to the workflow for additional checks and alerts, like a security playbook.

Lakera is another company that provides enterprise AI security. I tried playing their [online game](https://gandalf.lakera.ai/do-not-tell-and-block) that demonstrates how prompt engineering can allow unauthorized actors to obtain sensitive information. In a previous firm, we had a simple firewall to scrub PII and developers could use the LLMs. As it turns out, this is insufficient (the online game has examples). Lakera provides a solution that can detect and monitor bad actors who misuse LLMs.

Snyk has a product called DeepCode AI that allows developers to detect vulnerabilities as they code and apply fixes for these security issues. Finally, the speaker mentioned that Sentinel One has a fantastic blog and I read an [article](https://www.sentinelone.com/blog/avoiding-mcp-mania-how-to-secure-the-next-frontier-of-ai/) about various types of attacks on MCP tools and how monitoring and strict permissioning are important to scale these tools.

In the next presentation, the second speaker explained that Langchain was a complex AI framework with too many abstractions and a poor evolution of their API, making development cumbersome. They proposed that it's more effective for developers to write their own agentic pipelines in code instead of using Langchain. I resonated with their conclusion. In a previous firm, my team summarized customer call transcripts to create client profiles and extract insights. We wrote workflows (using Airflow) and I agree with the speaker that Langchain adds unnecessary complexity when workflows can be used instead. Something new I want to try are context graphs. These allow agents to query GraphDBs to provide information about relationships between entities which can lead to better quality results.