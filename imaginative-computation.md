# "imaginative computation"

A talk about reading code

## Slide 1

I don't really have a single good thesis statement for this talk, but if I was
forced to condense it down into one it would be something like:

"Reading computer programs is a bizarre and difficult activity."

A computer program is both a set of instructions meant for a computer to read
and also a sort of documentation which conveys human intention from one
programmer to another. In other words, when I write a program I am telling the
computer what to do and also telling another person -- even if that just means
future me -- what I intended to tell the computer to do.

This makes computer programming into a bizarre and potentially costly form of
writing: if my intention isn't expressed "clearly" in whatever language, it
can create costly bugs and broken software and make it more time-consuming for
other programmers to work with my code.

Part of becoming "fluent" in JavaScript or C or whatever language involves
learning to just forget how bizarre the activity of reading computer programs
is, so that you don't, like, get stuck thinking about the metaphysics of the
`this` keyword when you're supposed to be using it to create a new feature.

One intention I have for this talk is to make "code" feel less familiar by
looking at a few strange manifestations of "code". To do that I'm going to show
off and briefly discuss some quotes, languages, and code snippets that are
intentionally strange or unfamiliar, like obfuscated C code, descriptions of
INTERCAL, and Common Lisp macros.

One really big caveat here is that probably none of the code examples here
should be put into production software. If you want to take away a lesson from
this stuff, I guess you could think of some of these examples as anti-patterns.

The title of the talk, by the way, comes from a weird book by Florian Cramer
called _Words Made Flesh_ which argues that one can find examples of "executable
code" in things like Tarot card readings and experimental poetry. And we are
going to look at an Emily Dickinson poem like halfway through, so if this is
super-dumb or super-bad you'll at least have that to look forward to!

## Slide 2

### 2.1

So I'm Kelly Innes and I'm a software developer at Azavea, a company that makes
geospatial software for organizations like cities, non-profits, and NGOs. I mostly
write JavaScript, Python, and Scala code for work, and I've been learning Clojure
and Common Lisp for fun.

## Slide 3

I'm going to present a couple quotes and a bad code snippet to start.

### 3.1

First is a quote from _Structure and Interpretation of Computer Programs_,
which is a pretty important computer science book that sort of triees to teach
computer programming as one of the "liberal arts":

> Programs must be written for people to read and only incidentally for
> machines to execute.

So that's pretty strange: in essence the argument's that other human beings are
your primary audience when you're writing a computer program.

### 3.2

Next is a quote from _Literate Programming_, a work by Donald Knuth:

> I believe that the time is ripe for significantly better documentation of
> programs, and that we can best achieve this by considering programs to be
> works of literature.

That's also kind of bizarre! I guess the argument is that thinking of programs
as "works of literature" brings in aesthetic or literary judgements in addition
to the "will it run" standard. Note too that the point is to get better
"documentation" of programs -- in other words, writing code as if one is
writing literature will make it easier for other human beings to read that code
later.

### 3.3

This last thing's a pointed practical joke a coworker played on me: printing
out some code that I wrote -- formatted in exactly the way I had written it --
then signing my name to it and hanging it on the wall like it was a poem.

We've since had a team intervention and adopted Prettier and the occasion for
intervening was specifically that some of my coworkers thought that the
verticality here made the code generally a little more difficult (and
thereby costly) to read.

## Slide 4

### 4.1

The fact that it's possible to format code like that is a consequence of how
the JavaScript parser works. Even if it's harder for my coworkers to read the
vertical `handleParcelSearch` method, the parser doesn't care about most of
the whitespace because it is syntactically meaningless. This is a quote from the
ES6 language spec:

> Simple white space and single-line comments are discarded and do not appear
> in the stream of input elements for the syntactic grammar.

### 4.2

Some other languages, however, do treat whitespace as syntactically meaningful:
for instance, Python and Haskell. The Python reference manual has this bit
about how its parser reads whitespace:

> Leading whitespace (spaces and tabs) at the beginning of a logical line is
> used to compute the indentation level of the line, which in turn is used to
> determine the grouping of statements.

It's easy to accidentally introduce subtle bugs into Python programs by
indenting incorrectly, since the indentation governs stuff like whether or not
a particular statement is executed conditionally as part of an `if` block.

## Slide 5

Python in general is pretty opinionated about how it should be written and
read. It even goes so far as to build a sort of "rhetorical manual" directly
into the language itself.

### 5.1

If you go into a Python REPL and type `import this` you get a little easter
egg. (Let's note that this is a pretty bizarre thing to do!)

### 5.2

`import this` will print out "The Zen of Python", which has a list of really
didactic statements about how to read and write code. I'm not going to read
through all of them right now -- although I'll highlight a few of them in a
moment -- but there are two comments I wanted to make about it:

First, these rhetorical rules are essentially what it means for something be
"Pythonic" or "idiomatic Python".

Secondly, this is built directly into the language as part of the standard
library. That means it's in some sense "blessed" as the "right rules for writing
Python."

### 5.3

If you really adore all these rules and want to have a print of them on your
desk so you can internalize them over time, there's a print available on Etsy.
Interestingly, though, the print presents "The Zen of Python" in the most
illegible possible way.

### 5.4

Anyway, back to "The Zen of Python": here are a few of its most exemplary
rules:

- Explicit is better than implicit.
- Readability counts.
- In the face of ambiguity, refuse the temptation to guess.
- There should be one -- and preferably only one -- obvious way to do it.

If I was going to summarize these rules I would say that all four of them are
about reducing the number of "meanings" of any particular piece of Python code
to just one thing. Ambiguity and implicitness are bad because they can increase
the number of possible meanings of any given written thing: if a piece of
writing is ambiguous, that means that a reader has to guess between two or more
possible meanings. If something's implicit it is there as a meaning without
having been actually written.

## Slide 6

So let's contrast this with the most amiguous thing in JavaScript: the
`this` keyword.

If you go to the MDN documentation for `this`, you get a pretty elaborate
explanation.

### 6.1 - 6.15

... scroll through ..

### 6.16

Then you get to the very end of this and there's now an outbound link to a
"Gentle explanation of `this` keyword in JavaScript." In other words, after
you've read all of that it is assumed that you now need to read another guide
in order to understand it.

## Slide 7

The `this` keyword is so complicated that asking how it works is a popular
interview question. It also feels like kind of a bummer gatekeeping mechanism
since it's asking interviewees to prove they know JavaScript by asking them
about one of the flaws in the langauge design.

### 7.1

I found this synopsis of `this` on some online "Front-End Interview Handbook":

> There's no simple explanation for `this`; it is one of the most confusing
> concepts in JavaScript. A hand-wavey explanation is that the value of `this`
> depends on how the function is called.

The guide then presents 6 rules for determining how the value of `this` gets
determined: if a function's called this way, then this means that; if a
function's called this other way, then this means this other thing.

I did want to highlight the last rule though, which is that if the function's
an ES6 arrow function, then ignore the entire rule sequence altogether and do
this specific new thing!

### 7.2

Here's an attempt to show off the difference succinctly. If you open the Node
REPL or browser console and call these two different functions, they will print
out different things!

### 7.3

Here's a poor Python analogue, using a class and `self` instead of an object
and `this`. These two methods will print out the same thing. Note also that
there are no braces, and that whitespace determines what in the class and
what's in a method.

### 7.4

The lack of braces is very very intentional! If you go to the Python REPL and
type `from __future__ import braces` you get this `SyntaxError` message:

```
SyntaxError: not a chance
```

### 7.5

Did you know that JavaScript allows one to dispense with braces in certain
conditions, like when the expression for an `if` clause is on a single line.

This is the Ackermann function, which is a computer science example of a
function that theoretically does have a value for all of its inputs but which
can't be proven to terminate for all of its inputs. The `if` and the `else if`
expressions here are valid JavaScript.

## Slide 8

### 8.1

Abrupt transition, but this is an Emily Dickinson poem that is also essentially
about how to read and write "correctly". As I read through it, note how the
printed form of the both looks like what we usually expect of a poem and has
some oddities, like the lack of punctuation, slant rhyme, and trailing dash.

... read poem ...

I don't want to spend too mnuch time interpreting it, but its argument
generally seems to be about telling the truth via ambiguity and double
meanings and there are a couple places where the poem concentrates several
meanings into a single phrase or word. "Circuit", for example, might mean
"circuitousness" but also, like, an electrical circuit.

### 8.2

This is a quote from another poet named Susan Howe, talking about the editor
who had formatted the poem we just saw.

> For a long time I believed that this editor had given us the poems as they
> looked.

### 8.3

Here's the actual manuscript version from the Emily Dickinson archive. Note how
it diverges from the typeset version of poem, that it kind of barely looks like
poetry, and that there are a few places where Dickinson has directly written
two possibilities for words that became one word in the typeset version:

- bright/good
- gradually/moderately

### 8.4

This is another Susan Howe quote, this time talking about an editor who was
typesetting a new edition of Emily Dickinson poems:

> ...if he were editing a book of letters, he would use the run-on treatment,
> as there is no expected genre for prose. He told me that there is such a form
> for poetry, and he intended to follow it, rather than accidents of physical
> line breaks on paper.

To paraphrase: since readers of poetry expect poems to be formatted in a
specific way, the editor is going to put the poems into that format. This is
effectively the argument for using ESLint or Prettier or gofmt or whatever code
formatter!

## Slide 9

I'd like to transition now to talking about examples of code that is difficult
to read as code for whatever reason: either the authors were making their
programs intentionally difficult or, in my last example, it's a form of "code"
that we aren't used to thinking of as code.

### 9.1

For example: the Perl Poetry contest, which was a competition for poems that
also happen to be functioning programs.

### 9.2

This is from the contest rules: essentially one can either write a program
that's in the form of a poem or write a program that writes poems.

### 9.3

There were a few constraints, including

- the program must compile and run
- don't cheat by using lots of comments

### 9.4

The poems from the contest are mostly pretty terrible! This is one of the less
terrible ones: "Fish Dinner" which is I guess a Perl program that uses keywords
to tell a story about cooking and eating a fish dinner.

### 9.5

Here are some comments about it from the Perlmonks forum. Somebody replied to
it with their own poem, then user "bart" showed up and complained that the poem
in the reply wouldn't compile properly!

## Slide 10

### 10.1

The Obfuscated C Code Contest is another competition to write "artistically
interesting" code. The avowed purposes of the contest are all about writing
difficult-to-read code.

### 10.2

This is an entry from 2011 which imitates the famous Magritte "This is not a
pipe" painting, with a clever "This is not a function" declaration framed by
the actual program.

### 10.3

If you compile the program and then run it by trying to pipe input into it you
get this bit of ASCII art along with the line "This is not a pipe."

If you just try to run `./goren` without trying to pipe input into it, the program
will just sit there.

### 10.4

Here's Goren's "artist's statement" or whatever about the program:

> ...the code is 100% condition free, and every function runs its instructions
> in perfect order. Naturally this makes the program flow trivial to
> understand. This can be verified by disassembling the compiled code and
> looking for conditional jumps (when compiled with gcc on x86, there are
> none).

Just to paraphrase:

- this program does not include and conditional jumps
- that makes it really easy to understand!
- to understand it, all you have to do is:
    - compile the code
    - run the dissassmbler on the compiled program to generate its assembly
    - read the assembly code
    
And then you can easily understand it!

### 10.5

Last year I learned a little bit about Assembly, including how to use the
`ndisasm`, a standard disassembler. So I did what Goren asked and here's the
Assembly I got back.

I don't know Assembly super well, but I am pretty sure that the `jg`
instruction in the first line here is in fact a conditional jump: "jump if
greater than" some value. If that's true then the artist's statement thing is
also being facetious about the program being "condition free". Note that the
program does one thing if you run it by piping input in and a second thing if
you run if without piping input in.

## Slide 11

Common Lisp is uniquely interesting because it makes it possible for
programmers to adjust the actual language syntax sort of on the fly via macros.
The short explanation of why this is possible is that Common Lisp syntax is
just the list data structure and that therefore you write Lisp code directly in
data.

### 11.1

Here's a quote from _Paradigms of Artificial Intelligence Programming_, which
is now mostly useful as a guide for learning how to program in Common Lisp:

> Common Lisp provides a number of built-in macros and allows the user to
> extend the language by defining new macros.

### 11.2

This is an example of a macro that adds a `while` loop to Lisp syntax.
`defun`, the main function declaration keyword, is a built-in Lisp macro.

### 11.3

There's some sentiment that macros should be used extremely sparingly -- only
when necessary -- because they create additional complexity for people trying
to read Lisp code. Immediately after introducing macros, Peter Norving offers
this pretty interestingly-worded warning against them:

> ... every time you write one, you are defining a new language that is just
> like Lisp except for your new macro.

That's a pretty metaphysical way to warn against making code more complicated.
By the way, there's an interesting Lisp language called Racket that is designed
specifically to make it easy for people to write new languages with new syntax.

### 11.4

Macros are also possible in JavaScript via a library called SweetJS. Here's a
screenshot from the library's marketing page which says you can use it to

- build your dream language!
- craft the language you always wanted!

### 11.5

The SweetJS tutorial has an example of making a brand new valid JavaScript
operator. I don't know if you've heard the line about how the JavaScript
`Promise` is secretly a "monad", but here is Haskell's monadic "bind" operator
defined and used to chain the results of some Promises together.

### 11.6

This is the pipe operator from F# or Elixir defined for Javacsript via
SweetJS. From what I understand, occasionally SweetJS is used to implement
proposed ECMAScript syntax changes so they can be used before they make it into
the official language spec.

## Slide 12

### 12.1

There are even some languages that are intentionally difficult to read and
write, like INTERCAL. INTERCAL is a joke language from the 70s that breaks
some norms around syntax and documentation and the like. Here's a quote about
it from a cool article by Michael Mateas and Nick Montfort about weird
languages:

> INTERCAL borrows only variables, arrays, text input/output, and assigment
> from other languages. All other statements, operators and expressions are
> unique (and uniquely werid).

### 12.2

For example...

> INTERCAL has no simple if construction for doing conditional branching, no
> loop constructions, and no basic math operators -- not even addition.

### 12.3

Here's some INTERCAL code. It kind of looks like Assembly, as if the statements
are moving values from one register to another. Note the occasional `PLEASE`s
throughout.

### 12.4

A certain number of `PLEASE` keywords is required to get your program to run:

> if "PLEASE" does not appear often enough, the program is considered
> insufficiently polite, and the error message says this; if too often, the
> program could be rejected as excessively polite. Although this feature
> existed in the original INTERCAL compiler, it was undocumented.

### 12.5 -- maybe cut

### 12.6 -- maybe cut

### 12.7 -- maybe cut

### Slide 13

My last example is a form of code that we don't typically regard as computer
code at all but which people frequently suggest has deep similarities with
computer programming: knitting notation.

### 13.1

This is a quote from "The Computer Science of Knitting"

> Knitted fabric is binary. Just like the 0s and 1s of computerese, there are
> two basic stitches in knitting: the knit stitch and the purl stitch. On the
> "right side" of the fabric, the knit is a flat loop, the purl is a bump.

### 13.2

Here's an example of written knitting notation from an article called "How
Knitting is like Coding".

### 13.3

> k5 means “knit each of the next 5 stitches.” ... The asterisk allows you to
> make a loop — every time you reach the second asterisk, you go back and start
> over at the first asterisk. 

### 13.4

This is what that'd look like if transposed into Ruby/pseudo-code.

## Slide 14

I don't really have a good conclusion for the talk, so instead I thought I'd
close by repeating two of the weird quotes that I began with:

### 14.1

> I believe the time is ripe for significantly better documentation of
> programs, and that we can best achieve this by considering programs to be
> works of literature.

### 14.2

> Programs must be written for people to read and only incidentally for
> machines to execute.

### 14.3

The end.
