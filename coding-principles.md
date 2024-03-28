[back to table of contents](readme.md)

(_this document is part of the space150 Engineering Standards & Guidelines_)

# Coding Principles

## Overview

This is a brief summary of several coding principles we hold close to our hearts. Try your best to learn them and adopt them as they will help you write clean / readable code.

## DRY (Don't repeat yourself)

If you find yourself copying/pasting functions, HTML, CSS, or anything consider turning that code into a reusable abstraction.

Before you do that, consider the `AHA` principle (Avoid Hasty Abstractions). In short, abstract when needed and avoid doing it prematurely.

## YAGNI

`You Aren't Gonna Need It`. We've all run into situations where the client wants features `A`, `B`, and `C` and we think that we can support the whole alphabet with a little more effort. YAGNI asks us to only build what is necessary *right now* for the project. [Don't bother supporting the entire alphabet when all you need is ABC](https://xkcd.com/974/). Keep it simple, and don't try to predict what you or the client will need in the future. Remember: humans are terrible at predicting the future.

- You may think the feature will be useful, but your extra work may never actually be needed.
- You may think you know what all the uses cases are, but you're likely wrong. Did you know the alphabet also needs to support punctuation? Whitespace? Kerning? Letterspacing?
- You may think it might be easy to extend, but solving general problems is actually quite difficult. Flexibility means complexity, and complexity makes code maintenance more difficult. Don't do it unless you need to.
- Clients can simply change their minds or may even explicitly not want `D` through `Z` to work.

So only build what is necessary right now, when the requirements are well-known and requested. Don't waste your time on *someday*'s and *maybe*'s. `YAGNI`.

## Separation of Concerns

This is a design principle for separating code into distinct sections.
At a `macro` level, the dealer locator code should not also contain code for the contact us page. At a `micro` level, the function that fetches the dealers should not be concerned with how the dealers are displayed on the page.

A good test of whether you are following this principle is if you can easily write unit tests that cover individual pieces of the feature.

## Throw Early & Throw Often
When your program encounters unfamiliar or unsupported behaviour, it should immediately log and `throw` a useful error context. This gives us a precise location to go to when diagnosing errors as they occur.

- Does your method have a required string argument, but the value passed in was `null` or `undefined`? Throw an error!
- Does your synchronous API make an HTTP call and that API call failed? Throw an error saying so.
- Did you read in a JSON string, but the parser returned `null`? Throw an error saying the JSON wasn't valid!

Imagine a NullPointerException that only occurs in production. The only tools you have available is logging and a stack trace; it is unlikely you will track this down quickly. Having an explicit custom error message to search for can save days of constantly adding log outputs and re-testing different conditions. Contextual error messages makes it significantly easier to find the exact location of a bug. And it's super easy to implement; an if-statement and a throw message is usually all that is required!


## Favor Composition over Inheritence

Think of inheritance as an `is-a` relationship with functionality and composition as a `has-a` relationship. Inheritance has a place, but it should rarely be your first idea when trying to reuse code. [This article](https://medium.com/geekculture/composition-over-inheritance-7faed1628595) sums it up very nicely.

If the inheritance chain is more than a couple levels deep, take a look at what code is actually necessary & useful in that chain. Oftentimes, it can be refactored to use composition and be much cleaner. [Decorators](https://en.wikipedia.org/wiki/Decorator_pattern) can also be used to wrap & modify functionality in a much more flexible manner than inheritance.

## Leverage Pure Functions

A [pure function](https://en.wikipedia.org/wiki/Pure_function), given identical inputs, will return identical outputs and also produce no side effects. This makes them predictable and testable; excellent features in any codebase!

While impure functions are sometimes necessary (`Math.random()`, for instance), when possible, functions should be pure in order to ensure consistency, predictability, and testability. More in [this article](https://dev.to/sanspanic/pure-vs-impure-functions-50aj).