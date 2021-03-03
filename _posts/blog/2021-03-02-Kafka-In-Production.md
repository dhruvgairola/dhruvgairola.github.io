---
layout: post
title: Kafka in production
type: blog
---

I spent the last 5 months implementing the first kafka system at Wealthsimple, rolling it into production and scaling it to many millions of events everyday. Our old system that would update customer balances used a poller and we knew it was slow. However, even I was shocked when our new system increased our throughput (of balance updates) by 800% within first month of deployment in-part due to the meme-stock/Gamestop frenzy that lead to a massive growth in our trade volume. Had it not been for this system, we'd have been in serious trouble with the poller since it would prevent customers from placing orders due to bad balances!

It was a difficult journey since we had no prior art for kafka in-house. Still 3 of us senior devs put our hearts into it and came out with a slew of learnings. I'd like to share a few of them.

### Overpartition from the start
In kafka, messages are effectively "load-balanced" into partitions within a topic. So if you have more partitions, the more consumers you can attach per partition and scale. Initially, our production topic was woefully underpartitioned and we thought that we can always increase the paritions in the future. But it's not so simple because if you decide to increase the partitions, then the ordering of messages is impacted and customers would get bad balances. Hence, each time you increase partitions, you will have to schedule downtime and this is harder if you have many different services reading from your topic since all those services will also have downtime. We addressed this fairly early at 50% rollout by cutting over to a new topic with many more paritions and creating a partitioning runbook for the future. My advice would be to overpartition from the very start (but don't go overboard e.g., > 200 partitions because there's a cost to this too).

### Tuning kafka is tricky so have a load testing setup
Both the producer and consumers will need to be tuned depending on the specific library you choose. We went with a java lib for the producer and a JS library for the consumer (we had constraints). Each library has it's own quirks so I would recommend load testing fairly early to see if your code can scale. The reason is that kafka is ridiculously fast - if your consumer isn't setup correctly, you can overwhelm your server or even your DB if you don't have any backpressure implementation. On the flipside, your consumer may turn out to be really slow and multiple consumers may be needed for the heavy load. These libs will usually have various configs to tune your consumer but sometimes it's unclear which config to toggle. Hence, you need a way to load test your consumer to see how the different configs impact throughput.

### Metrics - offset lag is king
If you're moving fast, the most important metric you should focus on is the offset lag. This tells you how far behind your consumer is from your producer. You will need to tune this like crazy if you really care about latency. Over time, you will track other metrics  (e.g., number of events over time) and create alerts for those, but offset lag is #1.

### Data processing and Javascript?
We were stuck with Javascript for the consumer (for reasons I won't go into) so we had to pick a JS library. It was actually pretty decent but the bad part was the Avro plugin that came with it. We actually had to fork the plugin and patch it internally. Anyway, this should be obvious but you really need to vet your third party libs. As a general rule, with data-processing, stick to Python or Java libs.

### DLT
If your consumer can't process certain messages, these should go to a dead-letter topic so they can be replayed later in an automated manner. Most devs who work with queues will know this one since the DLQ (dead-letter queue) is a common pattern but it bears repeating.

### Config as code
If your org buys into kafka, consider building tooling to support kafka. This will allow devs from different teams to collaborate on the same Git repo around creating and configuring topics (e.g., replication factor), schemas, Kafka connect settings, etc. Your CI pipeline can push these changes in a safe manner to staging and prod automatically.

### Logging
Make sure you log which partition is processing which message. This is because specific consumers are attached to specific partitions and if a consumer gets stuck for whatever reason, at the very least you can restart those specific consumers without impacting the consumer group.

### Non-kafka related
Create your rollout plan, rollback plan, on-call runbooks, comms plan, etc. Unfortunately a lot of this only comes with experience...

These sum up a few of my learnings. I consider myself lucky since we didn't have to manage our own infra in-house and went with Confluent instead. I am also very thankful to my manager for supporting me and my teammates for basically everything.