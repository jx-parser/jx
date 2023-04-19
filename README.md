# About jx

jx (short for JSON eXtended, file extension `.jx`) is a file format that extends [JSON](https://www.json.org/json-en.html). jx was designed specifically for configuration files, but has a wide range of potential applications. jx is a super-set of JSON that supports core JSON but adds many more powerful features. For example:
- Inline and block comments
- Keys without quotes
- Single and double quoted strings
- Equations
- Variables, functions and user-defined functions
- String, Array and Object operations (combine, add, remove etc)
- Color support and manipulation
- [Check out an example jx file](examples/example.jx)

This document is a summary of the full jx specification. Instead:

> **[Read the documentation for the full jx specification](docs/jx.md)**
> **[Read the documentation for the jx-lite specification](docs/jx-lite.md)**

# About this repository

This is the home of the jx file format specification and roadmap.

To use jx with your choosen coding language, see the various implementations:

- [dart](https://github.com/jx-parser/jx-dart/) - this is the reference implementation
- [haxe](https://github.com/jx-parser/jx-haxe/) - works, but out of date and a little buggy

# Licence

The jx format is provided under the MIT licence (free for any use, personal or commercial, without attribution, but also without warranty).

# Features

- [File structure](#file-structure)
- [Data types](#data-types)
    - [Number](#number)
    - [String](#string)  
    - [Boolean](#boolean)
    - [Color](#color)
    - [null](#null)
    - [object](#object)
    - [array](#array)
- [Comments](#comments)
    - (Single-line comments)[#single-line-comments]
    - (Block comments)[#block-comments]
- [Constants](#constants)
    - [Built-in constants](#built-in-constants)
    - [User-defined constants](#user-defined-constants)
- [Variables](#variables)
    - [Declaring variables](#declaring-variables)
    - [Referencing variables](#referencing-variables)
    - [User-defined variables](#user-defined-variables)
    - [Default values for variables](#default-values-for-variables) 
- [Functions](#functions)
    - [Built-in functions](#built-in-functions)
    - [User-defined functions](#user-defined-functions)
- [Expressions](#expressions)
    - [Operators](#operators)
- [Official style guide](styleguide.md)
- [Road map](roadmap.md)

## File structure

The jx file format is a super-set of [JSON](https://www.json.org/json-en.html). Start by becoming familiar with JSON - most of the documentation assumes a good understanding of the JSON format.

A jx file must have a single element at it's root. That element can be either an `object`, an `array` or an `expression`. The following are all valid jx files.

```js
// This is an object
{
    name: 'jx'
}
```

```js
// This is an array
[
    'jx',
    'JSON',
]
```

```js
// This is an expression (evaluates to a value, object or array)
sin(32) + 17
```

> Unlike JSON, jx can contain a single expression or value at it's root.

## Data types

### number

A number can be represented in several formats. A number is not surrounded by quotes.

Examples:

```js
[
    100, // int
    100.26 // double
    0xff56a1002, // hex
    0b1011101101, // binary
]
```

> **[Read the full documentation for the number data type](docs/number.md)**

### string

A string is similar to a string in JSON, except it can be surrounded by single `'` or double `"` quotes.

```js
[
    'This is valid',
    "This is also valid",
]
```

A string may contain quotes of the other type (e.g. a single-quoted string may contain double quotes), or the quotes can be escaped.

```js
[
    'This is "quoted"',
    "This is also 'quoted'",
    'This another \'quoted\' string',
    "This is also a \"quoted\" string",
]
```

Strings may flow across multiple lines. The string will include the newline characters and any whitespace. However, jx will automatically trim the first and last lines if they contain only whitespace. This option can be disabled.

```js
[
    // These two strings are identical

    "Mrs. Darling loved to have everything just so, and Mr. Darling
had a passion for being exactly like his neighbours; so, of 
course, they had a nurse. As they were poor, owing to the 
amount of milk the children drank, this nurse was a prim 
Newfoundland dog, called Nana, who had belonged to no one in 
particular until the Darlings engaged her.",

    "
Mrs. Darling loved to have everything just so, and Mr. Darling 
had a passion for being exactly like his neighbours; so, of 
course, they had a nurse. As they were poor, owing to the 
amount of milk the children drank, this nurse was a prim 
Newfoundland dog, called Nana, who had belonged to no one in 
particular until the Darlings engaged her.
    ",
]

```

There's a lot more jx can do with strings:

- Combine strings
- Add strings to arrays
- Combine other values into a string


> **[Full documentation for the string data type](docs/string.md)**
> **[Functions that work with the string data type](docs/builtins.md#string)**

### boolean

jx, like JSON, supports the boolean values `true` and `false`. Boolean values are not currounded by quotes. Case is ignored.

```js
[
    true,
    false,
    True,
    FALSE,
]
```

> **[Read the full documentation for the boolean data type](docs/boolean.md)**

### color

A color can be represented in a couple of ways, but always evaluates to a number. The following are valid color formats:

- #ff9900 (hex color format RGB)
- #f90 (shorthand hex color format RGB)
- 0xff9900 (hex number RGB)
- 0xffff9900 (hex number ARGB)

The jx format supports many built-in color functions that manipulate colors directly within the file, like `lighten`, `darken` and `tint`.

> **[Read the full documentation about colors](docs/color.md)**
> **[Functions that work with colors](docs/builtins.md#color)**

### null

A special constant that usually represents 'no value'. Case is ignored.

```js
[
    null,
    Null,
    NULL,
]
```

> **[Read the full documentation about null](docs/constants.md#null)**

### Object

Like JSON, the jx object is represented by curly braces `{...}` and must contain key-value pairs. However, keys in an object may be un-quoted (i.e. they do not need to be surrounded by quotes, like a string). Key-value pairs are seperated by a colon `:` (the assignment operator).

```js
{
    name: 'jx';
    fullName: 'JSON extended';
    extension: '.jx';
    'data types': [
        'object',
        'array',
        'number',
        'string',
        'boolean',
        'null'
    ];
}
```

> The official jx style guide recommends unquoted keys

> The official jx style guide recommends semicolons to seperate key-value pairs in an object, including a trailing semicolon after the last element

The jx format supports many advanced object operations, such as:
- Combining objects
- Removing or adding elements from objects

> **[Read the full documentation about the object data type](docs/object.md)**

### Array

Just like in JSON, an array is a list of values surrounded by square brackets. An array can be empty, or can contain any number of elements. Elements in an array can be a mixture of any data type.

Elements in an array may be seperated by a comma `,` or a semicolon `;`. A trailing seperator (afetr the final element) is optional, but allowed.

> The official jx style guide recommends comma seperators in an array, with a trailing comma after the last element

```js
[
    1.234,
    'String',
    NULL,
    1234,
]
```

```js
[
    1.234;
    'String';
    NULL;
    1234;
]
```

> Unlike JSON, jx array elements can be seperated by a semicolon `;` or a comma `,` (JSON supports comma only)

The jx format supports many advanced array operations, such as:
- Combining arrays
- Diffing arrays
- Removing or adding elements from arrays

> **[Read the full documentation about the array data type](docs/array.md)**

## Comments

Two different comment types are supported in jx files. Comments are treated as whitespace and are ignored. They can occur anywhere that whitespace occurs. Be careful where you place comments, though, because they can result in hard-to-read code. Just because you can doesn't mean you should!

### Single-line comments

Single-line comments start with two forward slashes `//`. Anything after these slashes until the end of the line are part of the comment. This means that you cannot put any jx code after the slashes.

```js
// Comments at the top of the file before the first element are fine 
[
    // This is a comment within an element. Like all comments, it's ignored by the parser
    name: 'jx', // A comment at the end of a line is ok too
]
```

### Block comments

Block comments start with `/*` and end with `*/`. Anything in between is part of the comment. Any jx code can be placed before or after the comment. A block comment can span multiple lines.
```js
/*
 * Block comments are great for headers, or any larger
 * blocks of documentation. They can span many lines.
 */
[
    /* This is a block comment within an element. Like all comments, it's ignored by the parser */
    name: 'jx', /* A block comment at the end of a line is ok too */

    /* The following example shows what is possible with
       block comments, but is NOT recommended practice, as
       it makes the file difficult to read! */
    /* Before key */ example /* After key */ : /* Before value */ 'value' /* After value */ 
]
```

> The official jx style guide recommends single-line comments at the end of lines and block comments for larger sections of documentation

## Constants

A constant is a predefined value that you can refer to by name. JSON and jx both support the constants `true`, `false` and `null`.

### Built-in constants

The following constants are available to use within jx files.

- **true** (*boolean*, true)
- **false** (*boolean*, false)
- **null** (*null*)
- **pi** (*number*, approx 3.14159)
- **pi_2** (*number*, pi divided by 2)
- **inv_pi** (*number*, 1 divided by pi)

### User-defined constants

Custom constants can be provided before parsing a jx file. How this is done depends on the implementation. Constants are single words. They can contain (and start with) letters, digits, underscore, and various other characters (like `#`,`.`). They may not contain `-` or `:`.

```js
// The following are all valid constant names
[
  first,
  second#,
  _third,
  four_th,
  FIFTH5,
  6th,
]
```

> The official jx style guide recommends lower snake case, e.g. `lower_snake_case` 

## Variables

A variable represents a value that can be referenced further down in the jx file. Once a variable is declared, it can be substituted elsewhere in the file. Variables have global scope within a file. It doesn't matter if they were declared in a nested structure - once they are declared they can be referenced anywhere.

### Declaring variables

A variable is declared as a key-value pair. The key is the variable name, and the value is what the variable contains.

A variable name starts with a dollar sign `$` followed by the name of the variable. They can contain (and start with) letters, digits, dash, underscore, and various other characters (like `#`,`.`). They may not contain `:`.

The variable name is followed by the assignment seperator `:` (colon) and then the value of the variable. The value can be any supported jx data type, including number, boolean, array, object, equation - even another variable. 

```js
// The following are all valid variable names
{
  $first: 1,
  $-second: 'second',
  $_third: [1, 2, 3],
  $four_th: { name: 'jx', supported: true },
  $FIFTH5: 5.0,
  $6th: true,
  $seven#th: $first,
  $eigthVar: null,
}
```

> The official jx style guide recommends lower pascal case, e.g. `$myVariable` or `$bigCameraBag`

### Referencing variables

A variable can be referenced by using it anywhere that a value would be used. Here are some examples:

```js
{
    $extn: '.jx',
    description: 'The jx file extension is ' + $extn,
}
```

```js
{
    data: {
        $foo: 30,
    },
    bar: $foo 
}
```

### User-defined variables

Variables can be defined before parsing a jx file. They are then available within that file. This is a very powerful feature of jx that makes them perfect for settings or config files.

The following example creates a theme file based on a couple of variables that are passed in. You may also need to refer to the section on [functions](#functions) to understand what's going on.

```js
// User passes in 
//   $baseColor = #4D6978
//   $baseFont = Arial
//   $baseFontSize = 16
{
    $headingFontSize: $baseFontSize * 1.4;
    bgColor: darken($baseColor, 0.8);
    borderColor: lighten($baseColor, 0.5);
    panelColor: $baseColor;
    body: {
        fontFamily: $baseFont;
        weight: 'normal';
        size: $baseFontSize;
        color: lighten($baseColor, 0.5);
    };
    heading: {
        fontFamily: $baseFont;
        weight: 'bold';
        size: $headingFontSize;
        color: lighten($baseColor, 0.5);
    };
}
```

### Default values for variables

jx allows variables to have default values. The default value is used if no user-defined variable is passed in. Preceed the variable declaration with a question mark `?` to specify a default value. For example `?$name: 'jx'`.

The following is a repeat of the previous example using default values. Note that `$headingFontSize` is not declared with a default value. If the user passed in a different value for `$headingFontSize` it will always be overwritten.

```js
// User passes in 
//   $baseColor = #4D6978
//   $headingFontSize = 20
// The final values are
//   $baseColor = #4D6978 (user-defined)
//   $baseFont = Calibri (default)
//   $baseFontSize = 12 (default)
//   $headingFontSize: 12 * 1.4 (not a default, so will not use user-defined value)
{
    ?$baseColor = #B04F07;
    ?$baseFont = Calibri;
    ?$baseFontSize = 12;
    $headingFontSize: $baseFontSize * 1.4;
    bgColor: darken($baseColor, 0.8);
    borderColor: lighten($baseColor, 0.5);
    panelColor: $baseColor;
    body: {
        fontFamily: $baseFont;
        weight: 'normal';
        size: $baseFontSize;
        color: lighten($baseColor, 0.5);
    };
    heading: {
        fontFamily: $baseFont;
        weight: 'bold';
        size: $baseFontSize * 1.4;
        color: lighten($baseColor, 0.5);
    };
}
```

## Functions

Functions take inputs and generate a new output. The output from a function is always one of the supported data types (object, array, number, string etc). There are many built-in functions, but jx also allows user-defined functions to provide even more functionality! a function name is immediately followed by parenthesis which contain any number of arguments (inputs). For example `doSomething(10, 'apples')`. When the jx file is parsed, the function is replaced by the output value of that function.

### Built-in functions

The following mathmatical functions are available within a jx file:

- **min(number, number)** return the minimum of the two arguments
- **max(number, number)** return the maximum of the two arguments
- **floor(number)** return the number rounded down to the nearest integer value
- **ceil(number)** return the number rounded up to the nearest integer value
- **round(number)** return the number rounded up or down to the nearest integer value
- **cos(number)** calculate the cosine of the argument. The argument is in radians
- **sin(number)** calculate the sine of the argument. The argument is in radians
- **tan(number)** calculate the tangent of the argument. The argument is in radians
- **acos(number)** calculate the arccosine of the argument
- **asin(number)** calculate the arcsine of the argument
- **atan(number)** calculate the arctangent of the argument
- **atan2(number, number)** calculate angle between origin and point given by the arguments
- ***sqrt(number)** return the square root of the argument
- **pow(number. number)** return the first argument raised to the power of the second argument
- **abs(number)** return the absolute value of a argument
- **clamp(number, number, number)** clamp the first argument between the second (min) and third (max) arguments
- **lerp(number, number, number)** linearly interpolate a position between the first and second arguments
- **degToRad(number)** convert the argument in radians to degrees
- **radToDeg(number)** convert the argument in degrees to radians
- **random(number)** return a random number (decimal) between 0 and the supplied argument

> **(Read the full documentation for math functions)[docs/functions.md#math]

The following functions work on color values:

- **rgb(int, int, int)** convert color channels R, G and B to a color
- **rgba(int, int, int, number)** convert color channels R, G and B and an alpha value (0.0 - 1.0) to a color
- **alpha(int)** return the alpha value of a color as int (0 - 255)
- **red(int)** return the red channel of a color as int (0 - 255)
- **green(int)** return the green channel of a color as int (0 - 255)
- **blue(int)** return the blue channel of a color as int (0 - 255)
- **opacity(color, number)** change the opacity of a color and return it
- **darken(color, number)** darken a color towards black by an amount (0.0 - 1.0)
- **lighten(color, number)** lighten a color towards white by an amount (0.0 - 1.0) 
- **tint(color, color, number)** tint a color towards another color by an amount (0.0 - 1.0)
- **grayscale(color)** return the color converted to grayscale

> **(Read the full documentation for color functions)[docs/functions.md#color]

### User-defined function

Any unrecognised functions found within the jx file are assumed to be user-defined functions. The user is notified (via code) of the function name that was called, and it's arguments. The code can process this function and return the result back to the jx file.

Exactly how user-defined functions are implemented depends on the language. Here is an example using dart that implements a function called `wrapInArray`.

```dart
var parser = JxParser()
    ..options.strict()
    // Provide custom function handler
    ..onFunction = (String fn, List<dynamic> args) {
        switch (fn) {
        case 'wrapInArray':
            return [...args];
        default:
            return null;
        }
    };
var result = parser.parse('{ myArray: wrapInArray("a", 100, true); }');
```

> The official jx style guide recommends lower pascal case for function names, e.g. `myFunctionName(...)`

> consider using a (constant)[#constants] instead of a function if it takes no inputs and always returns the same fixed value

## Expressions

An expression is an equation or other sequence of values and operators that is processed to calculate a new value. Expressions in a jx file are processed when the jx file is parsed, and the resulting value is included in the output.

Some examples of expression are:

```js
[
    17 * (sin($angle) + pi); // Mathematical expression
    ['a', 'b'] + wrapInArray(getPeopleInTeam('team1')); // Array manipulation
    'party at ' + $place; // String manipulation
]
```

### Operators

The following operators are available to use in expressions.

```js
[
    // Mathematical operators

    -10, // Negative
    5 - 1, // subtraction
    3.12 + pi, // addition
    3 * 3, // multiplication
    6 / 2, // division
    2 ^ 4, // power (2 raised to the 4th power)
    7 * (2 + 4), // parenthesis for grouping
    16 % 3, // remainder after division

    // String operators

    'name = ' + 'jx',             // String concatenation

    // Array operators

    ['a', 'b'] + ['c', 'd'],      // Array concatenation
    ['a', 'b'] + 'c',             // Add element to array
    ['a', 'b', 'c'] - ['b', 'c'], // Removing multiple elements from array
    ['a', 'b', 'c'] - 'c',        // Remove element from array

    // Object operators

    {a:1, b:2} + {c:3, d:4},      // Object concatenation
    {a:1, b:2, c:3} - {b:0, c:0}, // Removing multiple elements from object
    {a:1, b:2, c:3} - ['b', 'c'], // Removing multiple elements from object
    {a:1, b:2, c:3} - 'b',        // Remove element from object
]
```
