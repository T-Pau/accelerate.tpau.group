# Assembler Source File

A source file contains definitions that will be translated to a **library** or a **binary program**.

It consists of a list of elements and directives, separated by newlines. Instructions, labels, and assignments within an **object** or **macro** are also separated by newlines.

## Directives

### `.cpu`

```
.cpu <name>
```

### `.pin`

```
.pin <name> <address>
```

### `.section`

```
.section <section-name>
```

The `.section` directive specifies which section the following objects will be placed in. This determines where in memory they will be placed, and whether they will be included in the **binary program** (**data objects**) or not (**reservation objects**).


### `.target`

```
.target "<target-name>"
```

The `.target` directive specifies which [**target definition**](Target-Definition.md) file will be used in translating the file.


### `.use`

```
.use <name>
```

### `.visibility`

```
.visibility [public|private]
```
The `.visibility` directive specifies the visibility of the following elements.

`private` elements are only visible within the same module (library, main program, or target).

`public` elements are also visible to modules using a library. Elements from the program used by the target must also be `public`.


## Elements

### Objects

There are two kinds of objects:

**Data objects** contain code or other data and are saved in the resulting program binary:

```
[.default] [.private|.public] <name> [.align <alignment>] [.address <address>] [.used] [.uses <name>] {
    <body>
}
```

**Reservation objects** only reserve a certain amount of memory, which will not be initialized and is usually not saved in the resulting program binary:

```
[.default] [.private|.public] <name> [.align <alignment>] [.address <address>] [.used] [.uses <name>] .reserve <length>
```

If `.default` is specified, this definition is only used if no regular definition is found. This can be used by a target to define a default implementation.

`.private` or `.public` override the visibility specified by [`.visibilty`](#visibility).

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
