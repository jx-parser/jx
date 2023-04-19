# jx file structure

> **[Back to menu](jx.md)**

The jx file format is a super-set of [JSON](https://www.json.org/json-en.html). Start by becoming familiar with JSON - most of the documentation assumes a good understanding of the JSON format.

A jx file is a text file with the extension `.jx`. 

## Root element

A jx file must have a single element at it's root. That element can be either an `object`, an `array` or an `expression`. The following are all valid jx files.

```js
// A single object at the root
{
    name: 'jx';
    implements: 'JSON';
}
```

```js
// A single array at the root
[
    'jx',
    'JSON',
]
```

```js
// A single expression or value at the root.
// Note: An expression evaluates to a single element.
sin(32) + 17
```

> Unlike JSON: jx can contain a single expression or value at it's root. JSON supports only an array or object.

## Version specifier

Although it is optional and currently unused, it is recommended to specify the format and version in a comment at the top of the file.

The full jx specification uses `@jx-version` and the jx-lite specification uses `@jx-lite-version`

_note_: A future version of jx may change the way the version is specified.

```js
// @jx-version: 1.0
{
    name: 'jx';
    implements: 'JSON';
}
```

or somewhere in the first block comment

```js
/*
 * @jx-lite-version: 1.0
 * @author: Projectitis
 */
{
    name: 'jx-lite';
    implements: 'JSON';
}
```

> Unlike JSON: jx can contain comments. Both single-line and block comments are supported
