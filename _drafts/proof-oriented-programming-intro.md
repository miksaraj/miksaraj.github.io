---
layout: post
title:  "Proof-Oriented Programming in F* - what, why and how?"
categories: fstar
author: Mikko Rajakangas
---
*Test-Driven Development*, *Behaviour-Driven Development*,
and now *Proof-Oriented Programming*.
Is nothing enough when it comes to
verifying that the program you write
functions as expected? Short answer: **No**.

Long answer...<!--excerpt--> yes, but
why settle for unit or behavioural
tests when you can go a step further?
First things first, though: what is
proof-oriented programming? It's a
programming paradigm in which one
co-designs programs and proofs to
provide mathematical guarantees about
various aspects of a program's behavior,
including properties like functional
correctness, security properties, and
bounds on resource usage.

Ok so, what the heck is ***F\****? Just
yet another language with a letter and
a symbol as its name? Geez... well, it's
much more than that. F\* is a general
purpose functional programming language
with effects aimed at program verification.
You can read much more about F* from their
[official website](http://fstar-lang.org).

I also mentioned *program verification* which
your average developer might not be familiar
with, either. I know I wasn't until one day
I accidentally stumbled into F* some time ago.
Program verification refers to the application
of mathematical proof techniques to establish
the correctness of a program with respect to
a formal specification of the desired behavior.
So *BDD* on steroids and science, if you'd like
to put it that way.

So that brings us to the real question, and the
one ***I*** had on top of my mind when I found
out about F* and program verification: how does
one use it and what are the real-world applications
outside scientific research where proof-oriented
programming could be of use? That's what I set out
to figure out when I started tapping away at my first
ever lines of F* code. This is my attempt at answering
that question.

First, a little background. At a hackathon
earlier this years we put together a simple
web app to calculate the level of your
intoxication (BAC) if you drank X drinks
in Y time - or vice versa, what you'd need
to drink if you wanted a certain level of
intoxication in a given time. Yes, very Finnish,
I know. Also, quite simple. But it didn't work
quite as expected. Kind of did, but not really.
As a project it's one of those that's simple
enough for me to use it for playing around with
different techniques, frameworks et cetera. What
interests us in this instance, however, is the
maths required: we can try and use F* as a tool
and proof-oriented programming as a technique for
formally proving the correctness of the
formulae required for the calculations. So
that's what I set out to do, and here's what
I did, step by step.