---
title: How to replace && with ternary operators
categories:
    -   Others
---
In React Native, logical AND may cause crash as below.

[Conditional rendering in React Native may crash your app](https://www.koprowski.it/blog/conditional-rendering-react-native-text-crash/)

This babel plugin [here](https://gist.github.com/bogas04/d27ec339ff48758525bb4ca3136367cb) can replace all logical AND with ternary operators, with a little more configuration.

```jsx
// .babelrc.js
module.exports = function (api) {
    api.cache(true);

    const presets = [];
    const plugins = [
        './ternary-jsx.js',
// this two plugins below only parse but not transform code
        ['@babel/plugin-syntax-decorators', { decoratorsBeforeExport: true }],
        ['@babel/plugin-syntax-class-properties', { loose: true }],
    ];

    return {
        parserOpts: {
            plugins: ['jsx', 'typescript'],
        },
        presets,
        plugins,
        generatorOpts: {
            retainLines: true,
            compact: false,
            minified: false,
            concise: false,
        },
    };
};
```

[However, Babel will lose some code formatting in the process, as it works based on the AST.](https://stackoverflow.com/questions/77531610/how-to-prevent-babel-from-formatting-the-generated-code).


