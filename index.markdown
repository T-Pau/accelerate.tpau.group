---
layout: home
is_main_page: true
---
## Introduction

Accelerate is a flexible cross-assembler running on modern systems targeting multiple 8- and 16-bit computers. Support for additional CPU types, memory models, and output formats can easily be added via configuration files.

Programs are divided into small objects like a single subroutine or variables. These objects are automatically arranged in memory according to provided constraints like alingment or placement within a certain memory region. This allows to fulfil the hardware requirements of the target machine while automatically using the remaining memory optimally.

## Warning

**This program is still in the early stages of development. It contains bugs and features that aren't completely implemented yet. Also, details may still change.**

## Rationale

While developing programs for multiple 8-bit computers, existing cross assemblers did not fit my needs in some key regards: 

- They usually only target one CPU family, and all have slightly different syntax.
- They don’t allow precise control where objects are placed in memory when necessary, while efficiently using the remaining memory automatically.
- Adding a new CPU variant requires understanding the inner workings of the assembler, changing and recompiling the assembler, and either getting the change accepted upstream or maintaining a fork.
- Not all assemblers support automatic dependency tracking of included files and used libraries, which makes integrating them into a build system much harder.

To address these issues, Accelerate was created.

CPU definitions are text files, so it is easy to add a new CPU family without altering the assembler itself. Definitions for variants can include the definition of the main CPU model and only specify the differences.

The output file format is also defined in a text file, allowing to add new platforms and their requirements. File formats that further encode the binary data (like tape or disk images) need a separate program to do the final conversion.

The program itself is divided into small objects, one for each function, variable, or table. This allows the assembler more flexibility in placing them in memory. For each object, the constraints imposed by the platform can be specified, for example a fixed address, address alignment, or placement in a specific memory region. Gaps left by placing them are filled with other small objects.

This also allows libraries to be more efficient, since only the objects actually used will be included in the program.

## Design Goals

These are the design goals of Accelerate, in order of importance:

### 1. Allow extending its functionality easily.

Accelerate is a generic assembler without built-in knowledge about any CPU architecture, memory layout, or output file format. All these aspects are defined in easily readable configuration files.

A definition file can extend another, thus allowing to adapt existing platform support to your needs with a minimum of effort.

This allows adding support for new target computers without modifying Accelerate itself. 

### 2. Allow precise control over the produced binary when needed, without requiring it when not.

You can specify the exact address where to place an object, or you can specify a memory region and alignment (i. e. the address must be divisible by a certain number). Accelerate will make sure the requirements are met, while placing the other elements as efficiently as possible.

### 3. Meet modern expectation of a programming tool.

Accelerate is written in C++20 and should work on all modern platforms. It is a command line tool that follows the usual Unix conventions.

Accelerate integrates well with development and build environments: If it encounters an error, it will not produce output files and exit with a non-zero status. Also, it supports gcc-style dependency tracking.

Accelerate supports reproducible builds: if given the same input files, it will create byte-wise identical results. This means that if a program is built from the same source on two different computers, the resulting program will be identical.