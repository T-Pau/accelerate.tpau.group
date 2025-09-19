---
<<<<<<< HEAD
title: Accelerate
---
## What is Accelerate?
=======
title: Toolkit
---
## What Is Toolkit?
>>>>>>> bd0a15b (Flesh out.)

Toolkit is a collection of scripts, Python classes, assembler routines and build rules for developing programs for 8- and 16-bit computers. 

It is designed to be used with [Accelerate](accelerate.tpau.group) and [fast-ninja](fast-ninja.tpau.group).

## Why Use Toolkit?

<<<<<<< HEAD
**This program is still in the early stages of development. It contains bugs and features that aren't completely implemented yet. Also, details may still change in backwards incompatible ways.**

## Why Use Accelerate?

Accelerate is especially useful if you develop for multiple CPU architectures, since it provides a uniform environment.

If your program has a complicated memory layout, Accelerate helps fulfilling them without having to do it all manually.

## Getting Started

First, [build and install](https://github.com/T-Pau/Accelerate/blob/main/INSTALL.md) Accelerate.

Then, assemble your program:

<code>xlr8 -o <em>program</em> -t <em>target</em> <em>sources ...</em></code>

## Reporting Problems

It's an old adage that it's never the compiler's fault, but since Accelerate is still young, bugs and misfeatures are likely. If you found a problem, please [create an issue on GitHub](https://github.com/T-Pau/Accelerate/issues/new/choose) or let us know at [accelerate@tpau.group](mailto:accelerate@tpau.group).
=======
Toolkit was primarily developed for use in our own programs.

While the scripts creating assembly code, the provided assembler routines, and the build rules are specific to Accelerate and fast-ninja, some scripts and the underlying Python classes are more versatile.

## Getting Started

Include Toolkit as a submodule in your Git repository:

```sh
git submodule add https://github.com/T-Pau/Toolkit Toolkit
```

## Staying in Touch

If you found a problem, please [create an issue on GitHub](https://github.com/T-Pau/Toolkit/issues/new/choose) or let us know at [toolkit@tpau.group](mailto:toolkit@tpau.group).
>>>>>>> bd0a15b (Flesh out.)

Also let us know if the documentation is incomplete, inaccurate, or hard to understand.
