# Refactoring

**This repository is a work in progress and is being constantly updated. Stay tuned!**

This repository is a working implementation of the refactoring patterns described in the "Refactoring" book, by Martin Fowler. To keep things clean and to make each refactoring as detailed and precise as possible, this repo was organized using [git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules), where each refactoring belongs to a separate git repository, added to this main repo as a reference.
Each submodule is named after the refactoring name, and inside each of them you'll find a similar structure:

- a brief description on the motivation of the refactoring
- a "before / after" section containing the sample code we will be working with
- a "test suite" section, explaining which unit tests were added to support the refactoring work (please note that these tests may differ from what's in the working examples from Fowler's website because I didn't use them as a reference)
- a step by step description of the refactoring process, including git diffs for each step

## Refactoring catalog

[extract-function](https://github.com/kaiosilveira/refactoring-extract-function)

## Disclaimer

The main purpose of this repo is to help fixating the learnings from the book. Please refer to [Refactoring](https://martinfowler.com/books/refactoring.html) for more info about Fowler's work on the topic.
