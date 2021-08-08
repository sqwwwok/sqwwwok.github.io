---
title: Confusions about parseInt
categories:
  - Javascript
date: 2025-01-26 10:50:03
tags:
---

## Backgrounds

```js
parseInt(0.0000005)
// 5
```

## Explanations

​	`parseInt` accepts string as the first argument, represents the value to parse. If this argument is not a string, then it is converted to one using the `ToString` abstract operation.

​	In JavaScript, the number which has more than 6 zeros in front of the first non-zero digit will be displayed in scientific notation, specified by [ECMA ](https://tc39.es/ecma262/multipage/ecmascript-data-types-and-values.html#sec-numeric-types-number-tostring). That means , `0.00000005.toString() `will return `'5e-7'`

​	So, the code above can be explained as following:

```js
0.0000005.toString()
// '5e-7'

parseInt(5e-7)
// 5
parseInt('5e-7')
// 5
```

## Extra

What about postive numbers ?

```js
parseInt(5e7)
parseInt(50000000)
// both return 50000000

// 5e7 will not be displayed in scientific notation
5e7.toString()
// '50000000'

// '5e6' is string
parseInt('5e6')
// 5
```

## Tips

`parseInt` should *not* be used as a substitute for `Math.floor()`