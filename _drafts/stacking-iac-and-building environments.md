---
layout: post
title:  "Existing cloud infrastructure to code, Part 2: Stacking your IaC constructs and building environments"
categories: cloud
author: Mikko Rajakangas
---
Once you have managed to jot down all - or even the
central parts - of your [infrastructure into constructs]({% post_url 2021-10-14-former2-to-meaningful-iac %}),
it's time to think about combining them into
infrastructure stacks. There are a few ways of going
about it. You could make stacks by *application groups*,
by *services* or even go the *micro stacks* way. There
is also the **antipatterns** of a *monolithic* stack
that you probably should avoid. We'll touch on [the reasons
why](#why-is-a-monolith-bad) as well as [all the options](#how-to-stack-em-up)
in this post.<!--excerpt-->

Besides the decision on the stacking granularity, we'll also
discuss [building environments with stacks](#building-environments-with-stacks)
and [more](#about-configuring-stacks) in this post. I'll go into
the topics of testing infrastructure, design considerations well
as delivering and changing infrastructure in future posts. We have
enough to talk about already without trying to cram those topics
in here.

## How to stack 'em up? ##

### Why is a monolith BAD? ###

## Building environments with stacks ##

## About configuring stacks ##