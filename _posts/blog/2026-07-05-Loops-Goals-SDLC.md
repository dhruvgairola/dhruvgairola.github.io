---
layout: post
title: Ralph loops vs Goals in the Software development lifecycle
type: blog
tags: [AI]
---
### AI assisted SDLC
The newest models have made it possible to accelerate various phases of the traditional SDLC (software development lifecycle). At a high-level, SDLC involves ideating, creating a PRD (product requirement doc), UX mockups, a technical brief and implementing and testing the code. AI-based SDLC frameworks such as [BMAD](https://github.com/bmad-code-org/bmad-method) and [Speckit](https://github.com/github/spec-kit) have re-imagined each stage as AI skills that can be triggered by users. The output of each stage are passed along the SDLC pipeline. Recently, Google also published a [short document](https://www.kaggle.com/whitepaper-the-new-SDLC-with-vibe-coding) describing this new paradigm.

![_config.yml]({{ site.baseurl }}/images/sdlc.png)

e.g., of a spec.md created by Speckit for a hotel trip planner app which is a PRD. This is converted to a tech brief and then broken down into tasks.md.

{% include snippet.html file="hotel-trip-planner-spec.md" lang="markdown" title="spec.md" %}

{% include snippet.html file="tasks.md" lang="markdown" title="tasks.md" %}

Unlike Anthropic where every employee is a Member of technical staff, many firms still have defined roles. In these cases, different people own each stage and pass the outputs along. For example, PMs own the initial stages while engineers own the implementation stages. Close collaboration and alignment, quality checks and signoffs are important for most brownfield projects. These checks remain inherently human so the ideation, PRD, UX and tech briefs still feel like traditional SDLC. Some amount of accelearation occurs creating the output artifacts but the bottleneck is human. In the next few sections, I want to zoom in on the implementation phase because [Google's doc](https://www.kaggle.com/whitepaper-the-new-SDLC-with-vibe-coding) claims 25 to 39% productivity gains during this phase.

### Speeding up implementation
The idea of speeding up implementation is that the tasks.md list can be implemented rapidly using AI. Earlier this year, Ralph loop was a popular technique to implement this checklist. An external (bash) `while` loop would pickup the tasks, implement one, **erase the agent's context** and pickup the next task. The context was erased to solve the [context rot](https://redis.io/blog/context-rot/) issue.

Recently, this "loop" idea has been re-imagined natively inside the harnesses (Claude code or Codex) via the `goal` command. For example, assume Claude is being asked to execute 1 task within tasks.md. In this case, you can specify that task within the `goal` command. Claude then uses it's **implementation model** to implement the task. When the work is completed, Claude's `Stop` [hook](https://code.claude.com/docs/en/hooks) is triggered and the agent asks a **separate evaulator model** to run a quality check. If the checks fail, the agent does another turn (turn represents the state machine from `PromptSubmit` to `Stop` states) until the quality gates are passed. So it's more expensive but leads to a higher quality output assuming that the evaluation is defined properly.

### Ease-of-use
![_config.yml]({{ site.baseurl }}/images/ralph.png)

Internal employees of OpenAI call the `goal` command their own take of the Ralph loop but this is a misleading comparison. `goal` runs within 1 session while the Ralph loop creates new sessions each time. Reseting sessions is important for long task lists to avoid context rot.

I think the main value that `goal` provides is the **easy helper function to evaluate quality**. Technically you can implement the same evaluation loop within Ralph loops. For example, after each task is implemented, check that CI passes, linter passes or the test coverage is a certain number. But `goal` makes it super easy to define the quality gates for human engineers.

### Quality within a session
`goal` is useful for **non-deterministic quality checks within the same session**. With Ralph loops, the quality check within a session consists of asking the same implementation LLM if it did a good enough job. However the same LLM that implemented the code will be hard-pressed to critique it's own code. With `goal`, a **separate** evaluator LLM is triggered. This is an edge over Ralph loops because separate LLMs are more honest.

If the quality check is deterministic per task/session, then it really doesn't matter if you use Ralph loops or the `goal` command. For example, in the image below, tiers 1-3 represent machine verifiable tasks while tiers 3-5 represent LLM or human verifiable tasks (courtesy of [Chase AI](https://www.youtube.com/watch?v=JirDfgJcJFU)). The machine verifiable tasks are completely deterministic and independent of LLMs.

![_config.yml]({{ site.baseurl }}/images/ai-verification.png)

### Conclusion
Ralph loops and `goal` are quality mechanisms at different granularities. Ralph loop resets context across sessions while `goal` optimizes implementation within a session. They can be used together if needed e.g., for every task in tasks.md that requires LLM judgement, define that task using `goal`. This gives us the best balance - the Ralph loop solves context rot for the entire task-list while `goal` ensures a higher quality specifically for the judgement-based tasks. 

For one-off tasks that don't need the entire SDLC process to be invoked, Ralph loop is irrelevant. In this case, assuming the task requires LLM judgement, `goal` is a good option because it forces the developer to specify a evaluation condition for quality. The alternative would've been to manually look at the output and apply your own human judgement but this is slower.

> NOTE: `goal` is not to be confused with the `loop` command, which is just a scheduled task that runs periodically and is scoped to a Claude session (only runs when the session is alive). Similarly, `goal` is different from `schedule` (routines) which run remotely on Anthropic's cloud environment, executing agentic instructions based on various triggers.
