---
layout: post
title: "Introducing event-based programming"
categories: general
author: Mikko Rajakangas
---
< This blog will explore an idea hatched by listening to Dave Farley, and rooted in Alan Kay's original idea of messaging as the power of OOP, but from a functional programmer's background - namely a novel programming "paradigm" (if you will) that is completely based on asynchronous messaging between code components/entities in a concurrent manner - or actor-based programming? >

I think Uncle Bob is overrated. There, I
said it, and if someone's ever going to
read this (besides myself), they're
probably going to think I'm an idiot.
But, what I really mean by that is that
taking his writings as a canon, the
ultimate thruth in programming, is no
in anyone's best interest.<!--excerpt-->
What we should be doing is to question
everything. So when Uncle Bob says that
we've discovered all possible programming
paradigms, I'm going to call bullsh*t on
that.

Dave Farley in one of his YouTube videos
briefly explores the concept of constraining
one's programming efforts to asynchronous
messaging, or as I like to call it, events,
similar to how we view them in the context
of event-driven architectures. Or, in fact, very similar to reactive
programming. But I'd like to think
event-based programming as I'm imagining it is in fact a different paradigm.
Why does this matter? Because at their core,
programming paradigms are chiefly
differentiable by how they constrain
programming efforts. So in that sense,
a novel set of constraints equals a new
programming paradigm. So how could this
work? Let's explore that next before
talking any more theory...

## One illustrative example of event-based programming ##

So let's try to build a program using
event-based programming (even though
I haven't even defined it yet). What we
really want are individual components
that emit messages that are acted upon
by other entities asynchronously. In a
nutshell, none of our functions will
actually return anything! Let's see how
this could work in a context of ordering
a bicycle that is ordered in parts and
then built to order. Let's first define
it behaviourally, using Gherkin:

```cucumber
Feature: Order a Bicycle

    Scenario: Customer orders a Bicycle
        When the Customer places an Order on a Bicycle
        Then a Bicycle is Built for the Customer
        
    Scenario: A Bicycle is Assembled from Parts to Order
        Given the Customer has placed an Order on a Bicycle
        When Bicycle Parts have been Ordered
        And Ordered Bicycle Parts are in Store
        Then a Bicycle is Assembled from Parts
```

Okay, realistically, that spec could be
much better. Please excuse me, since I 
only spent about 5 minutes on it - and
it is *not* what we should be focusing on. But for our example system, that gives us the outlines for what we need.

Let's use Python in our example - and
since those who know me also know how
I feel about Python, I have to clarify
we're only using it here for simplicity's sake.
First, we need some boilerplate code to
be able to deal with events in a sane
way. I know there are libraries for this
purpose, but since we only need a very
limited set of functionality, and we
want to be explicit, here we go:

```python
_subscribers = {}

class Event():

    @staticmethod
    def on(event_name, f):
        _subscribers[event_name] = _subscribers.get(event_name, []) + [f]

    @staticmethod
    def emit(event_name, *data):
        [f(*data) for f in _subscribers.get(event_name, [])]
```

So that is the boilerplate. For the sake
of this example, let us imagine this is
defined in a global library used across
our modules. For the same reasons, let's
imagine our subscribers are initialised
globally somewhere like this:

```python
Event.on('order_received', order_parts)
Event.on('frame_needed', order_frame)
# ... et cetera
```

I'm not going to write all of the
subscribers out there for now, since
the point is only to illustrate my idea,
not neccessarily write complete working
code.

< also see [this](https://gist.github.com/MelleB/4036d17f03dbbd126bb27c8dc05b4822) >

## Is it *really* just Reactive Programming? ##

I've been writing this text pretty
organically. By that I mean that apart
from a few minor edits, I've been
figuring what to write as I go. However,
from very early on I started to feel
like there's something very familiar to
the so-called novel paradigm I am
proposing. So am I only reinventing the
wheel, and really just describing what
is already known as reactive
programming? Let's do a little
comparison to find out.