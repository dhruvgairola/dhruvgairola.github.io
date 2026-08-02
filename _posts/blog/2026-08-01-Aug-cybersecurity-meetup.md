---
layout: post
title: Aug 2026 Cybersecurity meetup
type: blog
tags: [Tech Event]
---
![_config.yml]({{ site.baseurl }}/images/confidentialcomputing.jpg)
I've been attending the Toronto AI and cybersecurity meetups pretty regularly to deepen my knowledge and expand my network. There were 3 speakers at this event. 

The first speaked worked at TrendAI and described how they analyzed thousands of MCP repos for CVEs and found a bunch using AI. It was nominally interesting because the research was 7 months old and the AI models back then pale in comparison with the latest models. Also, the MCP ecosystem has evolved further and some of the security issues have been patched.

The second speaker spoke about Sovereign AI and I found it really fascinating as it made me think hard about how governments can deploy AI for internal use. In my team in okta, we use AWS nitro enclaves for some of our services so I could relate to most of the talk.

![_config.yml]({{ site.baseurl }}/images/confidentialcomputing2.png)

The third speaker described the infra and cost of hosting many of frontier-level AI models (millions of dollars for Kimi K3). One interesting point was that self-hosting your own models allows you to bypass artificial guardrails that cloud providers implement. For cybersecurity use cases, this is really critical because the cloud models actively block defenders from responding to threats as shown in the recent Open AI hack of huggingface.