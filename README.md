# The Value of Refactoring

## Objectives

- Recognize how refactoring code improves readability
- Recognize how refactoring code improves reusability

## Introduction

Refactoring is a critical part of the programming process. Being able to
_reinterpret_ code, is a great way to test how well you understand a language.
Being able to refactor means to know the behavior of a block of code well enough
that you can express an alternative way or writing it without altering the
outcome or purpose.

In this lesson we're going to briefly demonstrate some of ways in which
refactoring improves our code. While there are no hard rules on how to approach
refactoring, we will also suggest a possible approach to refactoring your own
code.

### Recognize how refactoring code improves readability

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
