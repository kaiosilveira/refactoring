🚧 **This repository is a work in progress. Check out its roadmap at [Refactoring Catalog Roadmap](https://github.com/users/kaiosilveira/projects/1/views/1) for an overview of what's in progress and an estimate of the remaining work** 🚧

ℹ️ _DISCLAIMER: This repository is a working implementation of the refactoring patterns as described in the "Refactoring" book, by Martin Fowler. Please refer to [Refactoring](https://martinfowler.com/books/refactoring.html) for more info about Fowler's work on the topic._

# Refactoring

Throughout my entire career, I've seen experienced engineers making the mistake of performing destructive\* changes in a code base and committing it as "refactoring". I'm no different and have done it myself more times than I can count. After reading the book and understanding in-depth what "refactoring" means as a discipline, I started to pay way more attention to the small details involved in changing existing code, either when aiming to improve its readability and architecture or when adding new functionality. Fowler mentions the "two hats" in the book as a metaphor to explain that we should do one thing at a time: we are either refactoring or introducing new code / changing behavior, but we should always try to avoid performing both actions at once.

Another key aspect of refactoring is that although we can do it in a codebase that doesn't contain at least unit tests, it would be way harder, both psychologically and practically. "Psychologically" because we will constantly feel unsure about whether our changes are good and didn't change anything (unless we spend a lot of time doing manual, error-prone testing, of course). And "practically" because we would be potentially creating demand for reworks, introducing bugs that will haunt us in the future or even messing up with the commit history and clarity. These are the main points that highlight the importance of a healthy test suite around a given code base to allow a refactoring session to be pleasant and productive.

_\*destructive: By "destructive" here I mean any change to the code that makes it different from the original one, in terms of external behavior. From this point of view, we could be performing a destructive action even if we are only adding new, bug-free, functionality. It's an aggressive approach, but I think it helps illustrate the problem space and highlights the importance of the small steps._

## What is refactoring?

> Refactoring is the process of changing a software system in a way that does not alter the external behavior of the code yet improves its internal structure. It is a **disciplined way** to clean up code that minimizes the chances of introducing bugs. In essence, when you refactor, you are improving the design of the code after it has been written

— Fowler, Martin @ Refactoring

## Technical details and repo structure

To keep things clean and to make each refactoring as detailed and precise as possible, this repo was organized using [git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules), where each refactoring belongs to a separate git repository, added to this main repo as a reference. Therefore, what you will see here is just an aggregator with some general documentation. Each submodule is named after the refactoring name, and inside each of them you'll find a similar structure:

- a brief description of the motivation for the refactoring
- a "Working example" section containing the sample code we will be working with, including a "before / after" comparison
- a "test suite" section, explaining which unit tests were added to support the refactoring work (please note that these tests may differ from what's in the working examples from Fowler's website because I didn't use them as a reference)
- a step-by-step description of the refactoring process, including `git diff`s for each step
- a shortened (`git log --oneline`) git history of the commits made

### Development environment

To speed things up a bit and make use of one of GitHub's most useful features implemented in recent times, I developed **[a template repository](https://github.com/kaiosilveira/refactoring-catalog-template)** that sets up most of the boilerplate code and documentation needed to properly implement each refactoring. This strategy was a big time saver and helped me move quickly and efficiently through several refactorings.

### Programming language

The chosen programming language for implementing all patterns is Javascript on top of NodeJS. This matches what Fowler brings in the second edition of his book.
To reduce boilerplate and focus on the refactoring explanation, all the repositories have roughly the same structure, with minimum external dependencies (only `jest` in most cases) and no transpilation / build step.

### Continuous integration

Regarding continuous integration, a simple GitHub Actions pipeline was put in place to make sure that every commit pushed to `main` is a healthy change. Also, while performing the refactorings, I always kept the test suite running, so I could have quick feedback when something went wrong.

### CLI tools

I also **[developed](https://github.com/kaiosilveira/refactoring-catalog-cli)** and **[published](https://github.com/users/kaiosilveira/packages/npm/package/refactoring-catalog-cli)** a simple Command Line Interface (CLI) tool to assist with some useful commands, such as generating the commit history table for each repo. This tool is installed as a dependency in the base template mentioned above, so all refactorings created off of the template already inherit its features.

## The refactoring catalog

This catalog explains in detail (and with actual working code) the various strategies presented in the book. The examples used here are the same as the ones in the book, with some slight changes when applicable.

### A first set of refactorings

The refactorings listed below are considered "essential" ones and were probably used by all readers at some point. They form the foundation for the more involved and complicated refactorings and also help create the refactoring mindset, especially the attention to detail and the concerns regarding keeping the code in a working state and the test suites passing as we go:

- **[Extract function](https://github.com/kaiosilveira/extract-function-refactoring)**: move a code block into a function so it has a meaningful name and a clear intent. It will also improve code readability by helping reduce code repetition.

- **[Inline function](https://github.com/kaiosilveira/inline-function-refactoring)**: sometimes a short function's body is as clear as its name, in these cases, we often want to inline it and be more idiomatic instead of performing a function call.

- **[Extract variable](https://github.com/kaiosilveira/extract-variable-refactoring)**: sometimes expressions become hard to read and add a lot of overhead when trying to understand a given piece of code. Extracting a variable for a complex expression helps the reader by reducing the amount of logic to interpret a given piece of code, replacing an expression with well-structured text.

- **[Inline variable](https://github.com/kaiosilveira/inline-variable-refactoring)**: sometimes a variable is as clear and short as the expression it is derived from, and sometimes it gets in the way of refactoring the code surrounding it. In these cases, it is often best to delete the variable and inline the originating expression.

- **[Change function declaration](https://github.com/kaiosilveira/change-function-declaration-refactoring)**: sometimes we want to rename a function or change its parameters' structure, but it's being referenced by a lot of different files and in different ways. This refactoring helps with an approach for these cases.

- **[Encapsulate variable](https://github.com/kaiosilveira/encapsulate-variable-refactoring)**: Global variables are a well-known pain in the development world, not only because it is hard to reason about and keep track of, but also because when the time comes to change it, it's really hard to apply a straightforward, clean refactoring, due to a lot of problems that arise as soon as we touch them. This refactoring provides a strategy to deal with these cases.

- **[Rename variable](https://github.com/kaiosilveira/rename-variable-refactoring)**: Variable names are one of the most important aspects of our software systems. They play an integral role in explaining to code readers what a given piece of code does and how things relate to each other. Usually, as our knowledge about the domain evolves, a natural need to change variable names to better reflect both our understanding and the actual behavior of the system arises.

- **[Introduce Parameter Object](https://github.com/kaiosilveira/introduce-parameter-object-refactoring)**: Oftentimes we see a group of parameters being used repeatedly as arguments for multiple functions. These groups are often suggesting a hidden structure inside the project's domain. When this pattern is detected, we can use **Introduce Parameter Object** to create a class based on these parameters and use it instead.

- **[Combine Functions into Class](https://github.com/kaiosilveira/combine-functions-into-class-refactoring)**: Sometimes we see groups of functions repeatedly operating together over a chunk of data. These functions may be independent and well-defined, but their responsibilities are tightly related to some other, bigger aspect of a piece of computation. In these cases, there is sometimes a hidden class waiting to be discovered.

- **[Combine Functions into Transform](https://github.com/kaiosilveira/combine-functions-into-transform-refactoring)**: Sometimes we see groups of functions repeatedly operating together over a chunk of data. These functions may be independent and well-defined, but their responsibilities are tightly related to some other, bigger aspect of a piece of computation. In these cases, we can create a higher-order transform function to wrap up this bigger aspect and keep all clients consistent.

- **[Split Phase](https://github.com/kaiosilveira/split-phase-refactoring)**: We often find code that's doing more than one thing. Sometimes in a clear order and with some separation to help readers understand what's going on, some other times, without worrying much. When we come across code like this, we often want to make it more clear and readable so we don't have to load our brain with the full context and can rather focus on specific parts.

### Encapsulation

Encapsulation is one of the core concepts of Object-Oriented Programming and is often related to well-modularized code. With encapsulation, we are in full control of our data structures, being sure that any changes to it will have to go through its wrapper, allowing for expansion, gradual modification and deprecation.

- **[Encapsulate record](https://github.com/kaiosilveira/encapsulate-record-refactoring)**: When working with a data record, it is often easy to lose sight of how it's being accessed and modified throughout our application. This refactoring suggests a solution to this problem and provides a step-by-step guide on how to encapsulate our existing raw data records.

- **[Encapsulate collection](https://github.com/kaiosilveira/encapsulate-collection-refactoring)**: Sometimes our code already has a good level of encapsulation: we have classes protecting the access to records and we are providing getters and setters when appropriate. Then, it turns out that one of these getters is returning a collection. Providing raw access to our original nested data structures can be tricky[<sup>[1]</sup>](https://github.com/kaiosilveira/encapsulate-variable-refactoring/tree/099d816c773ebb72232cb1f0744e32bfbf300628#example-2-complex-objects), as unwanted side effects can creep up as hard-to-debug problems. This refactoring provides general guidelines on how to avoid this problem.

- **[Replace primitive with object](https://github.com/kaiosilveira/replace-primitive-with-object-refactoring)**: Often enough, we start modeling things as simple data records, such as strings and numbers, just to find out later that it wasn't really that "simple". This phenomenon often leads us to implement duplicated code throughout the codebase, especially to perform validations and comparisons. This refactoring helps refactoring in these embarrassing situations.

- **[Replace temp with query](https://github.com/kaiosilveira/replace-temp-with-query-refactoring)**: Sometimes we find it useful to use temporary variables, they help us capture the meaning of involved expressions and also help us to refer to a chunk of computation later. In some situations, though, they're not enough. Sometimes we want to compute a value based on a specific context, so creating multiple temp variables would be less than ideal. In these cases, it's often better to replace the temp with a query.

- **[Extract class](https://github.com/kaiosilveira/extract-class-refactoring)**: Classes inevitably grow, and as they grow, their "single responsibilities" get larger and larger, blurring the idea of "single" as well as the idea of "responsibility". Often enough, we find ourselves looking to a class that has clusters of functionality that speak to separate concerns. In these cases, it's better to extract these concerns into separate classes. The result? Each one of them will be smaller, more focused and less susceptible to bloating.

- **[Inline class](https://github.com/kaiosilveira/inline-class-refactoring)**: As a result of the flexibility we have in healthy codebases, we often move things around to test new ideas, to better isolate responsibilities, and/or to reallocate behavior throughout our layers. Sometimes, though, we go too far. This refactoring helps in these cases where we want to merge two or more classes into a single unit.

- **[Hide delegate](https://github.com/kaiosilveira/hide-delegate-refactoring)**: Encapsulation is at the roots of good code design, it helps in keeping coupling low by hiding the internal details of a given class/module, which allows for better and safer refactorings, and freedom for exploring new ideas (which eventually leads to deeper, more supple domain models). This refactoring sheds some light on how to hide our delegates, therefore keeping our data encapsulated.

- **[Remove middle man](https://github.com/kaiosilveira/remove-middle-man-refactoring)**: Encapsulation comes with a price: many small chunks of code hiding internal details and doing tiny bits of processing here and there. Although it's a good guideline to keep our code encapsulated and modular enough, there's always a blurred space where these guidelines can take us too far. This refactoring helps to revert our path when we find ourselves in this situation.

- **[Substitute algorithm](https://github.com/kaiosilveira/substitute-algorithm-refactoring)**: Sometimes we try our best to improve a piece of code, but it's not enough. Some other times, we take a look at a piece of logic and identify straightaway an easier, more readable way of implementing it. This refactoring helps us in these cases.

### Moving features

So far, all the refactorings were about creating, removing, and editing existing elements, but part of the everyday life of every fine programmer is the habit of thinking about the previous decisions made regarding where to place things and leveraging existing functions and code structures to reduce duplication. The group of refactorings below helps with these concerns.

- **[Move function](https://github.com/kaiosilveira/move-function-refactoring)**: Good software is modular, but modularization isn't by itself static. Finding the right place for functionality is a process of trial and error that involves previous experiences, knowledge depth and, sometimes, gut instinct. This refactoring helps with the cases where we have changed our minds about where to place a function.

- **[Move field](https://github.com/kaiosilveira/move-field-refactoring)**: Fields are the most granular representation of data in an object-oriented world. Nevertheless, defining which classes should contain which fields is often a hard task, even for experienced domain-driven architects. Hopefully, definitions aren't written in stone and, with enough codebase hygiene, moving a field from one class to another is straightforward. This refactoring brings a set of steps and considerations to help with these cases.

- **[Move statements into function](https://github.com/kaiosilveira/move-statements-into-function-refactoring)**: Duplication is one of the easiest code smells to detect and at the same time one of the most difficult ones to remove. This refactoring helps with cases where we know we can encapsulate a sequence of statements into an isolate function, but we can't do it straightaway without breaking callers.

- **[Move statements into callers](https://github.com/kaiosilveira/move-statements-into-callers-refactoring)**: Programmers like encapsulating things that make sense going together, as encapsulation promotes reuse, and as we all know, the premise of reuse is at least a good excuse to have code well-factored in small logical chunks. Sometimes, though, we don't get our boundaries right at first and we need to modify our function's behavior slightly here and there. This refactoring helps in these cases.

- **[Replace inline code with function call](https://github.com/kaiosilveira/replace-inline-code-with-function-call-refactoring)**: Sometimes (and especially in large codebases) it's common for us to find code that is doing essentially the same sequence of computations that we have already encapsulated in a nearby function. In these cases, we can simply replace this block of computation with a function call.

- **[Slide statements](https://github.com/kaiosilveira/slide-statements-refactoring)**: Code is usually read way more times than it is written, and when one is reading code, simple things such as the order in which each statement appears in the code can either aid or inhibit understanding. This tiny refactoring helps in identifying and improving such cases.

- **[Split loop](https://github.com/kaiosilveira/split-loop-refactoring)**: Loops are one of the most fundamental building blocks in every programming language, they are simple and flexible. But, as we know well, flexibility is a two-edged sword: it's really easy to get carried away and end up with bloated loops holding several responsibilities. This refactoring helps get rid of those cases.

- **[Replace loop with pipeline](https://github.com/kaiosilveira/replace-loop-with-pipeline-refactoring)**: Loops are one of the most basic programming constructs and they're present in virtually all programming languages. Sometimes, though, there are more idiomatic and programming language-specific ways to do the same task and accomplish the same results. This helps with migrating to those cases.

- **[Remove dead code](https://github.com/kaiosilveira/remove-dead-code-refactoring)**: Borrowing from the biology world, "evolution" is better seen as a series of adaptations with modifications, with these modifications always focused on optimizing fitness. The side-effects of these modifications, though, are vestigial organs. Codebases also evolve towards better fitness levels, and their equivalent of vestigial organs is dead code: code that doesn't do anything anymore but used to matter at some point in time.

### Organizing data

Our programs are nothing without data: be it internally created or externally imported, holding and manipulating data is an integral part of our day-to-day work as programmers. This section is all about refactorings that help make our code more clear, legible, and precise when it comes to data manipulation.

- **[Split variable](https://github.com/kaiosilveira/split-variable-refactoring)**: Variables are dynamic and may have many uses: holding a value, holding a reference, accumulating a result, and so on. This dynamism, though, can sometimes get in the way we understand code. For a variable to be both useful and understandable, it must be contained inside one, and only one, context, and should also have only one meaning/purpose within this context. This refactoring helps with fixing problems related to the violation of this principle.

- **[Rename field](https://github.com/kaiosilveira/rename-field-refactoring)**: Naming is one of the hardest things in the programming world. No matter how well we know our problem space and our domain, we will often want to rename bits of code here and there to improve clarity, legibility, and to better reflect our updated understanding. This refactoring helps with these cases.

- **[Replace derived variable with query](https://github.com/kaiosilveira/replace-derived-variable-with-query-refactoring)**: In exact sciences, we build new things on top of truth, there's the only way we can be sure our work is correct. Therefore, the source of the truth itself is paramount for all programs, and it's not difficult to understand why having multiple sources of truth brings additional confusion and complexity. This refactoring helps remove these multiple sources.

- **[Change reference to value](https://github.com/kaiosilveira/change-reference-to-value-refactoring)**: Mutability is one of the most important aspects to be aware of in any software program. Ripple effects can cause hard-to-debug problems and flaky tests but, sometimes, they're exactly what we're expecting to happen. This refactoring helps in cases where we want our underlying objects (or data structures) to be immutable, therefore avoiding rippling side effects and leveraging the **[Value Object](https://github.com/kaiosilveira/poeaa-value-object)** pattern.

- **[Change value to reference](https://github.com/kaiosilveira/change-value-to-reference-refactoring)**: Mutability is one of the most important aspects to be aware of in any software program. Ripple effects can cause hard-to-debug problems and flaky tests but, sometimes, they're exactly what we're expecting to happen. This refactoring helps in cases where we want our underlying objects (or data structures) to be mutable, often by protecting them and providing a standard, centralized, and memory-efficient way of retrieving them.

### Simplifying conditional logic

Conditionals play a central role in every program: it'd be really hard to implement anything without them. Unfortunately, the complexity that comes out of complicated conditionals can make it hard for the readers to reason about the code they're staring at. This section brings refactorings focused on working on conditionals to make them easier to understand, more communicative, and more fluid.

- **[Decompose conditional](https://github.com/kaiosilveira/decompose-conditional-refactoring)**: Conditionals can get tricky fast, be it because of the length of each conditional leg, because of the rules applied to each branch, or because of the intrinsic nature of the conditions. This refactoring helps bring clarity to all these cases.

- **[Consolidate conditional expression](https://github.com/kaiosilveira/consolidate-conditional-expression-refactoring)**: Oftentimes we see conditions that evolve in the form of new rules being added to an already existing chain of validations. Sometimes, though, these conditions are only extensions to the already stated conditions. In these cases, consolidating them into a single condition helps with readability and sets the stage for other refactorings.

### Appendix: Useful commands when cloning this codebase locally

- fetch all submodules after cloning this repository for the first time in a machine:

```bash
git submodule update --init --recursive
```

- update all submodules to their respective `HEAD`s:

```bash
git submodule update --recursive --remote
```
