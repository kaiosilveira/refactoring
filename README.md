🚧 **This repository is a work in progress. Check out its roadmap at [Refactoring Catalog Roadmap](https://github.com/users/kaiosilveira/projects/1/views/1) for an overview of what's in progress and an estimate of the remaining work** 🚧

ℹ️ _This repository is a working implementation of the refactoring patterns described in the "Refactoring" book, by Martin Fowler. Please refer to [Refactoring](https://martinfowler.com/books/refactoring.html) for more info about Fowler's work on the topic._

# Refactoring

Throughout my entire career, I've seen experienced engineers making the mistake of performing destructive\* changes in a code base and committing it as "refactoring". I'm no different and have done it myself more times than I can count. After reading the book and understanding in-depth what "refactoring" means as a discipline, I started to pay way more attention to the small details involved in changing existing code, either when aiming to improve its readability and architecture or when adding new functionality. Fowler mentions the "two hats" in the book as a metaphor to explain that we should do one thing at a time: we are either refactoring or introducing new code / changing behavior, but we should always try to avoid performing both actions at once.

Another key aspect of refactoring is that although we can do it in a codebase that doesn't contain at least unit tests, it would be way harder, both psychologically and practically. Psychologically because we will constantly feel unsure about whether our changes are good and didn't change anything (unless we spend a lot of time doing manual, error-prone testing, of course). And practically because we would be potentially creating demand for reworks, introducing bugs that will haunt us in the future or even messing up with the commit history and clarity. These are the main points that highlight the importance of a healthy test suite around a given code base to allow a refactoring session to be pleasant and productive.

_\*destructive: By "destructive" here I mean any change to the code that makes it different from the original one, in terms of external behavior. From this point of view, we could be performing a destructive action even if we are only adding new, bug-free, functionality. It's an aggressive approach, but I think it helps illustrate the problem space and highlights the importance of the small steps._

## What is refactoring?

"Refactoring is the process of changing a software system in a way that does not alter the external behavior of the code yet improves its internal structure. It is a **disciplined way** to clean up code that minimizes the chances of introducing bugs. In essence, when you refactor, you are improving the design of the code after it has been written" - Fowler, Martin @ Refactoring

## Technical details and repo structure

To keep things clean and to make each refactoring as detailed and precise as possible, this repo was organized using [git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules), where each refactoring belongs to a separate git repository, added to this main repo as a reference. Therefore, what you will see here is just an aggregator with some general documentation. Each submodule is named after the refactoring name, and inside each of them you'll find a similar structure:

- a brief description of the motivation for the refactoring
- a "Working example" section containing the sample code we will be working with, including a "before / after" comparison
- a "test suite" section, explaining which unit tests were added to support the refactoring work (please note that these tests may differ from what's in the working examples from Fowler's website because I didn't use them as a reference)
- a step-by-step description of the refactoring process, including `git diff`s for each step
- a shortened (`git log --oneline`) git history of the commits made

**Programming language**

The chosen programming language for implementing all patterns is Javascript on top of NodeJS. This matches what Fowler brings in the second edition of his book.
To reduce boilerplate and focus on the refactoring explanation, all the repositories have roughly the same structure, with minimum external dependencies (only `jest` in most cases) and no transpilation / build step.

Regarding continuous integration, a simple GitHub Actions pipeline was put in place to make sure that every commit pushed to `main` is a healthy change. The `ci.yml` file is pretty simple:

```yml
name: CI

on:
  push:
    branches: [main]

jobs:
  Integration:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [16.x]

    steps:
      - name: Check out the repository
        uses: actions/checkout@v2

      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v1
        with:
          node-version: ${{ matrix.node-version }}

      - name: Install dependencies
        run: npm install

      - name: Run unit tests
        run: npm run test
```

This file is used for every refactoring repo.

Also, while performing the refactorings, I always kept the test suite running, so I could have quick feedback when something went wrong.

## The refactoring catalog

This catalog explains in detail (and with actual working code) the various strategies presented in the book. The examples used here are the same as the ones in the book, with some slight changes when applicable.

### A first set of refactorings

The refactorings listed below are considered "essential" ones and were probably used by all readers at some point. They form the foundation for the more involved and complicated refactorings and also help create the refactoring mindset, especially the attention to detail and the concerns regarding keeping the code in a working state and the test suites passing as we go:

- [Extract function](https://github.com/kaiosilveira/extract-function-refactoring): move a code block into a function so it has a meaningful name and a clear intent. It will also improve code readability by helping reduce code repetition.

- [Inline function](https://github.com/kaiosilveira/inline-function-refactoring): sometimes a short function's body is as clear as its name, in these cases we often want to inline it and be more idiomatic instead of performing a function call.

- [Extract variable](https://github.com/kaiosilveira/extract-variable-refactoring): sometimes expressions become hard to read and add a lot of overhead when trying to understand a given piece of code. Extracting a variable for a complex expression helps the reader by reducing the amount of logic to interpret a given piece of code, replacing an expression with well-structured text.

- [Inline variable](https://github.com/kaiosilveira/inline-variable-refactoring): sometimes a variable is as clear and short as the expression it is derived from, and sometimes it gets in the way of refactoring the code surrounding it. In these cases, it is often best to delete the variable and inline the originating expression.

- [Change function declaration](https://github.com/kaiosilveira/change-function-declaration-refactoring): sometimes we want to rename a function or change its parameters' structure, but it's being referenced by a lot of different files and in different ways. This refactoring helps with an approach for these cases.

- [Encapsulate variable](https://github.com/kaiosilveira/encapsulate-variable-refactoring): Global variables are a well-known pain in the development world, not only because it is hard to reason about and keep track of, but also because when the time comes to change it, it's really hard to apply a straightforward, clean refactoring, due to a lot of problems that arise as soon as we touch them. This refactoring provides a strategy to deal with these cases.

- [Rename variable](https://github.com/kaiosilveira/rename-variable-refactoring): Variable names are one of the most important aspects of our software systems. They play an integral role in explaining to code readers what a given piece of code does and how things relate to each other. Usually, as our knowledge about the domain evolves, a natural need to change variable names to better reflect both our understanding and the actual behavior of the system arise.

- [Introduce Parameter Object](https://github.com/kaiosilveira/introduce-parameter-object-refactoring): Oftentimes we see a group of parameters being used repeatedly as arguments for multiple functions. These groups are often suggesting a hidden structure inside the project's domain. When this pattern is detected, we can use **Introduce Parameter Object** to create a class based on these parameters and use it instead.

- [Combine Functions into Class](https://github.com/kaiosilveira/combine-functions-into-class-refactoring): Sometimes we see groups of functions repeatedly operating together over a chunk of data. These functions may be independent and well-defined, but their responsibilities are tightly related to some other, bigger aspect of a piece of computation. In these cases, there is sometimes a hidden class waiting for being discovered.

- [Combine Functions into Transform](https://github.com/kaiosilveira/combine-functions-into-transform-refactoring): Sometimes we see groups of functions repeatedly operating together over a chunk of data. These functions may be independent and well-defined, but their responsibilities are tightly related to some other, bigger aspect of a piece of computation. In these cases, we can create a higher-order transform function to wrap up this bigger aspect and keep all clients consistent.

- [Split Phase](https://github.com/kaiosilveira/split-phase-refactoring): We often find code that's doing more than one thing. Sometimes in a clear order and with some separation to help readers understand what's going on, some other times, without worrying much. When we come across code like this, we often want to make it more clear and readable so we don't have to load our brain with the full context and can rather focus on specific parts.

### Encapsulation

Encapsulation is one of the core concepts of Object-Oriented Programming and is often related to well-modularized code. With encapsulation, we are in full control of our data structures, being sure that any changes to it will have to go through its wrapper, allowing for expansion and gradual modification and deprecation.

- [Encapsulate record](https://github.com/kaiosilveira/encapsulate-record-refactoring): When working with a data record, it is often easy to lose sight of how it's being accessed and modified throughout our application. This refactoring suggests a solution to this problem and provides a step-by-step guide on how to encapsulate our existing raw data records.

- [Encapsulate collection](https://github.com/kaiosilveira/encapsulate-collection-refactoring): Sometimes our code already has a good level of encapsulation: we have classes protecting the access to records and we are providing getters and setters when appropriate. Then, it turns out that one of these getters is returning a collection. Providing raw access to our original nested data structures can be tricky[<sup>[1]</sup>](https://github.com/kaiosilveira/encapsulate-variable-refactoring/tree/099d816c773ebb72232cb1f0744e32bfbf300628#example-2-complex-objects), as unwanted side effects can creep up as hard-to-debug problems. This refactoring provides general guidelines on how to avoid this problem.

- [Replace primitive with object](https://github.com/kaiosilveira/replace-primitive-with-object-refactoring): Often enough, we start modeling things as simple data records, such strings and numbers, just to find out later that it wasn't really that "simple". This phenomenae often leads us to implement duplicated code throughout the codebase, specially to perform validations and comparisons. This refactoring helps refactoring in these embarassing situations.

- [Replace temp with query](https://github.com/kaiosilveira/replace-temp-with-query-refactoring): Sometimes we find it useful to use temporary variables, they help us capture the meaning of involved expressions and also help us to refer to a chunk of computation later. In some situations, though, they're not enough. Sometimes we want to compute a value based on a specific context, so creating multiple temp variables would be less than ideal. In these cases, it's often better to replace the temp with a query.

## Appendix: Useful commands

- fetch all submodules after cloning this repository for the first time in a machine:

```bash
git submodule update --init --recursive
```

- update all submodules to their respective `HEAD`s:

```bash
git submodule update --recursive --remote
```
