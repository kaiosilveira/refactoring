# Refactoring

**🚧 This repository is a work in progress and is being constantly updated. Stay tuned! 🚧**

This repository is a working implementation of the refactoring patterns described in the "Refactoring" book, by Martin Fowler.

Throughout my entire career I've seen experienced engineers making the mistake of performing destructive\* changes in a code base and committing it as "refactoring". I'm no different and have done it myself more times than I can count. After reading the book and understanding in-depth what "refactoring" means as a discipline, I started to pay way more attention on the small details involved in changing existing code, either when aiming to improve its readability and architecture or when adding new functionality. Fowler mentions the "two hats" in the book as a metaphor to explain that we should do one thing at a time: we are either refactoring or introducing new code / changing behavior, but we should always try to avoid performing both actions at once.

Another key aspect of refactoring is that although we can do it in a codebase that doesn't contain at least unit tests, it would be way harder, both psychologically and practically. Psychologically because we will constantly feel unsure about whether our changes are good and didn't change anything (unless we spend a lot of time doing manually, error-prone manual testing, of course). And practically because we would be potentially creating demand for reworks, introducing bugs that will haunt us in the future or even messing up with the commit history and clarity. These are the main points that highlights the importance of a healthy test suite around a given code base to allow a refactoring session to be pleasant and productive.

_\*destructive: By "destructive" here I mean any change to the code that makes it different from the original one, in terms of external behavior. From this point of view, we could be performing a destructive action even if we are only adding new, bug free, functionality. It's an aggressive approach, but I think it helps illustrating the problem space and highlighting the importance of the small steps._

## What is refactoring?

"Refactoring is the process of changing a software system in a way that does not alter the external behavior of the code yet improves its internal structure. It is a **disciplined way** to clean up code that minimizes the chances of introducing bugs. In essence, when you refactor, you are improving the design of the code after it has been written" - Fowler, Martin @ Refactoring

## Technical details and repo structure

To keep things clean and to make each refactoring as detailed and precise as possible, this repo was organized using [git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules), where each refactoring belongs to a separate git repository, added to this main repo as a reference. Therefore, what you will see here is just an aggregator with some general documentation. Each submodule is named after the refactoring name, and inside each of them you'll find a similar structure:

- a brief description on the motivation of the refactoring
- a "Working example" section containing the sample code we will be working with, including a "before / after" comparison
- a "test suite" section, explaining which unit tests were added to support the refactoring work (please note that these tests may differ from what's in the working examples from Fowler's website because I didn't use them as a reference)
- a step by step description of the refactoring process, including `git diff`s for each step
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

Also, while performing the refactorings, I always kept the test suite running, so I could have a quick feedback when something went wrong.

## The refactoring catalog

This catalog explains in detail (and with working code) the various strategies presented in the book. The examples used here are the same as the ones in the book, with some slight changes when applicable.

- [Extract function](https://github.com/kaiosilveira/extract-function-refactoring): move a code block into a function so it has a meaningful name and a clear intent. It will also improve code readability by helping reduce code repetition.

- [Inline function](https://github.com/kaiosilveira/inline-function-refactoring): sometimes a short function's body is as clear as its name, in these cases we often want to inline it and be more idiomatic instead of performing a function call.

- [Extract variable](https://github.com/kaiosilveira/extract-variable-refactoring): sometimes expressions become hard to read and adds a lot of overhead when trying to understand a given piece of code. Extracting a variable for a complex expression helps the reader by reducing the amount of logic to interpret, replacing it basically by well structured text.

- [Inline variable](https://github.com/kaiosilveira/inline-variable-refactoring): sometimes a variable is as clear and short as the expression it is derived from, and sometimes it gets in the way of refactoring the code surrounding it. In these cases, it is often best to delete the variable and inline the originating expression.

- [Change function declaration](https://github.com/kaiosilveira/change-function-declaration-refactoring): sometimes we want to rename a function or change its parameters' structure, but it's being referenced by a lot of different files and in different ways. This refactoring helps with an approach for these cases.

- [Encapsulate variable](https://github.com/kaiosilveira/encapsulate-variable-refactoring): Global variables are a well-known pain in the development world, not only because it is hard to reason about and keep track of, but also because when the time comes to change it, it's really hard to apply a straightforward, clean refactoring, due to a lot of problems that arise as soon as we touch them. This refactoring provides a strategy to deal with these cases.

- [Rename variable](https://github.com/kaiosilveira/rename-variable-refactoring): Variable names are one of the most important aspects of our software systems. They play an integral role on explaining for code readers what a given piece of code does and how things relate to each other. Usually, as our knowledge about the domain evolve, a natural need to changing variable names to better reflect both our understanding and the actual behavior of the system arise.

- [Introduce Parameter Object](https://github.com/kaiosilveira/introduce-parameter-object-refactoring): Oftentimes we see a group of parameters being used repeatedly as arguments for multiple functions. These groups are often suggesting a hidden structure inside the project's domain. When this pattern is detected, we can use **Introduce Parameter Object** to create a class based on these parameters and to use it instead.

## Disclaimer

The main purpose of this repo is to help fixating the learnings from the book. Please refer to [Refactoring](https://martinfowler.com/books/refactoring.html) for more info about Fowler's work on the topic.
