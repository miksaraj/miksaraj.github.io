---
layout: post
title:  "Cloud Modernisation of Legacy Applications: Initial Thoughts on Minimum Viable Migrations to Event-Driven Architectures"
categories: cloud
author: Mikko Rajakangas
---
Cloud Modernisation is a hot topic when it comes to
development of enterprise scale applications that some
would call legacy and that have already been basically
lift-and-shift migrated into the cloud. In my case I am
working with a LAMP stack application that was migrated
into AWS half a decade ago. As I've said in [a previous
post]({% post_url 2021-09-28-iac-project %}), there's
nothing inherently wrong in having a legacy-style static
infrastructure in production. Legacy is legacy because
it works. But there are downsides for not embracing
cloud native architecture best practices. <!--excerpt-->
If you have the resources to spare, it's hard to reason
against the flexibility, scalability and other benefits
that migrating to a cloud native way of architecting your
applications provides. The main argument I've personally
come against with has been the sheer scale of a cloud
modernisation project. Yes, it can be a massive
undertaking - **if** you are trying to do it all at
once. Don't do it all at once. It is not a good idea,
put plainly and simply.

Since you are already very likely using agile
methodologies in your development workflows, and it's
a common practice to launch new products and services
in an agile way as MVPs, why not use the same approach
when it comes to cloud modernisation? Using an approach
called Minimum Viable Migrations the aim is to go from
System-As-Is to System-To-Be in small steps,
progressively one service at a time. Of course it does
necessitate you to be able to start breaking the monolith
by identifying how you can break it down to smaller
services - microservices, if you will. It is a similar
process, while the end goal is not to break the application
into containerised microservices as such. The idea here
is to take those proto-microservices and turn them into
cloud native, serverless based units that will communicate
with each other via events.

Serverless and Event-Driven Architectures are two tenets
on which modern cloud native architectures are based on. And
to get there from a traditional monolith we need to enter
the domain of Domain-Driven Design. We're not going hardcore
here, but just borrowing from the core ideas in order to
identify the service boundaries and connections (events)
so that we can start chiseling away pieces of the monolith
and structuring event-driven serverless architectures.