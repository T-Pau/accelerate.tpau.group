---
layout: page
title: convert-characters
---
### Convert images to charset and screens.

## Synopsis

```sh
convert-characters [-M file] [-o file] [-I directory] [-s section] [-v] specs
```

## Description

Convert images to charsets and screens according to specification file `specs`, which lists the images to convert and how to convert them.

Images are divided into fixed sized blocks. Distinct blocks are stored in the charset and a screen matrix listing the block indices is created.

It generates an assembler source file containing all the created objects.


## Supported Options

`-h`, `--help`
:   Show help message and exit.

<code>-I <em>directory</em></code>
:   Search for included files in `directory`. Can be given multiple times.

<code>-M <em>file</em><code>
: Write gcc-style dependency information to `file`.

<code>-o <em>file</em></code>
:   Write generated assembly code to `file`.

<code>-s <em>section</em></code>, <code>--section <em>section</em></code>
:   Put generated objects in section `section`.


## Specification File

The specification file uses [YAML](https://yaml.org) syntax.


### Global Directives

`charset`
: [Charset definition](#charset-directives) or list of [charset definition](#charset-directives) describing the created charsets. (required)

`characters`
: List of [character definitions](#character-directives) describing images containing characters to convert.

`images`
: List of [image definitions](#image-directives) describing the images to convert.

<code>section <em>name</em></code>
: Section to place created objects in. (default: `data`)

<code>screen_width <em>number</em></code>
: Specify the default width of the screen. (default: none)


### Charset Directives

This describes the charsets to be created.

<code>align <em>alignment</em></code>
: Align created object to `alignment` bytes. This is ignored if runlength-encoding is used. (default: no alignment)

<code>count <em>count</em></code>
: Charset contains `count` characters. (default: 256)

<code>height <em>height</em></code>
: Characters are `height` logical pixels high. (default: 8)

<code>name <em>string</em></code>
: Specify name of object created for this charset. (required)

`predefined`
: Map of character codes to hex strings. These characters will be defined before processing characters and images. (default: none)

<code>rl-encode <em>boolean</em></code>
: Wether to runlength-encode the charset object (default: false).

<code>section <em>section</em></code>
: Section to place charset object in. (default: global `section`)
<code>width <em>width</em></code>
: Characters are `width` logical pixels wide (default: 8).

### Character Directives

This describes images containing character sets to include in the created charsets. The contained characters are placed in order, and no screen object will be created.

`additional_palette`
: A map of color values to palette indices which will be added to the palette. (default: none)

<code>charset <em>number</em></code>
: The characters will be stored in charset `number` (default: 0)

<code>char-size-x <em>number</em></code>
: The width of logical characters in chars. (default: 1)

<code>char-size-y <em>number</em></code>
: The height of logical characters in chars. (default: 1)

<code>file <em>name</em></code>
: The name of the file containing the image.

<code>inverted <em>enum</em></code>
: (default: `none`)

    `create`
    :   Create inverted characters for this image.

    `none`
    : No inverted characters are needed for this image.

    `present`
    : This image contains inverted characters.

<code>offset <em>number</em></code>
: The characters in the image will be placed starting at position `number` in the created charset. (default: 0)

`palette`
: A map of color values to palette indices. (default: white: 1, black or transparent: 0, 0x40ff40: skip)

<code>pixel-size-x <em>size</em></code>
: Treat `size` horizontally adjacent pixels (which must be of the same color) as one logical pixel, useful for image formats with non-square pixels. (default: 1)

<code>pixel-size-y <em>size</em></code>
: Treat `size` vertically adjacent pixels (which must be of the same color) as one logical pixel, useful for image formats with non-square pixels. (default: 1)


### Image Directives

This describes an image to encode in the created charsets. A screen object will be created to record the character indices used.

Optionally, the image can be sliced to create multiple screen objects.

`additional_palette`
: A map of color values to palette indices which will be added to the palette. (default: none)

`charset`
: If this is a number, the image will be stored in that charset. If it is a map, the keys are line numbers and the values in which charset to store the portion of the image staring at that line. (default: 0)

<code>file <em>name</em></code>
: The name of the file containing the image.

<code>include-count <em>boolean</em></code>
: For a sliced image, include an object containing the number of screen objects created. (default: true)

<code>include-index <em>boolean</em></code>
: For a sliced image, include an object containing pointers to the screen objects. (default: true)

<code>inverted <em>enum</em></code>
: (default: `none`)

    `create`
    : Create inverted characters for this image.

    `if-different`
    : Store inverted character if it is different from no-inverted one.

    `none`
    : No inverted characters are needed for this image.

    `present`
    : This image contains inverted characters.

<code>name <em>string</em></code>
: Name of the created screen or index object. (required)

<code>names <em>string-list</em></code>
: For sliced images, a list of names for the screen object. 

`palette`
: A map of color values to palette indices. (default: white: 1, black or transparent: 0, 0x40ff40: skip)

<code>pixel-size-x <em>size</em></code>
: Treat `size` horizontally adjacent pixels (which must be of the same color) as one logical pixel, useful for image formats with non-square pixels. (default: 1)

<code>pixel-size-y <em>size</em></code>
: Treat `size` vertically adjacent pixels (which must be of the same color) as one logical pixel, useful for image formats with non-square pixels. (default: 1)

<code>rl-encode <em>boolean</em></code>
: Wether to runlength-encode the screen object. (default: true)

<code>screen-file <em></em></code>
: 

<code>screen-width <em>width</em></code>
: The logical width of the screen for this image. If the image is narrower, the remaining bytes will be skipped in the runlength-encoded screen object (default: global <code>screen-width</code>).

<code>slice-x <em>number</em></code>
: Divide the image into `number` slices horizontally, creating a separate screen object for each slice. (default: 1)

<code>slice-y <em>number</em></code>
: Divide the image into `number` slices vertically, creating a separate screen object for each slice. (default: 1)

<code>trim <em>enum</em></code>
: Specifies at which end of the encoded screen object skips will be omitted (default: `trailing`).

    `none`
    : Don't omit any skips.

    `both`
    : Omit skips at the beginning and end.

    `leading`
    : Omit skips at the beginning.

    `trailing`
    : Omit skips at the end.
