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
is also the **antipattern** of a *monolithic* stack
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

The question of how to build stacks
from lower level infrastructure
constructs can be a tricky one. TL;DR:
there is no single right way to go
about it. In your specific case, the way
to divide your infrastruce into stacks
might be obvious. But in the case it is
not, there are a few ways to go about it.

### Why is a monolith BAD? ###

While I do maintain that declaring all
of your infrastructure under one
monolithic stack is an antipattern,
there is one discliner I'd like to get
out of the way. If you're infrastructure
is really simple, like a single serverless
function behind an API gateway and a
storage of some sort, dividing it into
separate stacks serves only in adding
unneccessary complexity and nothing of
value. Most infrastructures, however,
are a little more complicated than that.

## Building environments with stacks ##

## About configuring stacks ##