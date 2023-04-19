# jx boolean type

> **[Back to data types](datatypes.md)**

The boolean type represents true or false. `true` and `false` are [built-in variables](variables.md#builtin-variables).

By default, they are not case sensitive. `true`, `TRUE` and `True` are all allowed. If this is disabled, lowercase `true` and `false` must be used. 

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

> Unlike JSON: the case of `true` and `false` does not matter

> Recommended style: use lowercase `true` and `false`
