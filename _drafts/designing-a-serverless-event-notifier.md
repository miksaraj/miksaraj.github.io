---
layout: post
title:  "Designing an Serverless Event Notifier"
categories: dev-serverless
author: Mikko Rajakangas
---
I have a personal use case for an application
that doesn't yet exist. For a developer,
that always presents itself as an opportunity
to design and build. In this post I am
going to address building this application, that, lacking a better wording, could be described as an event
notifier - and since my first instinct
is to go serverless with it, let's add
that catchword in there as well.
<!--excerpt-->

So, a little background may be in order
before we move forward. There is a
company here that serves [khachapuri](https://en.m.wikipedia.org/wiki/Khachapuri)
from a food truck in weekly changing
locations around the country. Sounds
good, doesn't it? At least if you happen
to like decadent heart attack inducing
delicacies. However, they release their
week's locations every Monday only on
their Instagram account and their website.
Realistically, if you don't have the discipline
to go check the locations from their
website weekly, you are really at the
mercy of Instagram's algorithms if you
want to know if there's khachapuri near
you that week. I, for one, have missed
the opportunity at least a handful of
times. Granted, the information is easily
available at their website. But why make the effort to go and check it there if
it is possible to get notified of treats
near you? Now that we have the problem, let's get designing...

## Gimme some specs! ##

What does our application need to do, and what does it *not* need to do? On the surface, the functionality is quite simple:

1. Fetch the information on the week's khachapuri food truck locations.
2. Notify the users subscribed to given locations, if khachapuri is available in their town.

Simple at the outset, but how do we achieve this?

### Getting the information ###

First, we have to fetch the information
regarding the khachapuri truck locations. That requires a web scraper,
or to be more precise, an instagram
scraper. To get the relevant information
for each week, we need to do the
scraping once a week. What if the
information isn't available when we the
scraping code runs? Then we need to set
it up in a way that it retries until the
scraping returns a result. If for some
reason there isn't information available
at all (say, the trucks are on holiday 
break), we need to also set a limit to          
the retries. Now we have the first set
of specification for our first function:

1. A function that scrapes the company's instagram posts for the week's schedule.
2. The above function should be invoked by a cron job once a week.
3. If the scraping function fails to find the week'e schedule, set a retry at a set interval.
4. Keep track of the retries and impose a limit on the retries to prevent an endless retry loop.

### Doing something with the data ###

Ok, now that we have the data containing
the week's khachapuri truck locations
and opening hours, what do we do with it?