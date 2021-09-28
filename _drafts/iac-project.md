---
layout: post
title:  "Why should you keep on top of your cloud architecture?"
categories: cloud
author: Mikko Rajakangas
---
So here it is: the first real blog post ever. I'm now in my sixth week of writing
my diary-style thesis, so I thought I'd write about some of the things I've been
a bit side-tracked on during it. Mainly, my thesis has been about cloud architecture
redesign. The process is still very much in its infancy at my workplace, but I
already have something to say about it.

<!--excerpt-->

First of all, we have a very simple, perhaps even outdated, production architecture
in AWS. Although it is a perfectly valid architecture model for a LAMP application,
we do not make much use of cloud-native features in our architecture. Of course,
this is not necessarily a problem in itself. But if the management of the entire
stack is spread out across repositories, configuration files, and so on, with
almost no documentation, it is very challenging for a small team to manage. So
why modernise? First, you definitely want your infrastructure under the tight
control of a DevOps team. Second, since your DevOps team are application developers,
you want your infrastructure to be code to make it easier to manage. Third, having
your infrastructure under the easy control of your developers makes it easier
and more agile to deploy, modify, and decommission cloud services as business
needs and new trends require these actions.

Static infrastructure quickly becomes obsolete, especially in the cloud, where
service providers are constantly evolving their services. Obsolete infrastructure
will sooner or later become part of the technical debt of the system. There is
so much technical debt in our system that to pay it back, resources need to be
focused on this very work, both in the codebase and in the infrastructure. The
target state in this case is irrelevant - even if it is generally relevant and
important to be known by all.

In the short term, there are a few clear tasks we could do to improve our infrastructure:
we can optimize some computing and database instances, convert schedulable functions
to serverless, and start using the database cluster endpoint to balance the load
on databases. Simple things that can also be done manually - and will improve our
infrastructure in the short term. However, insofar as the aim is both to reduce
technical debt and to prevent it from building up in the future, we need long-term
solutions, and that is a long road to travel.

It is quite clear to me that the first steps on this long road are to make the
infrastructure code-managed so that it can be centrally, clearly and developer-friendly
managed in a way that can be taken over with relatively little effort by future
human resources, and to reconfigure the test and staging environments into environments
that serve the entire DevOps arc. It is clear that in both of these environments,
for example, compute instances can be shut down outside office hours and the infrastructure
of the staging environment must match as closely as possible the infrastructure
of the production environment so that changes to both the codebase and the infrastructure
can be tested in a production-like environment before a production upgrade is made.
Without a staging environment, QA and automated functional testing, the customer
is ultimately also the tester, which is obviously not an ideal situation.

Two things that recur frequently, and are particularly emphasized in AWS best
practices, are automation and monitoring. In other words, anything that can be
automated should be automated, and anything that can be monitored should be monitored.
I'll come back to monitoring later, but let's start with automation from an infrastructure
project perspective. If production upgrades are done manually by running scripts
from developers' own workstations, the production upgrades are vulnerable to changes
in the configurations of individual workstations. At worst, this puts production
in a vulnerable state. Similarly, with changes to the infrastructure being a manual
process, without testing directly into production, it is probably needless to point
out that making even minor changes does not feel very safe when a mistake is
likely to result in a downed service. Both of these things should definitely
be subject to automation. Infrastructure as code is the first step for the infrastructure,
but it is not enough; once it is brought under version control like the codebase,
a CI/CD pipeline can be built for both, where functional testing is performed
automatically in the pipeline itself, and where the staging environment is
the last stand before pushing changes through to production.

As noted, monitoring is another area where particular attention should be paid.
It is not enough to just log things at the application level and have someone
occasionally investigate something. It's not enough to start looking at logs and
things that AWS monitors by default only after something has already gone wrong,
but we should put monitoring in a state where we monitor both infrastructure and
application level for all things that are relevant to the functioning of the
whole, and AWS proactively notifies us when something alarming occurs in the
monitoring. Monitoring should also be automated. However, building best practice
monitoring is not quite as straightforward a process as an automated CI/CD pipeline:
you need to think about what is being monitored and why, what are the thresholds
to trigger alerts or some automated action, and so on. Well constructed and thorough
monitoring can, at best, also serve as a tool to assist decision making on infrastructure
changes. It helps to identify bottlenecks, security vulnerabilities, over- or
under-provisioned resources, spikes in their load (and where these come from),
potentially schedules on how to scale the provisioning of different resources,
where there is room for improvement, and so on. Of course, all of this is of limited
use if processes are not developed at the same time. This is, of course, a topic in
its own right, which I will potentially return to at a later date.

Most of the best practices in cloud computing are continuous or repetitive processes
that are cyclically executed over and over again: the infrastructure, in the same way
as the software itself, is never complete - which in itself is an argument
for the first step: managing the infrastructure as code.