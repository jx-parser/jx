# jx number type

> **[Back to data types](datatypes.md)**

A number can either be an integer value, like `12`, or a decimal value, like `12.5`. The actual underlying datatype for each of these depends on the language implementation, but in most cases an integer will be stored as a signed `int` and a decimal will be a `double`.

In a jx file, numbers can be represented several ways:

```js
// @jx-version: 1.0
[
    100,      // Integer
    -78,      // Negative integer
    3.14159,  // Decimal
    0xFF31B,  // Hexadecimal
    0b1011110110, // Binary
    1234E+7,  // Scientific notation
    #28ff60,  // Hex color value (see notes below)
]
```

Colors are technically just integers. However, colors can be specified in other ways, too. See (Color datatype)[datatype-color.md] for more information.

> Unlike JSON: numbers in jx can be represented in a variety of ways
