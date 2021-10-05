---
layout: post
title:  "Pharo - a language on a postcard"
categories: pharo
author: Mikko Rajakangas
---
```smalltalk
exampleWithNumber: x

<syntaxOn: #postcard>
"A ""complete"" Pharo syntax"
| y |

true & false not & (nil isNil)
    ifFalse: [ self perform: #add: with: x ].

y := thisContext stack size + super size.

byteArray := #[2 2r100 8r20 16rFF].

{ -42 . #($a #a #'I''m' 'a' 1.0 1.23e2 3.14s2 1) }
    do: [ :each |
    
        | var |
        var := Transcript
            show: each class name;
            show: each printString ].

^ x < y
```

That is the complete syntax of Pharo - a simple
but effective Smalltalk dialect. An exciting
language that fits on a postcard.<!--excerpt-->
Before we dive into *why* exactly I am excited
about Pharo, let's break down the postcard line
by line.

## What's in the message? ##

```smalltalk
exampleWithNumber: x
```
There you have a method declaration where
`exampleWithNumber` is the method name and
`x` is a parameter.

```smalltalk
<syntaxOn: #postcard>
````
This one's interesting and warrants a little
explanation. What you see on this line is a
*pragma*. <EXPLAIN PRAGMA>

```smalltalk
"A ""complete"" Pharo syntax"
| y |
```
Here the first line represents Pharo's comment
syntax, and the second line, `| y |` is a local
variable declaration.

```smalltalk
true & false not & (nil isNil)
    ifFalse: [ self perform: #add: with: x ].
```
Here we have a little more going on so let's
get straight to it: `true` and `false` are
self-evidently boolean literals and `nil` 
is a nil (or null) literal. The last really
simple construct common in non-Smalltalk
languages as well is `[ ... ]`, which is
a block. But there's more. Within these
two lines we have a *binary message* in
`&`, an *unary message* in `isNil`, a
*pseudo variable* in `self` and *keyword messages*
in `ifFalse:`, `perform:` and `with:`.
<EXPLAIN>

```smalltalk
y := thisContext stack size + super size.
```
The thing to notice here is the Go-esque
assignment operator `:=`. On top of that
we have a few pseudo variables in `thisContext`
and `super`.

```smalltalk
byteArray := #[2 2r100 8r20 16rFF].
```
As you'd probably be able to guess, `#[ ... ]`
is a byte array. Now that the elephant in
the room has been addressed, there's a few
other things to note in here. `byteArray` here
is an instance variable declaration, while
the values inside the byte array are integer
literals. Moving on...

```smalltalk
{ -42 . #($a #a #'I''m' 'a' 1.0 1.23e2 3.14s2 1) }
```
There's a lot to break down on this line. Let's get
the two different array constructs on this line first.
`{ ... }` is an array generated at runtime <EXPLAIN> and
`#( ... )` is a literal array <EXPLAIN>. Inside the
literal array we have a character represented by `$a`,
symbols represented by `#a` and `#'I''m'`, a string
represented by `'a'`, floating point numbers represented
by `1.0` and `1.23e2` as well as a *scaled decimal* <EXPLAIN>
represented by `3.14s2`.

```smalltalk
do: [ :each |

    | var |
    var := Transcript
        show: each class name;
        show: each printString ].
```
Here's what's going within this block. First off, `:each`
is a *block parameter* <EXPLAIN>. Then we have a local
block variable in `| var |` and a global variable in
`Transcript`. The most interesting construct to note
here is `show: each class name;` which is a *cascade* <EXPLAIN>.

```smalltalk
^ x < y
```
And finally we have the *return instruction* in `^` <EXPLAIN>.


## So why is this exciting? ##

Now that we've got the Pharo syntax down,
let's talk about what excites me about it
as a language.

## Where to use Pharo? ##