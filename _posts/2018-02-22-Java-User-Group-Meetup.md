---
layout: post
title: Java User Group Meetup (Structure101)
---

I attended my very first Java User Group meetup today held at Free Times Cafe near College and Spadina. The meetup room was packed, and most of the attendees were senior java devs. The presentation was about bridging the gap between the low level architecture of the application and the actual code. 

The problem is that in large scale systems, critical systems and large monolithic codebases, there are a ton of dependencies between the various classes, packages and modules. Visualizing these dependencies is important for devs who want to refactor code either to remove cyclic dependecies or devs who want to layer their code in an organized and maintainable manner. Also, without a visualization, devs will have a harder time understanding the low level architecture of the app. Finally, if a dev changes a piece of code that has a ton of dependents, a dependency graph visualization would be great to determine the impact of any change. 

Structure101 is an IDE plugin that helps devs visualize these dependencies. The visualizations are created based on Java bytecode, so basically this can only be used for compile time analysis (reflections not supported). Structure101 is able to detect cyclic dependencies and also higlight suspicious dependencies (e.g., where a lower layer, like a utility class calls a higher layer, like a service class). Additionally, devs can define a dependency specification between modules (best practices) and overlay this spec on their dependency visualization in order to detect violations. Junior devs who write code that violate these specs (e.g., a service package calls a helper package and the helper package cannot call the service package) can be notified automatically.

From my limited understanding, Structure101 is immensely helpful if you want to refactor large codebases or if you maintain codebases for critical systems. For smaller codebases, or a microservice scenario where dependencies are expressed across systems (rather than within monoliths) Structure101 is less useful.