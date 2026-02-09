---
layout: post
title: Context Management
type: blog
tags: [Technology]
---
Context management is crucial when it comes to the effectiveness of agents. As you interact with an agent, your input and the agent output are added to the agent's context, and this context constantly grows over every interaction. The agent **always** sends this context to an underlying LLM model to produce an output. Over time, as you interact more with your agent, this context hits a "context limit" because all LLM models have hard limits. To avoid breaching the context limit, the agent has to either summarize or discard some context. This is where quality problems get exacerbated as key details are lost and the LLM produces bad output or hallucinates.

The agent context can get even more bloated when it needs to share a list of MCP tool definitions with the LLM so the LLM can use those tools to retrieve additional information from other systems. [A recent article](https://www.anthropic.com/engineering/code-execution-with-mcp) describes a few issues. The first issue is that all MCP definitions have to be loaded into the agent, and therefore, the LLM context - this doesn't scale as the number of MCP definitions grow. The second issue is that when an agent makes multiple MCP calls on behalf of the LLM (e.g., load doc from google drive and upload it to salesforce), then the intermediate results have to be sent back to the LLM (e.g., the entire doc is sent to the LLM). When these results are huge (e.g., the doc is large) then the context window can get bloated.

![_config.yml]({{ site.baseurl }}/images/context_mgmt.png)

### Context reduction
A solution to these is to provide agents with LLM models that can write code. For example, in vscode, or even in online webapps like Gemini or ChatGPT, the LLM can write code that the agent can execute directly in the IDE or web UI. The agent can expose a /search API endpoint to the LLM so that the LLM can first query and get a smaller list of tool definitions that are relevant to the immediate task instead of loading every single MCP tool definition into context. A key point is that LLM code can also filter out irrelevant intermediate output or write code to summarize data - these can drastically reduce the context window.

### Context persistence
Another useful technique is to ask agents to **persist state in files**. For very long and complex tasks, you first break the task down into many smaller tasks. Then you can ask the agent to save state after each small task is completed and to read state before picking up the next small task. This is desirable because you can control **when and what** to save instead of the agent auto-compressing context by itself, and because you can **reset the agent context** when the next small task is picked up. You can optionally save individual instructions into files called skills (e.g., brainstorming skill) and you can ask the main agent to use those skills in future.

### Context isolation
Another innovation are subagents, which are processes that can do independent work and report back to the main Claude agent. You as a user can create subagents that have specific roles and personas like a technical design architect or product manager. Thereafter, you can ask the main agent questions and it can choose to use one or many subagents to solve a task. These subagents have their **own context window** so they don't need to store all the useless context that the main agent accumulates over time. They're great for parallel work because the main agent can call multiple subagents to do tasks. For example, you can create a technical architect subagent that can "design the youtube API" and another tester subagent that can "write tests for this API". Then if you ask the main agent to "design youtube", it will hopefully coordinate these 2 subagents to return an output. Subagents aren't just for developers - for example Gemini gems are available for consumers to use.

### Orchestration
There are also ways to formally store the coordination logic and call it explicitly with a command e.g., /techbrief command can be created that explicitly coordinates specific agents. Now you can explicitly ask Claude "/techbrief design youtube" and the main agent will load the orchestration instructions and follow the defined workflow.

![_config.yml]({{ site.baseurl }}/images/context_summary.png)

## Summary
So far we've discussed a few techniques for shrinking the context window - using models that can code, saving state in files to reset context and using subagents. Practically if you're starting from scratch as a developer and have a long and complex task at-hand, you would create subagents and commands to coordinate these subagents. In the command instruction file, you would ensure that intermediate state is saved somewhere and read periodically. Thereafter, you would use the command in the main Claude agent to perform your complex task, **always verfying** each step manually. Better yet, you could skip all of this and install Github libraries into Claude (e.g., "BMAD" or "Get shit done"). These are libraries that contain community-created commands, subagents and skills.

![_config.yml]({{ site.baseurl }}/images/bmad.png)

I have used some of these commands to perform market research and review tech designs. I found that commands are effective as long as the initial plan and incremental output is reviewed by me manually. At times, I've found that the community curated commands create verbose output and I find it better to tune them according to my needs.

It has to be noted that for smaller tasks where the underlying LLM won't run out of context window (e.g., PR reviews), commands are overkill. It's much better to write a good CLAUDE.md file with general instructions, rules and a style-guide, and just call the main agent directly. If necessary, create skills for the main agent. The commands and subagents are exclusively for longer, multi-step tasks to checkpoint and reset agent context instead of letting the agent arbitrarily manage it's own context.