---
layout: page
title: Documentation
sidebar_order: 2
---
# Introduction

Accelerate is a cross-assembler that runs on modern computers. It translates programs for retro computers written in assembly language into binaries that can run on those computers. It supports a variety of computers and CPU families, and adding support for even more is relatively easy and can be done without changing and recompiling the assemlber itself.

All source, definition, and intermediary files are text files, although the latter are not meant to be edited by hand.

The program is organized into **source files** and **libraries**, which contain **objects**, **constants**, **macros**, and **functions**. The assembly process is directed by a  **target definition**, which references a **CPU definition**. 

# Assembler Soruce File

A source file contains definitions that will be translated to an **object file**, a **library** or a **binary program**.

It consists of a list of elements and directives, separated by newlines. Instructions, labels, and assignments within an **object** or **macro** are also separated by newlines.

## Directives

### `.section`

```
.section <section-name>
```

The `.section` directive specifies which section the following objects will be placed in. This determines where in memory they will be placed, and whether they will be included in the **binary program** (**data objects**) or not (**reservation objects**).

### `.scope`

```
.scope [public|private]
```
The `.scope` directive specifies the visibility of the following elements. `private` elements are only visible within the same module (library or the main program). `public` elements are also visible to modules using a library.

### `.target`

```
.target "<target-name>"
```

The `.target` directive specifies which **target definition** file will be used in translating the file.

## Elements

### Objects

There are two kinds of objects:

**Data objects** contain code or other data and are saved in the resulting program binary:

```
[.private|.public] <name> [.align <alignment>] [.address <address>] {
    <body>
}
```

**Reservation objects** only reserve a certain amount of memory, which will not be initialized and is usually not saved in the resulting program binary:

```
[.private|.public] <name> [.align <alignment>] [.address <address>] .reserve <length>
```

### Constants

Constants define values that can be used in other parts of the program. They can refer to other constants or objects (which results in their address).

```
[.private|.public] <name> = <value>
```

### Macros

```
[.private|.public] .macro <name> [<argument>[=<default-value>], ...] {
    <body>
}
```

### Functions

```
[.private|.public] <name> ([<argument>[=<default-value>, ...]) = <value>
```

# Library

A library is a collection of **objects**, **constants**, **macros**, and **functions**. 

When importing a library, only the objects actually used will be included in the resulting binary program. This gives you flexibility in how you organize your code: even using only a small portion of a large library does not sacrifice efficiencey.

# Target Definition

# CPU Definition
