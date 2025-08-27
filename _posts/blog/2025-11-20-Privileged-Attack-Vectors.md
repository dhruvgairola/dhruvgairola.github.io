---
layout: post
title: Privileged Attack Vectors
type: blog
tags: [Management]
---
# Chapter 1
* Threat actor is someone without technical knowledge while a hacker or attacker are technical. Hackers may compromise due to curiousity while attackers will always have malicious intent.
* To defend against threat actors, PAM solution include access management, logging and monitoring.
* To defend against hackers, vulnerability management solutions identify issues such as missing patches, weak passwords and insecure configs
* To defend against attackers, least privilege solutions and network intrusion prevention is important. Removing root rights and logging and auditing are important.
* 3 pillars of cybersecurity (**all products can be classified in one of these pillars** and most effective solutions gravitate toward the center.)
![_config.yml]({{ site.baseurl }}/images/threepillars.png)
* Privilege: A special right or permission granted, or available only to, a particular person or group to perform special or sensitive
operations upon or within a resource. These are typically associated within information technology as administrator or root
accounts or groups and any accounts that may have been granted elevated entitlements.
* An attack vector is a path or means by which a hacker, attacker, or threat actor can gain access to a computer or network resource to perpetrate a malicious outcome. Attack vectors enable the exploitation of resources based on privileges, assets, and identities (accounts) and can include technology and human elements.

# Chapter 2
* Privileges must be built into the operating system, file system, application, database, hypervisor, cloud management platform, and even network via segmentation to be effective for a user and application-to-application communications.
* The process of defining, managing, and assigning these roles to ensure that the “right” people have the “right” access at the “right” time
is known as identity and access management (IAM). It is a specific solution family within identity governance. These “rights,” including
role-based access and permissions, are called entitlements.
* Identity management only provides access to the resource by scope or role (e.g., trade.read, trade.write). In contrast, privileged access management provides the granular permissions needed when the operating system or application is incapable of providing these privileges itself.
* It is fair to state that PAM is a subset of IAM and an extension to protect privileges at every level.
* Accounts can have role-based access applied at the account level, group level, within a directory, and these can range from disabled (denied access) to privileged accounts such as root, local administrator, or domain admin. **The level of privileges and role-based access is dependent on the security model of the system implementing them and can vary significantly from one implementation to another.**
* Accounts are literally a reference field to provide authentication, and an account may or may not have a password or key. **When a password is assigned, regardless of its strength, type, or security, it becomes a credential.**
* PAM approach for credentials : All activity is performed through an authorized bastion host or gateway that authenticates the user first for auditing and reporting and then brokers the connection.
* The SSH network protocol encrypts all traffic between the client and the server while it is in transit.
* With typically 50–200 SSH keys per server, organizations may have upward of a million SSH keys deployed within their environment. 

# Chapter 4
* Password hacking.
  * Guessing.
  * Shoulder surfing.
  * Dictionary attacks.
  * Pass the hash (scrape memory)
  * Security questions.
  * Credential stuffing.
  * Password spraying.
  * Password resets.
  * SIM jacking.
  * Malware.

# Chapter 6
* There are multiple philosophies on what to do once a breach is detected that are outside of the scope of this book. Regardless, when dealing with compromised credentials, everything privileged to that account is now fair game for the attacker. Resetting passwords is typically a priority and reimaging infected systems is a standard practice (especially if it involves servers).
* The security industry has multiple security standards to convey the risk, threat, and relevance of a vulnerability. The most common standards are the following:
  * Common Vulnerabilities and Exposures (CVE): A standard for information security vulnerability names and descriptions
  * Common Vulnerability Scoring System (CVSS): A mathematical system for scoring the risk of information technology vulnerabilities.


# Chapter 8
![_config.yml]({{ site.baseurl }}/images/threathunting.png)
* Security information enterprise managers (SIEM) are designed to ingest this information, but only allow limited tagging of data by source and type to apply a business element.

# Chapter 9