---
layout: post
title: Anti-patterns for engineering leaders
type: blog
---

Will Larson, the current CTO of Carta, did an [interview](https://review.firstround.com/unexpected-anti-patterns-for-engineering-leaders-lessons-from-stripe-uber-carta/) with a VC firm about anti-patterns for engineering leaders. In the article, Will describes that there's no clear formula for shipping value more quickly as engineering leaders. Additionally, applying rules from previous firms too universally is counter-productive since the context is different in a new firm.

Will describes that the best eng leaders have to balance three hats -
 * As executives, who focus on business decisions.
 * As eng managers, who optimize people and processes.
 * As engineers, who are responsible for technical excellence.

Eng leaders are often told to distance themselves from code and delegate, however this makes it difficult to balance the three hats. Leaders also become distanced from decisions and lack conviction around decisions. Instead, a leader needs to shift and adapt according to context, and micromanage if necessary. Some leadership [approaches](https://lethain.com/developing-leadership-styles/?ref=review.firstround.com) that one can shift-across are:
 * Leading with policy- create a predictable and consistent process to make decisions e.g., determining promotions.
 * Leading from consensus- pull together relevant stakeholders to find a shared approach to a problem. This works well when none of them have full context and a good decision is important.
 * Leading with conviction- pulling context into your head, evaluating and personally making a decision. This is good when no one has a clear view on making a decision.

Tactical approaches to dive deeper-
 * Conflict mining- Have conversations with experienced people with differing opinions from you. The wrong approach here is to simply implement the conflict-heavy thing, the right approach is to improve your understanding.
 * Writing the details- Building a culture of documentation is important in improving strategy, to share context and ultimately to unlock an engineering org.

Another anti-pattern is to avoid defining metrics just because they are imperfect. Will explains that imperfect metrics can also be useful because they can help spark a discussion with your leaders. Additionally, metrics change every quarter so **the real goal is to build an understanding with your CEO about the real work that your team is doing**. DORA and SPACE metrics are recommended. In cases where business leaders want to measure a flawed metric, instead of saying "this is a terrible way to measure", you can start with saying "let's start here and drill in until we can understand the limitations of measuring this way."

The final anti-pattern discussed was that protecting your team from reality makes things worse for them in the long run. One tactic is to give engineers a seat at the table when big conversations are being had. Another tactic is to appoint "navigators"- a person who acts as a mini-CTO and makes final decisions about their area's technology. 

Larson states that **organizations, even large ones, are just the culmination of a few individuals’ judgments**. As eng leaders, our goal is to create high-quality mechanisms to encourage people being curious, and looking for exceptions.

### Thoughts
I have often described the role of an eng manager as someone who balances people management, processes and technology. It is a difficult balance to strike and I appreciated Will's attempt to define different leadership styles that one can apply to different contexts.

It also resonated with me that an effective manager should contribute to business decisions. Sometimes I've observed artificial boundaries that can develop with product managers and stakeholders, where engineers mostly execute or drive a technical vision. However, I think eng managers should not shy away from steering product direction. EMs need to have a good end-to-end visibility of the product they're shipping so they can understand exactly how they impact customers. At Wealthsimple, we have multifecta syncs with stakeholders to improve our combined understanding of the problem and discuss solutions together.

An EM should not shy away from code. I read an article about Stripe's eng culture where **EMs participate in ["engineerication"](https://newsletter.pragmaticengineer.com/p/stripe-part-2) where the EM takes a week to participate as an IC**. I think this is a fantastic idea to drive home the culture that the devil is in the details. Also, a culture of documentation is something I espouse internally because a problem half-written is half-solved.

Reading through the article also gave me deeper insights into my own mistakes as a manager. When Will described conflict mining, I recalled times where I forced processes on new teams that were sometimes unpopular. Instead, I should have seeked to improve my own understanding of the challenges.

Regarding metrics, Will raised an important point that metrics are really a means to an end, which is to communicate with your leaders about the work your team is doing and context sharing within the team. This was an important insight for me about managing upwards because your leaders should ultimately understand the complexities and the impact of the work your team is shipping.