---
layout: post
title: Team Topologies
type: blog
tags: [Management]
---

![_config.yml]({{ site.baseurl }}/images/topologies.png)

* Conways Law - Orgs will produce system designs that are copies of the communication structures of those orgs.
* Org design is about designing for collaborative tech for the voice of the customer.

## Part 1 - Teams as the means of delivery
### The problem with org charts
* Org charts don't help us understand what the actual patterns of comms in our org are. Instead, orgs need to develop more realistic pictures of the expected and actual comms happening between individuals and teams.
* Systems thinking focuses on optimizing for the whole, looking atthe overall flow of work, identifying what the largest bottleneck is today and eliminating it. Then repeat.
* This might not be your largest bottleneck today, but eventually you will face the issue of rigid team structures with poor comm or inadequate processes, slowing down delivery.
* In practice, people comm horizontally with people from other reporting lines in order to get work done. This needs to be nurtured, not restricted to optimize for top-down/bottom-up comms and reporting.
* **An org chart is always out of sync with reality.**
* Instead, we can use 3 different org structures.
  1. Org chart - facilitates compliance.
  2. Informal structure - "realm of influence" between individuals.
  3. Value creation structure - how work actually gets done based on inter-team reputation.
* Key to successful orgs is in the interactions between people and teams.
* Team topologies (TT) approach adds the dynamic and sensing aspects required for tech orgs that are missing from traditional org design.
* TT provides 4 team types (stream-aligned, platform, enabling and complicated subsystem) and 3 team interaction modes (collaboration, x-as-a-service, and facilitating).
* **Periods of product discovery required a highly collaborative env but keeping the same structure once discovery is over can lead to wasted effort and misunderstandings.**
* **Team structures must match the required software architecture or risk producing unintended designs.**
* Thinking of sw architecutre as a standalone concept that can be designed in isolation and then be implemented by any groups of teams is wrong.
* When cognitive load isn't considered, teams are spread think trying to cover an excessive amount of responsibilities and domains. Such a team lacks bandwidth to pursue mastery of their trade and struggles with the costs of switching contexts.
* Obstacles to fast flow
  * Pushing against Conways law.
  * Software too big for teams.
  * Confusing org design options.
  * Team pulled in too many directions.
  * Painful re-org often.
  * Too many surprises.
  * Disengaged teams.

### Conways Law and why it matters
* If the architecture of the system and the architecture of the org are at odds, the architecture of the org wins.
* An org arranged in functional silos (e.g., QA, DBA) is unlikely to producer software systems well-architected for e2e flow.
* **Communication paths effectively restrict the kinds of solutions that the org can devise.**
* Inverse conway maneuver - orgs should evolve their team to achive their desired architecture. The goal should be to support the ability of your team to get work done - from design to dev - without requiring high-bandwidth comms bet teams.
* Team assignments are the first draft of the architecture.
* Keeping things decoupled and team-scoped should be a key bec real gains in perf can often be achieved by adopting designs that adhere to a disaggregated model. The perf gains are due to the increased rate of flow of change and also due to the orgs ability to change the architecture to suit new contexts.
* **Anyone who makes decisions about the shape and placement of teams is strongly influencing the software architecture.**
* Org design and sw design are 2 sides of the same coin - both need to be undertaken by the same people.
* We don't need more comms bet teams but more focused comms between specific teams.
* Managers should look on understanding unpredicted team interactions across modular systems and addressing the cause.
* If everyone is communicating with everyone then we have an org design problem - this will tend to produce highly coupled, interdependent systems that do not support fast flow.
* Reorgs for the sake of management convenience is destructive, esp if they ignore conways law, team cognitive load and related dynamics/flow risk.
* Collab is needed during discovery but less important during execution phase.

### Team-first thinking
* Research by Google found that who is on the team matters less than the team dynamics and that when it comes to measuring perf, teams matter more than individuals.
* Allowing team to grow to beyond nine people imperils the viability of the sw being built by that team because trust will begin to break down and unsuitable decisions will ensue.
* **When delivering changes rapidly, it is important to ensure high trust is explicitly valued and designed for. This is what enables a team to innovate and experiment.**
* Single team - 5 to 8 folks
* Families - groups of teams no more than 50.
* Divisions - No more than 150-500 people.
* It takes about 3 months for a new team to become effective. We need to provide stability around and within the team to allow them to reach that level.
* Tuckman model describes how teams perform.
  1. Forming - creation.
  2. Storming - working through personality differences.
  3. Norming - evolving standard ways of working together.
  4. Performing - reaching high effectiveness.
* The danger in allowing multiple teams to change the same system is that no one owns either the changes made or the resulting mess.
* Every part of the software system needs to be owned by exactly one team. This means no shared ownership of components, libs or code. Ownership doesn't mean policing, more like caretaking.
* **Team members should put the needs of the team above their own.**
* They should 
  * Arrive for standups on time.
  * Keep investigations on track.
  * Encourage a focus on team goals.
  * **Help unblock other folks before starting new work.**
  * Mentor less experienced folks.
  * Agree to explore options - don't try to win the argument.
* Research suggests that diverse teams tend to produce more creative solutions more rapidly and tend to be better at empathizing with other's team needs.
* Diversity fosters better results as members make fewer assumptions about their users.
* Looking to reward indiv perf in modern orgs tends to drive poor results and damages staff behavior.
* Teams can suffer from cognitive load in terms of responsibility, number of apps, number of tools. They spend less time coding and more time on ops.
* **If we stress the team by giving it responsibility for part of the system that is beyond it's cognitive load capacity, it ceases to act like a high-performing unit and starts to behave like a loosely associated group of individuals, each doing their own tasks and failing to consider the best interests of the team.**
* Simple way to assess cognitive load - "Do you feel like you're effective and able to respond in a timely fashion to the work you're asked to do?"
* Identify distinct domains that each team has to deal with and classify these into simple, complicated or complex.
* First heuristic is to assign a single domain to a single team. If its too large, split the domain into subdomains and assign each new subdomain to a single team.
* Second heuristic - A single team can accomodate 2-3 simple domains.
* Third heuristic - A team responsible for a complex domain should not have any more domains assigned to them, not even a simple one.
* The last heuristic - Avoid a single team being responsible for 2 complicated domains. It's best to split them into 2 separate teams of 5 people so they can be more focused and autonomous.
* These are recommendations, not a definitive path to success.
* Use a team-first approach to deciding sw subsystem boundaries.
* Create a team API
  * Code produced by team.
  * How team communicates changes to its code.
  * Wikis.
  * Principles and preferred way of working.
  * Comms - chat and video tools.
  * Work info - what team is working on now, what's coming and priorities in the short to medium term.
* Instead of need a thick skin in order to survive in an org that atomizes individuals, people in a team-first org have the space and support to develop their skills and practices within the context of the team.

## Part 2 - Team Topologies that work for flow
### Static team topologies
* We will discuss 2 anti-patterns for team formation.
* The first is ad hoc team design - too much comms overhead.
* The other is shuffling team members - too much context switching.
* Ask these questions - give our skills, contraints, eng maturity and business goals, which TT will help us deliver faster and safer? How can we reduce hadovers to deployment? Where should the boundaries be?
* **Orgs with many handoffs involving separate sioloed divisions is incompatible with the speed of change and complexity of modern systems. Instead, we must ensure teams have all the skills necessary to design, develop, test, deploy and operate within the team. Teams can improve sw more rapidly and develop a heightened responsiveness to users.**
* Cross-functional/e2e/feature team require high-engineering maturity and trust. Otherwise folks might take shortcuts (e.g., not automating tests), which leads to breakdown of trust between teams as tech debt increases.
* Product teams (own all features for a product) need a support system e.g., self-service infra, CI, monitoring, etc). Otherwise, product teams will be frequently blocked/bottlenecked by other teams.
* Cloud team/infra team should work on infra-as-code and self-serve capabilities.
* SRE team makes sense at scale - SRE is what happens when you ask a sw engineer to design an ops function. They make improvements to ops and app layer to deal with global multi-million user systems.
* Spotify uses a spreadsheet to track inter-dependencies bet squads and tribes. Knowledge, tasks and resources are dependencies.

### Four Fundamental Team Topologies
* 4 fundamental TTs are- stream aligned team, enabling team, complicated subsystem team and platform team.
* Stream aligned teams (e.g., product team, service team).
  * e.g., single service, or product, single set of features, single user journey, etc.
  * Build and deliver value quickly, safely and indepdendently.
  * This is the primary team type in an org.
  * This team is funded in a long-term sustainable manner, not as a fleeting project.
  * Different streams can coexist e.g., customer stream, business area streams, product streams, user-personal streams, etc.
  * Why not call this e2e product team? Bec calling it "stream aligned" is a better generalization and a better term to emphasize fast "flow" of output.
  * A stream aligned team produces steady flow of work, quick to incorporate feedback, experiments, 0 handoffs to other teams, address tech debt, members must achieve "autonomy, mastery and purpose".
* Platform teams (e.g., internal tooling team, infra team).
  * Provides internal services to reduce the cognitive load that we be required from stream-aligned teams to develop underlying services.
  * Platform teams knowledge is made available via API that stream-aligned teams can easily consume.
  * Common platforms abstract away infra, netwroking and other cross-cutting capabilities at the lower level of the stakc. This is a good first step but a platform can also refer to a higher level of abstraction.
  * e.g., logging, monitoring services, APIs for creating test envs, facilities for querying resource usage.
* Enabling teams (e.g., internal training team, internal evangelizing)
  * These teams cross cut multiple stream aligned teams and make informed suggestions on adequate tooling, practice, frameworks and ecosystem choices around the app stack.
  * This team is ahead of the curve in keeping abreast of new tech and promote learning across stream aligned teams, acting as a curator that facilitates knowledge sharing.
  * The purpose is to help stream aligned team deliver working sw. They don't exist to fix problems that arise from poor practices or poor code quality.
  * A stream aligned team should expect to work with an enabling team only for a short time in order to increase their capability around a new tech or concept.
* Complicated subsystem teams (e.g., algo team).
  * Build and maintain a system that depends heavily on specialist knowledge, to the extent that most team members must be specialists in that area.
  * This expertise is hard to find or grow.
  * e.g., mathematical model, face-recognition sw, real-time trade reconciliation algo.
* Avoid team silos in the flow of change as this requires handoffs and slows things down.