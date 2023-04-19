# jx color type

> **[Back to data types](datatypes.md)**

A color is just an integer number. However, it's useful to think of it as a separate type because jx provides a number of features to manipulate colors.

Two color formats are supported. The user must decide which one they are using, and consistently use the same format:

- RGB (red, green and blue channels)
- ARGB (alpha, red, green, blue channels)

Colors are usually expressed as a hex color string or as a hexadecimal number. Note that expressing an ARGB color value can't be done as a hex color value. Hexadecimal notation should be used.

```js
// @jx-version: 1.0
[
    #33bb77,    // RGB as Hex color string
    #3b7,       // RGB as shorthand hex string (equivilant of #33bb77)
    0x33bb77,   // RGB as hexadecimal integer
    0xff33bb77, // ARGB as hexadecimal integer
]
```

While it is possible to express a number as a standard integer it is not recommended as it is hard to read. It does not give a user any clues that the value represents a color.

```js
[
    #a3ff10,  // hex color value
    10747664, // the same value as an integer. Will work, but not recommended!
]
```

Although technically just an integer number, colors can be specified too. See (Color datatype)[datatype-color.md] for more information.

> Unlike JSON: jx can represent and work with color values
