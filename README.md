# Refactoring

**This repository is a work in progress and is being constantly updated. Stay tuned!**

This repository is a working implementation of the refactoring patterns described in the "Refactoring" book, by Martin Fowler. To keep things clean and to make each refactoring as detailed and precise as possible, this repo was organized using [git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules), where each refactoring belongs to a separate git repository, added to this main repo as a reference.
Each submodule is named after the refactoring name, and inside each of them you'll find a similar structure:

- a brief description on the motivation of the refactoring
- a "before / after" section containing the sample code we will be working with
- a shortened (`git log --oneline`) git history of the commits made
- a "test suite" section, explaining which unit tests were added to support the refactoring work (please note that these tests may differ from what's in the working examples from Fowler's website because I didn't use them as a reference)
- a step by step description of the refactoring process, including git diffs for each step

## What is refactoring?

"Refactoring is the process of changing a software system in a way that does not alter the external behavior of the code yet improves its internal structure. It is a **disciplined way** to clean up code that minimizes the chances of introducing bugs. In essence, when you refactor, you are improving the design of the code after it has been written" - Fowler, Martin @ Refactoring

## The refactoring catalog

This catalog explains in detail (and with working code) the various strategies presented in the book. The examples used here are the same as the ones in the book, with some slight changes when applicable.

[extract-function](https://github.com/kaiosilveira/refactoring-extract-function): move a code block into a function so it has a meaningful name and a clear intent. It will also improve code readability by helping reduce code repetition.

## Disclaimer

The main purpose of this repo is to help fixating the learnings from the book. Please refer to [Refactoring](https://martinfowler.com/books/refactoring.html) for more info about Fowler's work on the topic.
