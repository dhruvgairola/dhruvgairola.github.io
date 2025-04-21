---
layout: post
title: Devops Thoughts
type: blog
tags: [Technology]
---

Containerization has resulted in a slew of systems the past few years. While it is convenient to be able to create containers that work on any server, managing and deploying these in production has necessitated the need for orchestration systems such as Kubernetes and Nomad. Cloud providers have been quick to jump on the orchestration bandwagon by creating products such as GCP GKE and AWS ECS. I recently took time to dive deep and configure a complex side project with both GKE and ECS.

The GKE solution requires developers to get familiar with Kubernetes. I found that Kubernetes is a complex platform that solves a ton of problems for large organizations but creates a ton of problems for startups. This is because the networking component of Kubernetes is really difficult to configure. It doesn't help that the Kubenetes and GKE documentations are really fragmented and sometimes outdated. Google products can be pricy and the pricing models are sometimes really opaque (e.g., load balancer units?!) so be prepared for nice little surprises when you get billed. Overall, I wouldn't recommend Kubernetes or GKE for startups because it's too complex to configure and maintain but a larger firm can definitely benefit from it. I didn't try EKS but I'm assuming it has the same problems as GKE (maybe more since AWS VPC networking is tricky in itself).

The ECS solution has two flavors- developers can deploy their containers on EC2 clusters or Fargate clusters which are entirely managed. I found that the Fargate solution was convenient when it worked on their demo containers but I couldn't get it working out of the box on my own container. I switched to the EC2 flavor and thereafter enjoyed using ECS. Overall ECS was faster to get started with compared to GKE but it ties you very intimately into the AWS ecosystem, which may not be a bad thing since I have repeatedly found that AWS products are more feature complete compared to GCP products.

It took me significant effort to understand and configure GKE and ECS and I would not recommend either to a new startup. While it was fun for me to setup a CD pipeline (using Github Actions) that would push my container to a registry and auto-deploy to a cluster, it makes more sense for a startup to just use docker compose or manage containers manually over a load balancer. GKE and ECS are really meant for larger companies with complex ecosystems and dedicated devops teams.