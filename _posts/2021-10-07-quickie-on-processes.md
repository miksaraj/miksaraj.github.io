---
layout: post
title:  "A quick thought on development processes"
categories: dev-process
author: Mikko Rajakangas
---
One morning, I stopped to think about the processes in our
development team - or rather, the processes we lack.
Processes do exist, of course - although defining them is
vague at best. The work of our development team is quite
informal. Of course, this has both advantages and disadvantages.
As for the development of the software development processes
themselves, the main thing that came to my mind was the
opportunity to drive the adoption of Behaviour-Driven
Development (BDD).<!--excerpt--> BDD is an agile software development
process that originates from Test-Driven Development (TDD) and
encourages collaboration between software developers,
quality assurance and non-development stakeholders ([North 2006](http://dannorth.net/introducing-bdd/);
[2014](https://web.archive.org/web/20150901151029/http://behaviourdriven.org/);
[Bellware 2008](https://www.codemag.com/article/0805061)).
BDD combines the general techniques
and principles of TDD with ideas from Domain-Driven
Design (DDD) and object-oriented analysis and design to provide
software development and management teams with a common set
of tools and a common process for collaborative software
development (North 2014; Bellware 2008).

While BDD is in principle a framework for how software development
should be managed based on both business interests and technical
understanding, the practice of BDD is to use specialised
software tools to support the development process ([Craven 2015](https://www.methodsandtools.com/archive/entreprisebdd.php)).
Although these tools are often developed specifically for
use in BDD projects, they can be seen as specialised forms
of tools to support Test-Driven Development. The role
of these tools is to add automation to the ubiquitous
language, a central theme of BDD (North 2006; Bellware 2008;
[Pardasani 2015](https://www.red-gate.com/simple-talk/development/dotnet-development/behaviour-driven-development-overview-part-1-ubiquitous-language/)).

BDD is largely facilitated by the use of a simple Domain-Specific
Language (DSL), which uses natural language structures
(e.g., English phrases) to express behavior and expected outcomes
([Fowler 2008a](https://martinfowler.com/bliki/DslQandA.html); [2008b](https://martinfowler.com/bliki/BusinessReadableDSL.html)).
BDD is considered an effective technical practice, especially
when the problem space of the business problem to be solved
is complex ([Tharavil 2018](https://www.solutionsiq.com/resource/blog-post/behavior-driven-development-simplifying-the-complex-problem-space/)).
The key reason for me to consider the possibility of
adopting BDD is not so much the complexity of the problem
space as the role of BDD as a process for bringing
together internal stakeholders to collaborate. However,
Adam Craven (2015) argues that the use of BDD tools would
make other functional testing redundant, so one must also
consider one day how one would then deal with, for example,
already existing unit testing practices.

In general, I have been thinking a lot about development processes,
and I will certainly write more on this subject in the future.
It annoys me personally to see the stagnation of a development team,
and I would always like to see more development than is going on.
I'm always wondering what tools and processes we could implement
in our team, what processes we could develop, and what things
we could automate with tools and processes. But the goal
must be to reduce and ease the workload of the development
team and ensure consistency in service quality, without
forgetting the potential business value. Whatever processes
or tools are adopted, the most important thing would be
for the development team to have a continuous improvement
process in place to facilitate all other processes. This
would ensure that any improvements that are successfully made
are not just one-off efforts. All in all, since we as a team
do not have well-defined processes, there are numerous processes
that we could implement. Which ones we should implement,
when and how, is another matter entirely.