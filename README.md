# The Value of Refactoring

## Objectives

- Recognize how refactoring code improves readability
- Recognize how refactoring code improves reusability
- Identify refactoring priorities

## Introduction

Refactoring is a critical part of the programming process. It's the part where
you stop and re-read your **working** code with critical eyes. Ask yourself:

- Do I _really_ know how this method works?
- Is this method is simple and clear? Will I think so in 10, 30, 365 days from now?
- Is this implementation written in a way that matches the intention expressed
  by the name of this method?

Like debugging, developers spend their _entire careers_ getting better at
refactoring. You can always learn more, always get better, always elevate a
fellow programmer.

The word refactoring is inspired by middle-school mathematics. Given:

`24 + 12`, another way of seeing it is as `2(12 + 6)` or `6(4+2)` or `12(2 + 1)`, `36(1)`, and finally `36`.

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

## Refactoring Code Improves Readability

Take a glance at the following method:

```ruby
def method_eighteen(*args)
  variables = args.inspect
  !User.find_by_email(variables[2]) ? User.new(variables) : User.find_by_email(variables[2])
end
```

Is this method's purpose clear? The author may understand exactly what this is
doing, at least for now, but it isn't immediately clear to others. If we were to
apply our process for reading complex code, the entire thing can easily be
considered _opaque_ code. It takes a tangible amount of time to figure out whats
going on. Refactoring reduces this time - by making code clearer, the mental
burden can be at least partially lifted. Below is the same method, refactored:

```ruby
def find_or_create_user(first_name, last_name, email)
  user = User.find_by_email(email)

  unless user
    User.new(first_name, last_name, email)
  else
    user
  end
end
```

First thing, the method name `find_or_create_user` now explains what this
method should be doing. Wherever this method is called in a program, giving
it an accurate name will help with understanding. We don't need to know the line
by line code of `User.find_by_email` to infer what it is doing. That is, as long
as it is correctly named!

Second, the arguments for the method were rewritten. [`*args`][args] is a nifty
shorthand way to capture all arguments passed into a method, and it is a useful
thing to know. Programmers sometimes opt to use these sort of techniques to keep
their code abstract and flexible, However, they can also obscure meaning.
`*args` also allows for _any_ number of arguments which is not necessarily a
good thing. Instead, here, while we may be losing some flexibility, we are
solidifying the specific input requirements of this method.

Ruby's syntax allows us to write in a very human friendly way and provides a
variety of ways to structure code. The [`unless user`][unless] conditional works
in the same ways as `if !user`.

The outcome and the purpose of this method has remained the same. Imagine you
are a developer tasked to maintain code that includes this method? Which would
you prefer?

Notice that our refactored code isn't _shorter_ than the original. Refactoring
isn't about brevity or finding the shortest solution to a problem.

## Refactoring Code Improves Reusability

The other benefit of refactoring is that it can make your code less redundant.
Consider the following example:

```ruby
example_shipments_array = [
  {:id => "12321-142330-2133", :weight => 3002.4, :cost => 24000, :estimated_sale_value => 30000},
  {:id => "12321-142330-2134", :weight => 8030.9, :cost => 41000, :estimated_sale_value => 60000},
  {:id => "12321-142330-2135", :weight => 1544.0, :cost => 10000, :estimated_sale_value => 15000},
  {:id => "12321-142330-2136", :weight => 2290.5, :cost => 20000, :estimated_sale_value => 24000}
]

def average_cost_per_shipment(array_of_shipments, additional_taxes)
  total_number_of_shipments = array_of_shipments.length
  cost_array = array_of_shipments.map {|shipment| shipment[:cost]}
  total_costs = 0
  cost_array.each {|cost|
    total_costs += cost
  }
  (total_costs / total_number_of_shipments) * additional_taxes
end

def average_weight_per_shipment(array_of_shipments)
  total_number_of_shipments = array_of_shipments.length
  weight_array = array_of_shipments.map {|shipment| shipment[:weight]}
  total_weight = 0
  weight_array.each {|weight|
    total_weight += weight
  }
  total_weight / total_number_of_shipments
end
```

These two methods are nearly identical, but return different information. What
if we wanted a third method, one for averaging `:estimated_sale_value`? Right
now, we could copy any paste one of our existing methods and change the values
around. It works, but now we've got THREE sets of nearly duplicate code. Lets
consider a different option:

```ruby
example_shipments_array = [
  {:id => "12321-142330-2133", :weight => 3002.4, :cost => 24000, :estimated_sale_value => 30000},
  {:id => "12321-142330-2134", :weight => 8030.9, :cost => 41000, :estimated_sale_value => 60000},
  {:id => "12321-142330-2135", :weight => 1544.0, :cost => 10000, :estimated_sale_value => 15000},
  {:id => "12321-142330-2136", :weight => 2290.5, :cost => 20000, :estimated_sale_value => 24000}
]

def average_from_hash_array(array_of_hashes, key)
  count = array_of_hashes.length
  map_of_key_values = array_of_hashes.map {|hash| hash[key]}
  total = 0
  map_of_key_values.each {|value|
    total += value
  }
  total / count
end

def average_cost_per_shipment(array_of_shipments, additional_taxes)
  average_from_hash_array(array_of_shipments, :cost) * additional_taxes
end

def average_weight_per_shipment(array_of_shipments)
  average_from_hash_array(array_of_shipments, :cost)
end
```

Ah, here we've extracted the code shared between our two original methods and
stored it in its own method. If we needed to write an additional method for
averaging `:estimated_sale_value`, we can lean on the code we've written! We
can even go further with the help of Ruby's `reduce` `Enumerable`:

```ruby
def average_from_hash_array(array_of_hashes, key)
  count = array_of_hashes.length
  total = array_of_hashes.reduce(0) {|sum, shipment| sum += shipment[:key]}

  total / count
end
```

## Identify Refactoring Priorities

1.  **Get it working, then get it right** Refactor only code that works. If you
    try to refactor before the code is done you can get lost in a maze of
    indecision.

        1.  Get your code working (messily)
        2.  Figure out how to make sure your code still works (passes tests, when
            fed in test data it produces the right answer to the console)
        3.  Make small refactors and verify that your test isn't broken _often_
        4.  Commit your updated implementation

2.  **Always be refactoring** It's not a thing "to do later" as a large-scale
    clean up. Just like the dishes or keeping a tidy home, a few extra minutes to
    make sure that what you commit is "clean" will save you hours of misery later.

3.  **Reduce _noise_, boost _signal_** Never let your methods be lies. If the
    method's called `calculate_attack_damage` in a game program:

    - Does it ever return a `String`? That's confusing against the name of the
      method
    - Should this method change another player's life-points. If so, we'd expect
      the method
      to be called `update_opponent_life_points`
    - Can you extract repeated code to shared methods so that methods that use
      this same code get to cast a spotlight on how they're different?

4.  **Establish the "voice" others will write in** As recently as the 1800's
    doctoral students were expected to defend their dissertations in the style
    and voice of the author whose work they'd researched. The reasoning was that
    a scholar of a material could add to that material in a style that indicated
    they had become one with the material. If you leave sloppy code that's hard
    to reason about, those who follow will not hesitate to emulate that style.
    If you write neat, clean code, they will see that they've broken with the
    style and try to do better.

5.  While it doesn't replace the value of tests or of comments, code that's been
    carefully refactored tends to be self-documenting: the names are true to the
    implementations, the implementations are brief and use the most appropriate
    methods in the programming language's vocabulary, the tests (hopefully) provide
    a model of how to use the method. It's a joy to work with code like this: you
    get to focus on making dreams real versus how to code tiny pieces of those
    dreams

### Identify What to Avoid When Refactoring

Once you see how beautiful nicely-factored code looks, you might want to
refactor all the code into succinct "magical" chained method calls. **Avoid
this.**

Remember, the goal is code that's friendly and approachable. You want to avoid
being _too_ clever as well as being _too_ sloppy. As ever, the middle path is
the way to success.

## Conclusion

Refactoring is the process of taking existing working code and improving it for
readability, use of the most appropriate methods, and communication with other
developers. It's a skill that should be practiced often and which can always be
improved, like debugging.
