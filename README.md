# The Value of Refactoring

## Objectives

- Recognize how refactoring code improves readability
- Recognize how refactoring code improves reusability

## Introduction

Refactoring is a critical part of the programming process. It's the part where
you stop and re-read your **working** code with critical eyes. Ask yourself:

* Do I _really_ know how this method works?
* Is this method is simple and clear? Will I think so in 10, 30, 365 days from now?
* Is this implementation written in a way that matches the intention expressed
  by the name of this method?

Like debugging, developers spend their _entire careers_ getting better at
refactoring. You can always learn more, always get better, always elevate a
fellow programmer.

The word refactoring is inspired by middle-school mathematics. Given:

`24 + 12`, another way of seeing it is as `2(12 + 6)` or `6(4+2)` or `12(2 +
1)`, `36(1)`, and finally `36`.

In each of these mathematical expressions, the result was the same, `36`. However,
with each step the influence, the dominance, of the factor (`2`, `6`, `12`, `36`)
became _clearer_ and the we could more easily see that the expression meant
`36`.

In code, we want to write our methods so that the obvious "heart" of the
of the method is more and more obvious. We want code to be so darn clever that it
screams "HERE'S HOW I WORK" to an on-call programmer at 3am on Sunday.

Many new programmers think that code should be cleverly written to show how clever
they are. This is not desirable. To quote Brian Kernighan:

> Everyone knows that debugging is twice as hard as writing a program
> in the first place. So if you're as clever as you can be when you
> write it, how will you ever debug it?

Being able to refactor means to know the behavior of a block of
code well enough that you can express an alternative way of writing it without
altering the outcome or purpose.

In this lesson, we're going to briefly demonstrate some of ways in which
refactoring improves our code. While there are no hard rules on how to approach
refactoring, we will also suggest a basic process to refactoring your own code.

> **NOTE**: While there are no _rules_, there are a number of wonderful books
> teach the fuzzy "art" of refactoring. As you become more comfortable coding,
> learning these techniques and their proper names (e.g. "Extract Method,"
> "Introduce Method Object") will be a worthy undertaking. You won't need those
> techniques yet. When you're ready we recommend Fields' "Refactoring: Ruby Edition" as
> a starting place as well as Sandi Metz's "Practical Object Oriented Design in
> Ruby."

### Recognize how refactoring code improves readability

- ( provide a simple each with accumulator and move to map )
- Provide example of a complicated, messy function, that, after refactoring,
  becomes much more obvious. Point out some of the contents that were removed or
  rewritten without going into too much depth

### Recognize how refactoring code improves usability

- Provide example method that has repetitive code and multiple distinct actions
  that, after refactoring, becomes multiple methods, separated out and highly
  reusable in other parts of a project. Point out some of the contents that were
  rewritten without going into too much depth.

### Identify Refactoring Priorities

Refactoring Priorities:

- Reduce _noise_, boost _signal_
- Rewrite code so it makes sense and is more useful to you
  - Working on a project, at every stage of coding, refactor to make sure the
    code you write will be easy to use in the near future
- Rewrite code so it makes sense and is more useful to _future_ you and to others

  - Refactor code to be able to come back in a few months and understand what you've
    written
  - Refactor code so that others may be able to understand and expand on it
  - Good code is self-documenting.

### Approaching Refactoring

Provide a narrative - you've been working on part of a project for the last day
and just got your methods producing what you need to produce. Resist the urge to
forge ahead to the next step in the project as this is the perfect time to take
a few moments to refactor. You know what your code is supposed to be doing and
just as importantly, what it _isn't_ supposed to be doing.

- Rewrite code so it makes sense and is more useful to you.

  - Bourdain in "Kitchen Confidential: "Work Clean"
  - Some of this may be analogous to tidying up after cooking. You may have
    made a mess while trying different ways to solve a problem - unused or
    unnecessary variables
  - Find any code that might be reusable in later parts of your project,
    separate and abstract it so it can be invoked independently wherever necessary

- Rewrite code so it makes sense and is more useful to _future_ you.

  - Assign method and variable names that accurately represent what they do
  - Breakdown larger, opaque methods
  - Identify unused or irrelevant code - it won't be as clear later on if there
    are extra, unnecessary steps in a method
  - Are there any parts of the code that stand out as peculiar or seem old/outdated?
  - Emphasize breaking methods down to singular actions
  - Correct poor syntax and code structure. Poor syntax adds noise.

### Identify What to Avoid When Refactoring

- Rewriting code so that it is as short as possible. Avoid 'super-solutions'
  where a method is condensed down into a single line of chained actions.
  (acknowledge that this may be done sometimes to minimize the size of files for
  sending/receiving but that minimizing code would be something applied after
  the code is thoroughly tested)

## Conclusion

- Literary analogy comparing a convoluted description to a succinct one.

- Reiterate the reusability and readability of refactored code.
- Reiterate that being able to refactor well improves our own ability to understand code

## Resources
