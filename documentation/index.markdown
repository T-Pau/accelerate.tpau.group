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

A source file contains definitions that will be translated to a **library** or a **binary program**.

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

# Expressions

Expressions are used in various places in a program.

## Types

### Boolean

A boolean is either `true` or `false`. Numbers and strings can be implicitly converted to boolean, with `0` and the empty string being `false` and all others being `true`.

### Signed Integers

A signed integer with a maximum precision of 64 bits, giving a value range of -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807.

### Unsigned Integer

An unsigned integer with a maximum precision of 64 bits, giving a value range of 0 to 18,446,744,073,709,551,615.

For hexadecimal numbers, prefix them with `$`, for binary numbers with `%`.

### Floating Point Number

A floating point number expressed as a **double**.

Scientific notation (including an exponent) is not supported yet.

### String

Strings are enclosed in `"` and support Unicode characters.

## Operators

### Unary Operators

All unary operators are followed by their operand and have higher precedence than binary operators.

#### `!` (Logical Not)

Converts its operand to boolean and negates it. The result is always boolean.

#### `+` (Unary Plus)

The operand must be a number, the result is the operand unchanged.

#### `-` (Unary Minus)

The operand must be a number, the result is the negative operand. 

#### `^` (Bank Byte)

The operand must be an unsigned integer, the result is the third lowest byte of the operand.

#### `<` (Low Byte)

The operand must be an unsigned integer, the result is the lowest byte of the operand.

#### `>` (Hight Byte)

The operand must be an unsigned integer, the result is the second lowest byte of the operand (high byte of a 16-bit value).

#### `~` (Bitwise Not)

The operand must be an unsigned integer, the result is the operand with each bit inverted.

### Binary Operators

All binary operators are left associative. The following list shows their relative precedence from high to low; operands on one line have the same precedence:

- `<<` (Shift Left), `>>` (Shift Right)
- `*` (Multiply), `/` (Divide), `&` (Bitwise And)
- `+` (Add), `-` (Subtract), `|` (Bitwise Or), `^` (Bitwise Exclusive Or)
- `=` (Equal), `!=` (Not Equal), `<` (Less), `<=` (Less Than), `>` (Greater), `>=` (Greater Than)
- `&&` (Logical And)
- `||` (Logical Or)

#### `<<` (Shift Left)

Both operands must be integers. The left operand is shifted to the left by the number of bits of the value of the right operand. The result has the same type as the left operand.

#### `>>` (Shift Right)

Both operands must be integers. The left operand is shifted to the left by the number of bits of the value of the right operand. The result has the same type as the left operand.

#### `*` (Multiply)

Both operands must be numbers. The result is the product of the two operands. If both operands are integers, the result is an integer, otherwise a float point number.

#### `/` (Divide)

Both operands must be numbers. If both operands are integers, the result is the quotient of the two operands, otherwise a floating point division is performed.

#### `&` (Bitwise And)

Both operands must be unsigned integers. The result is the bitwise and of the two operands.

#### `+` (Add)

Both operands must be numbers. The result is the sum of the two operands. If both operands are integers, the result is an integer, otherwise a float point number.

#### `-` (Subtract)

Both operands must be numbers. The result is the difference of the two operands. If both operands are integers, the result is an integer, otherwise a float point number.

#### `|` (Bitwise Or)

Both operands must be unsigned integers. The result is the bitwise or of the two operands.

#### `^` (Bitwise Exclusive Or)

Both operands must be unsigned integers. The result is the bitwise exclusive or of the two operands.

#### `!` (Equal)

Both operands must be either booleans, numbers, or strings. The result is `true` if both operands are the same, `false` otherwise.

#### `!=` (Not Equal)

Both operands must be either booleans, numbers, or strings. The result is `false` if both operands are the same, `true` otherwise.

#### `<` (Less)

Both operands must be either numbers or strings. The result is `false` if the left operand is less than the right, `true` otherwise.

#### `<=` (Less Equal)

Both operands must be either numbers or strings. The result is `false` if the left operand is grelessater than or equal to the right, `true` otherwise.

#### `>` (Greater)

Both operands must be either numbers or strings. The result is `false` if the left operand is greater than the right, `true` otherwise.

#### `>=` (Greater Equal)

Both operands must be either numbers or strings. The result is `false` if the left operand is greater than or equal to the right, `true` otherwise.

#### `&&` (Logical And)

Both operands are converted to boolean. The result is `true` if both operands are true, `false` otherwise.

#### `||` (Logical Or)

Both operands are converted to boolean. The result is `true` if at least one operand is true, `false` otherwise.
