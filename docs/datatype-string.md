# jx string type

> **[Back to data types](datatypes.md)**

Strings can be specified using single `'` or double `"` quotes.

A string can span multiple lines. Any whitespace, including newlines, are included in the string. By default, any single leading and/or trailing empty line is trimmed. This can be disabled in the parser. 

A single-quoted string may contain double-quotes and vice-versa. To include a single quote in a single quoted string, escape the quote with a backslash. The same can be done to include a double quote in a double-quoted string.

```js
// @jx-version: 1.0
[
    'Hello',
    "World",
    'A "mushroom" cloud',
    'A \'mushroom\' cloud',

    // The following two strings are identical
    '
All children, except one, grow up.
They soon know that they will grow up, and the way Wendy knew was this.
    ',
    'All children, except one, grow up.
They soon know that they will grow up, and the way Wendy knew was this.',
]
```

> Unlike JSON: strings in jx can be surrounded by single quotes as well as double quotes

> Recommended style: Use single quotes for strings
