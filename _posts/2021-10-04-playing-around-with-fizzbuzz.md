---
layout: post
title:  "Playing around with FizzBuzz"
categories: general
author: Mikko Rajakangas
---
```hs
import Data.Maybe (fromMaybe)
import Data.Monoid ((<>))
import Control.Monad (guard)
import Control.Applicative ((<$), (<*>))

main = mapM_ putStrLn $ f [1...100]

f = let (divisible -> string) x =
    string <$ guard
    (x `mod` divisible == 0)
    fn map (
        fromMaybe . show <*>
        3 ~> "fizz" <> 5 ~> "buzz"
    )
```

<!--excerpt-->
Happy now? This is what you can do with
Haskell, a purely functional programming
language that not only has pattern matching
(how can **you as a programmer** LIVE without
it?) but also the `<>` operator, which is kind
of a *null coalescing monoid map append* operator,
here resulting in either "fizz", "buzz" or the 
appended "fizzbuzz" depending on given value.

I'm getting way ahead of myself here, but maybe
you're now interested in reading where the heck
we going. We are used to thinking about *FizzBuzz*
as just a simple thing with the sole purpose to weed
out people who don't have basic programming
proficiency in software industry interview processes.
But it can be fun, too, as I'm about to demonstrate.

This is the og definiton of FizzBuzz:

> Write a program that prints the numbers from 1
> to 100. But for multiples of three print "Fizz"
> instead of the number and for the multiples of
> five print "Buzz". For numbers which are multiples
> of both three and five print "FizzBuzz".

Just to get us started, and as an example of those
of you new to FizzBuzz, here are two very, very
classic examples. First in C:

```c
#include <stdio.h>

int main (void)
{
    int i;
    for (i = 1; i <= 100; i++)
    {
        if (!(i % 15))
            printf("FizzBuzz");
        else if (!(i % 3))
            printf("Fizz");
        else if (!(i % 5))
            printf("Buzz");
        else
            printf("%d", i);
        
        printf("\n");
    }
    return 0;
}
```

and then in Python:

```py
for i in xrange(1, 101):
    if i % 15 == 0:
        print "FizzBuzz"
    elif i % 3 == 0:
        print "Fizz"
    elif i % 5 == 0:
        print "Buzz"
    else:
        print i
```

Simple - and ***ugly af***. You'll hopefully
never have to see code like this in the
real world of production code, where the code
should be extensible and maintainable. Let's
assume your client or PM or whoever's calling
the shots wants your FizzBuzz code to be able
to point out multiples of 7 with 'Bazz' as well.
Se let's change that Python code then:

```python
for i in xrange(1, 101):
    if i % 105 == 0: # Can't reach, but for completeness' sake.
        print "FizzBuzzBazz"
    elif i % 35 == 0:
        print "BuzzBazz"
    elif i % 21 == 0:
        print "FizzBazz"
    elif i % 15 == 0:
        print "FizzBuzz"
    elif i % 3 == 0:
        print "Fizz"
    elif i % 5 == 0:
        print "Buzz"
    elif i % 7 == 0:
        print "Bazz"
    else:
        print i
```

Get my point about extensibility and maintainability
now? That's awful. Terrible. As you were
thinking about the implementation you probably
got confused about what order you should be using
for the conditional clauses, what if you forget
an `elif` and so on. When the big boss comes in
asking if 11's could be labeled with 'Boo' you quit.
Enough is enough, here's my resignation letter. Thank
you and goodbye.

A simple conditional clause based solution for the
FizzBuzz problem is problematic, because it is
brittle when extended. Trying to describe this
kind of state machine at this level is foolish to
the point where it's unacceptable. Production code
should not behave this way. But you know what's fun
about playing around with FizzBuzz in this way? It is
doing one of the hardest things a good professional
programmer has to do, finding the line where the code
is *maintainable and extensible* yet not *over-engineered*.

It's just FizzBuzz, right? What can be so hard about it?
The thing is, an extended FizzBuzz is as a problem more
subtle than most developers realise at the outset. What
trips many of us up is that the control flow requires
understanding what you've previously done in order to
make a final decision. It's very easy to get this
wrong:

```c
// This version never outputs the "FizzBuzz" case
// even though it looks like it does
void fizzbuzz0(int i)
{
    if (!(i % 3))
        printf("Fizz");
    else if (!(i % 5))
        printf("Buzz");
    else if (!(i % 15))
        printf("FizzBuzz");
    else
        printf("%d", i);
    printf("\n");
}

// This solution goes through all options with independent
// if statements, but fails to handle the fallback correctly
void fizzbuzz1(int i)
{
    if (!(i % 3))
        printf("Fizz");
    if (!(i % 5))
        printf("Buzz");
    if (WHAT_CAN_WE_DO) // What can you do here? You have to know which branches you took in the past!
        printf("%d");
    printf("\n");
}

// This solution goes through all the options alternatively,
// but fails to handle the Fizz AND Buzz condition.
void fizzbuzz2(int i)
{
    if (!(i % 3))
        printf("Fizz");
    else if (!(i % 5))
        printf("Buzz");
    else
        printf("%d", i);
    printf("\n");
}
```

What we need, in order to make our FizzBuzz
production ready is a better structure to
capture the control flow. Using only naïve
conditionals would explode in size and is
error-prone, as demonstrated above

## Let's Do It Better... ##

```rb
class FizzBuzz

    def initialize
        @printed = nil
        @printers = [   lambda {|x| print (@printed = "Fizz") if x % 3 == 0},
                        lambda {|x| print (@printed = "Buzz") if x % 5 == 0},
                        lambda {|x| print (@printed = "Bazz") if x % 7 == 0}    ]
    end

    def for_num(i)
        @printed = nil
        @printers.each { |1| l.call(i) }
        print i unless @printed
        puts # Newline
    end

    def run(x = 100)
        (1..x).to_a.each { |i| for_num(i) }
    end
end

FizzBuzz.new.run(ARGV.first || 100)
```

Okay so now we have a *Ruby* implementation of
an extensible FizzBuzz* solution where you could
add as many prime factors as you want without a
worry in the world. It is on the slow side of
implementations, and someone could call it
unreadable. There's a lot of machinery around
the solution in order to solve it in an
extensible and maintainable manner. The real
issue with this solution, however, is that it
is only obvious in its operation to programmers
experienced in both Ruby and general software
engineering. Younger programmers would likely
dismiss it as "overly complex" and outright
impenetrable by the most inexperienced. Strange,
since FizzBuzz is supposed to be simple. Right?

As we refine the trivial FizzBuzz problem into
some sort of lambda-ized production ready piece
of code, we really need to start looking into the
actual logic of FizzBuzz. We are doing an awful
lot of work defining the control flow and shamefully
little work talking about the trivial algorithm itself.
As FizzBuzz is made up of several common operations - 
iterating over a group of entities, accumulating data
about that group, providing a sane alternative if no
data is available and producing a human readable output -
it's not even over-thinking a useless problem. Nearly
every program you interact with daily has elements of
FizzBuzz's logic: UI, logging, HTTP request processing,
graphics processing and embedded systems **all** do
things like this all the time. It's CRAZY that most
programming languages don't actually do a half-decent
job of capturing these patterns at a higher level. Indeed,
many software engineers don't even have names or
definitions for these patterns! Think about it for
a moment... just think about it. Crazy, eh?

## Let's Get Math-y! ##

If we go ask mathematicians about these patterns,
thanks to Category Theory, they'd be able to
provide you with a categorisation and general
definitions for most of these patterns. The
functional programming community acually has
dutifully created a system that can actually
leverage a lot of these constructs, albeit
in a limited fashion. Because Category Theory
is so broad, fairly obscure and difficult to know
what's useful in there, most people would
dismiss it as a programmer's utility. So let
me help you here. I'll drag two elementary
and useful concepts out of the FP Jungle for
your consideration. Bare with me...

### Monoids ###

Monoids generalise over anything that can
be "added" associatively without breaking
and that have an "identity" value. Fancy talk,
which is easier to understand as anything
that can be sanely added roughly the way
integers can. Integers form a monoid with `(+,0)`.
You may recall that the associative rule says that
`a + (b + c) = (a + b) + c` and `a + 0 = a`. Turns
out a lot of things programmers work with on a daily
basis actually ***form monoids*** - including
arrays and strings. Strings form a monoid where
the "addition" is string concatenation and the
empty string is the identity value. Which is exactly
what we're doing with "Fizz", "Buzz" and more.

### Optional Values (Maybe) ###

Optional values are actually simple to understand:

> `Maybe` is either `Just` `thing` or `Nothing`.

When dealing with optional values, we either have
an empty value or a wrapped value. Turns out that
if the enclosed `thing`'s type forms a monoid,
then `Maybe thing` forms a monoid! I'm assuming you
may not be familiar with monoids, so this can be
a little tricky to wrap your head around. So let's
take a moment to consider how exactly we define
this with a list of possible values for `mappend`,
the append operation in Haskell:

* `mappend(Just "hello ", Just "world") = Just "hello world"`
* `mappend(Just "hello ", Nothing) = Just "hello "`
* `mappend(Nothing, Just "world") = Just "world"`
* `mappend(Nothing, Nothing) = Nothing`

So we append the inner value if it is a `Just a`,
and if it's a `Nothing` we just treat it like an
empty string. Simple. Ok so let's finally get back
to a FizzBuzz* implementation, this time again in
Haskell. If you're unfamiliar with Haskell, don't
worry. I'll explain.

```hs
{-# LANGUAGE MonadComprehensions #-}

module Main where
import Data.Monoid (mappend)
import Data.Maybe (fromMaybe, listToMaybe, maybe)

fizzbuzz i = fromMaybe (show i) $ mappend   ["Fizz" | i `rem` 3 == 0]
                                            ["Buzz" | i `rem` 5 == 0]

-- mapM_ is our iterator, putStrLn writes to console
main = mapM_ putStrLn [ fizzbuzz i | i <- [1..100] ]
```

Et voilá, c'est **MAGIC**! No, not really, just
maths. But the magic happens in the definition
of `fizzbuzz`, so let's break it down:

1.  We use `fromMaybe (show i)` to help us catch
    the default case were neither "Fizz" nor "Buzz"
    apply by catching the empty value and turning
    it into our default value. `fromMaybe` takes
    a default value and a `Maybe` of the same type.
    If the `Maybe` is `None`, then it gives the
    default value - in this case the string
    representation of our number.
2.  Then we `mappend` two monoid values. Because
    we used `fromMaybe` earlier the compiler infers
    this should be a `Maybe String`. If it does
    become a `Nothing` our `fromMaybe` will catch
    it and turn it into the string representation
    of our number.
3.  Next, we use the [`MonadComprehensions`](https://gitlab.haskell.org/ghc/ghc/-/wikis/monad-comprehensions) syntactic sugar to
    define our `Maybe String` values. Each
    `[value | condition]` block returns a `Just value`
    if the condition is true, otherwise false.
    There are other ways we could have written
    this code (e.g. with a helper function), but
    here I chose monad comprehensions since they
    exist and are easy to use. You may have also
    noticed that we used the same syntax to generate
    our list of FizzBuzz in `main`. Monad Comprehensions
    are very flexible because they work with the
    existing Monad rules, which are a bit out of
    scope here.

The main function is just the same basic "please loop
over 1..100" logic, which we see in every example. It's
appropriate since that's almost exactly how the algorithm
itself is defined. Does it pass the "FooBarBazz Test"
however? Sure does, and we'll take this opportunity
to make the code cleaner by using a synonym for
`mappend`, the `<>` operator:

```hs
{-# LANGUAGE MonadComprehensions #-}

module Main where
import Data.Monoid ((<>))
import Data.Maybe (fromMaybe, listToMaybe, maybe)
import System.Environment (getArgs)

fizzbuzz i = fromMaybe (show i) $   ["Fizz" | i `rem` 3 == 0] <>
                                    ["Buzz" | i `rem` 5 == 0] <>
                                    ["Bazz" | i `rem` 7 == 0]

-- Read the first argument as a number or just use 100.
main = do
    upTo <- fmap (maybe 100 read . listToMaybe) getArgs
    mapM_ putStrLn [ fizzbuzz i | i <- [1..upTo] ]
```

Now that's what I call pretty code. And it's not
just pretty as a picture, we've defined our
program in a way that's **extensible, clear and efficient**
\- just what we wanted. We haven't been forced
to travel down the continuum of abstractions to
lambda lists, but are staying fairly high up
describing things in terms of *when to concatenate*
and *how to fall back on optional values*. The
reason we can do this so naturally is that Haskell
(and other functional languages) has captured
logical patterns - monoids and `Maybe` - that
aptly and succintly describe our control flow.
These patterns are no more complex than just
a few definitions, but allow code to function
at a much higher level. Without these abstractions
we would be forced to build our control flow
out of more basic parts that do not elegantly
express our intent.

## Who cares, tho? ##

Maybe you should? There's more to this than
just a deep feeling of satisfaction and some
benefits to extensibility. The concatenation
in our implementation is written in terms of
a monoid, so it turns out ***any*** suitable
monoid will do. That could matter. Here's a
little far-fetched very real-world problem:
many programming languages have a native string
type that is not actually very good for UTF-8
strings. Haskell's native string type is a list
of characters, which is also pretty terrible
from a performance perspective. Turns out that
since we wrote our implementation in terms of
monoids and constant string values, we can replace
our default strings with better `Text` values:

```hs
{-# LANGUAGE MonadComprehensions, OverloadedStrings #-}

module Main where
import Prelude hiding (putStrLn)
import Data.Monoid (mappend, (<>))
import Data.Maybe (fromMaybe, listToMaybe, maybe)
import System.Environment (getArgs)
import Data.Text
import Data.Text.IO

fizzbuzz d i = fromMaybe (d i) $
                ["Fizz" | i `rem` 3 == 0] <>
                ["Buzz" | i `rem` 5 == 0] <>
                ["Bazz" | i `rem` 7 == 0]

main = do
    upTo <- fmap (maybe 100 read . listToMaybe) getArgs
    mapM_ putStrLn [ fizzbuzz (pack . show) i | i <- [1..upTo] ]
```

1.  We use `Data.Text` values and the `OverloadedStrings`
    language extension to allow us to naturally create
    `Text` values instead of `String` values when the
    type inferencer can determine we need to.
2.  We also alter our function to take a default value
    generator, so that we can move the decision on how
    to produce the default out to the caller.

It doesn't need to stop here, either. We could even do
all sorts of potentially useful things like using a
key-value mapping to store the occurrences of "Fizz",
"Buzz" and "Bazz" instead of using a string representation.
A monoid there would even give us a way to combine them
across iterations, building a counter for occurrences - all
while using the same original function! Turns out that the 
monoid view of this logic is not only clearer and more succint,
it's also *more general*. Not just arrays, strings and integers
can form monoids, but also more powerful structures
like Bloom filters, hash tables, nodes in a neural network...
I could go on. But let's wrap it up here. Now, just ditch
your ol' control flow habits of using naïve conditionals and
join me in embracing and having fun with monoids!